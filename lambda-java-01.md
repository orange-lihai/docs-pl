###### 如何形象的理解 lambda 表达式 ?
* 在Java中, 给一个变量赋值:  
~~~
int a = 5 + 6;
String s = "Hello, World!";
boolean b = s.length() > a;
~~~
* 那么如何给一段代码赋值 ?
~~~
// 伪代码像是下面这样, 这在 Java8 以前是做不到的.
// 等于号右边小括号里面的是这段代码的参数.
int a = 5 + 6;
CodeType code = (int a) -> {
    String s = "Hello, World!";
    boolean b = s.length() > a;
    return b;
}
boolean b = code(a);  // 调用这段代码

// 将一段代码组合在一起效果, 那不就是"方法"吗 ?
public boolean doSomething(int a) {
    String s = "Hello, World!";
    boolean b = s.length() > a;
    return b;
}
// 但是, 上面的代码在Java里面有一个明显的限制:
// 那就是"方法"并不能做为参数传递给其它的方法, 因为"方法"必须属于某个类.
// 只能像是这样调用:
(new AClass()).doSomething();

// 欠债还钱, Java8 增加了一个 Function 接口, 上面的代码块赋值可以实现了:
Function<Integer, Boolean> f = (a) -> {
	String s = "Hello, World!";
    boolean b = s.length() > a;
    return b;    
};
int a = 5 + 6;
f.apply(a);    //  调用这段代码块.
// f 也可以作为参数传递个其它的方法
Stream.of(10, 13, 15).map(f).collect(Collectors.toList());
~~~

* 那么, 如果仅仅从形式上看, lambda 表达式可以看作是一个可以赋值给变量, 可以作为参数传递给其它"方法"的代码块, 它改变了Java代码里面"方法"被禁锢在类里面的限制.  
