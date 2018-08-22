###### 什么是闭包 ?
闭包本身是一个晦涩难懂的概念. 具体的解释 [参考](https://zh.wikipedia.org/wiki/%E9%97%AD%E5%8C%85_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6))
> 闭包（英语：Closure），又称词法闭包（Lexical Closure）或函数闭包（function closures），是引用了自由变量的函数。

###### 简单的闭包举例
~~~
// JS 中的闭包
var say = function(name) {
	var prefix = name + ": ";  
    return function(str) {
    	return prefix + str;
    }
}
var lihaiSay = say("lihai");
lihaiSay("Hello!") // "lihai:  Hello!"

var xiaoLongSay = say("xiao long");
xiaoLongSay("Hi!") // "xiao long:  Hi!"

// Java8 中的闭包
Function<String, Function<String, String>> fn = name -> {
  String prefix = name + ": ";
  // sayLogs 也可以放在 Function 定义外边
  final List<String> sayLogs = new ArrayList<>(); 
  return (s) -> {
    String _say = prefix + "Hello!";
    sayLogs.add(_say);
    return sayLogs.size() + " -> " + _say;
  };
};

Function<String, String> lihaiSay = fn.apply("lihai");
System.out.println(lihaiSay.apply("One!"));
System.out.println(lihaiSay.apply("Two!"));

// 那么有一种方法引用, 是不是也可以理解为闭包 ???
Student student = new Student(10000, "God");
BiFunction<String, Integer, String> biFunc = student::toStr;

~~~
