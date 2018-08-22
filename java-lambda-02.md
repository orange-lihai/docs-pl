###### 给Java升级的科学家们的权衡
* 折中的选择
~~~
// 函数式接口(Functional Interface)是 Java8 中对一类特殊类型的接口的称呼. 
// 这类接口只定义了唯一的抽象方法的接口(除了隐含的Object对象的公共方法). 
// 所以她叫做SAM类型的接口(Single Abstract Method).

// 为什么是这么设计的呢 ?
// 再看下 lambda 表达式的语法
Function<String, Integer> f = (s) -> { return (null == s) ? -1 : s.length(); };
// 上面的箭头表达式 (a, b) -> { ... ... } ,  是一种以前在Java中并不存在的类型, 可以把它叫做 "箭头类型". 
// 如果单独加一个 "箭头类型", 那必然要破坏既有的Java类型系统(interface, class, method), 并且要改造成千上万的类库.
// 权衡利弊, 因此最终还是利用SAM接口作为 Lambda表达式的目标类型.
~~~

* 函数式接口的语法
  * 接口只能包含1个自定义的抽象方法
  * 接口可以包含多个Object类里面的public方法
  * 接口可以包含多个default方法
  * 接口可以包含多个static方法
 
* 举例说明
![Function](images/ks_function.png)
![Comparator](images/ks_comparator.png)

* 比较观察2个接口从 Java6 升级到 Java8 的变化
~~~ 
// 比较接口 java.util.Comparator
// 在 Java6 中
public interface Comparator<T> {
  int compare(T var1, T var2);

  boolean equals(Object var1);
}
// 在 Java8 中
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);

    boolean equals(Object obj);

    default Comparator<T> reversed() {
        return Collections.reverseOrder(this);
    }
    // ... other default methods ...
   
    public static <T extends Comparable<? super T>> Comparator<T> reverseOrder() {
        return Collections.reverseOrder();
    }
	// ... other static methods ...
}


// 比较接口 java.lang.Runnable
// 在 Java6 中
public interface Runnable {
    public abstract void run();
}
// 在 Java8 中
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}

~~~

* 从接口及其实例化的角度理解lambda表达式
~~~
// 怎么理解 map 方法里面的 lambda 表达式呢 ?
List<String> fruits = Arrays.asList("apple", "orange", "banana");
fruits = fruits.stream().map(m -> m + "!").collect(Collectors.toList());

// 写成匿名内部类的使用方式.
fruits = fruits.stream().map(new Function<String, String>() {
  @Override public String apply(String s) {
    return s + "!";
  }
}).collect(Collectors.toList());

// 比较这两种写法, 可以有个合理的猜测:
// 在调用lambda表达式的时候, 其实是实例化了一个函数式接口. 
// 然后调用那个唯一的自定义的抽象方法.
~~~