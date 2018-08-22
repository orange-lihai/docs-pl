###### 高阶函数
高阶函数是至少满足下列一个条件的函数:
1. 接受一个或多个函数作为输入
2. 输出一个函数

~~~
// 看下 Function 的源码
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);

    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
        Objects.requireNonNull(before);
        return (V v) -> apply(before.apply(v));
    }

    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }

    static <T> Function<T, T> identity() {
        return t -> t;
    }
}

// 上面的 compose 和 andThen 都是高阶函数. 怎么用呢 ?
Function<String, Integer> strToInt = s -> s.length() + 1;
Function<Integer, Boolean> intToBool = (n) -> (n + 1) > 10;
Function<String, Boolean> strToBool = intToBool.compose(strToInt);

// 回想一下, 求导其实是一个高阶函数, 因为它映射一个函数到另一个函数
// 可以这么理解, 求导函数(x²) = 2·x
f(x) = x²;
f'(x) = 2·x 

// 如果按照这个定义, 上面的闭包举例也算是高阶函数
// 应该它满足条件 2
Function<String, Function<String, String>> fn = name -> {
  String prefix = name + ": ";
  final List<String> sayLogs = new ArrayList<>();
  return (s) -> {
    String _say = prefix + "Hello!";
    sayLogs.add(_say);
    return sayLogs.size() + " -> " + _say;
  };
};
~~~

###### Currying(库里化/柯里化)
返回另一个函数的高阶函数被称为Curry化的函数
从下面的例子可以看出, 柯里化 和 闭包 形影不离.
~~~
// 如果我需要一个函数, 三个类型为 A, B, C 的参数, 返回值类型为 D
// 之前我们说, 可以自定义 函数式接口
@FunctionalInterface
public interface MyFuncInterface<A, B, C, D> {
    D accept(A a, B b, C c);
}

// 其实, 我们也可以这样
Function<String, Function<Integer, Function<Boolean, Long>>> trioFunc = a -> b -> c -> {
  long g = -1;
  if (c) {
    g = a.length();
  } else {
    g = b ;
  }
  return g;
};
Function<Integer, Function<Boolean, Long>> biFunc2 = trioFunc.apply("lihai");
Function<Boolean, Long> func = biFunc2.apply(8);
long len = func.apply(true); // 5
~~~