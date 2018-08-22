###### 注解: 元数据, 即一种描述数据的数据.
~~~
// 看看 Spring 的 @Service 注解是怎么定义的
@Target({ElementType.TYPE})  // 决定注解可以标记在类上面
@Retention(RetentionPolicy.RUNTIME)  // 决定运行时可读
@Documented
@Component  // 注解也是一个Java类, 也可以被其它注解标记. 
public @interface Service {

	/**
	 * The value may indicate a suggestion for a logical component name,
	 * to be turned into a Spring bean in case of an autodetected component.
	 * @return the suggested component name, if any
	 */
	String value() default "";

}

// 注解的使用方式也很好理解. 
public enum RetentionPolicy {
    /**
     * Annotations are to be discarded by the compiler.
     */
    SOURCE,

    /**
     * Annotations are to be recorded in the class file by the compiler
     * but need not be retained by the VM at run time.  This is the default
     * behavior.
     */
    CLASS,

    /**
     * Annotations are to be recorded in the class file by the compiler and
     * retained by the VM at run time, so they may be read reflectively.
     *
     * @see java.lang.reflect.AnnotatedElement
     */
    RUNTIME
}
~~~

###### 考虑一些工作中常见的注解场景.
~~~
@RestController
public class ExtendServer {
  @Autowired private RedisTemplate redisTemplate;
  @Autowired private CrmStudentDao crmStudentDao;
  @Autowired private ExtendService extendService;

  @RequestMapping(value = "/extend/1/test/student/{studentId}") @ExtendTimerLogs(valid = true)
  @ExtendDateSource(name = "master")
  public Object reservation(@PathVariable("studentId") Integer studentId) {
    return crmStudentDao.selectByPrimaryKey(studentId);
  }

  @RequestMapping(value = "/extend/2/test/student/{studentId}")
  @ExtendDateSource(name = "slave")
  @ExtendTimerLogs(valid = true)
  public Object reservation2(@PathVariable("studentId") Integer studentId) {
    return extendService.doReservation(studentId);
  }

  @RequestMapping(value = "/extend/3/test/student/{studentId}")
  @ExtendTimerLogs(valid = true)
  public Object reservation3(@PathVariable("studentId") Integer studentId) {
    return extendService.doReservation(studentId);
  }
}

@Service
public class ExtendService {
  @Autowired private CrmStudentDao crmStudentDao;

  @ExtendDateSource(name = "master")
  @ExtendTimerLogs(valid = true)
  public Object doReservation(Integer studentId) {
    return crmStudentDao.selectByPrimaryKey(studentId);
  }
}

// 定义的的注解
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME) public @interface ExtendDateSource {
  String name() default "slave";
}

@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME) public @interface ExtendTimerLogs {
  boolean valid() default false;
}

// 实现 AOP 的一种思路一种思路
@Aspect
@Component
public class ExtendAspect {
  @Autowired private @Qualifier("masterDataSource") DruidDataSource masterDataSource;
  @Autowired private @Qualifier("slaveDataSource") DruidDataSource slaveDataSource;
  private static final String connKey = "Extend-Conn";
  private static final String topKey = "Extend-Top";
  private static final String methodKey = "Extend-Method";

  @Around("execution(* com.xdf.hills.crmbase.rest.server.extend.*.*(..))")
  public Object around(ProceedingJoinPoint pjp) throws Throwable {
    // 判断是否进入有 @Extend
    Method m = GenericUtil.getMethod(pjp, null);
    long check = Arrays.stream((m.getDeclaredAnnotations()))
                       .filter(f -> f instanceof ExtendTimerLogs
                                    || f instanceof ExtendDateSource
                       ).count();
    if (check <= 0) {
      return pjp.proceed();
    }
    Stack<String> stack = ContextHolder.get2(methodKey, new Stack<String>());
    stack.push(m.getName());
    ContextHolder.set(methodKey, stack);

    Stack<Integer> stack2 = ContextHolder.get2(topKey, new Stack<Integer>());
    stack2.push(1);
    ContextHolder.set(topKey, stack2);

    // 判断是否是最外层的 @Extend
    if (stack.size() == 1) {
      ContextHolder.set(connKey, null);
    }

    // wrapper
    Supplier f = () -> original(pjp);
    Annotation[] ans = m.getDeclaredAnnotations();
    for (Annotation a : ans) {
      if (a instanceof ExtendTimerLogs && ((ExtendTimerLogs)a).valid()) {
        f = wrapperWith(f, (ExtendTimerLogs)a);
      }
      if (a instanceof ExtendDateSource) {
        f = chooseDB(f, (ExtendDateSource)a);
      }
    }
    // 执行
    Object o = f.get();

    // 判断是否是最外层的 @Extend
    if (stack.size() == 1) {
      Connection conn = (Connection) ContextHolder.get(connKey);
      Optional<Connection> optConn = Optional.ofNullable(conn);
      optConn.ifPresent((oo) -> {
        try {
          if (!oo.isReadOnly()) { oo.commit(); }
          if (!oo.isClosed()) { oo.close(); }
        } catch (Exception ex) {
          ex.printStackTrace();
        }
      });
      ContextHolder.set(topKey, null);
      ContextHolder.set(connKey, null);
    }

    stack = ContextHolder.get2(methodKey, new Stack<String>());
    if (null != stack && stack.size() > 0) { stack.pop(); }
    ContextHolder.set(methodKey, stack);

    stack2 = ContextHolder.get2(topKey, new Stack<Integer>());
    if (null != stack2 && stack2.size() > 0) { stack2.pop(); }
    ContextHolder.set(topKey, stack2);

    // 返回
    return o;

  }

  private Supplier wrapperWith(Supplier supplier, ExtendTimerLogs a) {
    return () -> {
      System.out.println(StringUtils.repeat(" ", ContextHolder.get2(topKey, new Stack<Integer>()).size())
                         + ContextHolder.get2(methodKey, new Stack<String>()).peek()
                         + " -> ExtendTimerLogs: begin ... ");
      Object o = supplier.get();
      System.out.println(StringUtils.repeat(" ", ContextHolder.get2(topKey, new Stack<Integer>()).size())
                         + ContextHolder.get2(methodKey, new Stack<String>()).peek()
                         + " -> ExtendTimerLogs: done ");
      return o;
    };
  }

  private Supplier chooseDB(Supplier supplier, ExtendDateSource a) {
    return () -> {
      Connection conn = (Connection) ContextHolder.get(connKey);
      String newName =  a.name();
      try {
        if (null == conn) {
          if ("master".equals(newName)) {
            conn = masterDataSource.getPooledConnection().getConnection();
            conn.setAutoCommit(false);
            conn.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);
          }
          if ("slave".equals(newName)) {
            conn = slaveDataSource.getPooledConnection().getConnection();
            conn.setReadOnly(true);
          }
          System.out.println(StringUtils.repeat(" ", ContextHolder.get2(topKey, new Stack<Integer>()).size())
                             + ContextHolder.get2(methodKey, new Stack<String>()).peek()
                             + " -> ExtendDateSource: begin ... ");
          ContextHolder.set(connKey, conn);
          System.out.println(StringUtils.repeat(" ", ContextHolder.get2(topKey, new Stack<Integer>()).size())
                             + ContextHolder.get2(methodKey, new Stack<String>()).peek()
                             + " -> ExtendDateSource: done ");
        }
        return supplier.get();
      } catch (Exception ex) {
        return null;
      }
    };
  }

  // 原来的方法
  private Object original(ProceedingJoinPoint pjp) {
    try {

      Object o = pjp.proceed();
      return o;
    } catch (Throwable throwable) {
      throwable.printStackTrace();
      return null;
    }
  }

}
~~~