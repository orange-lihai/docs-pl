###### 先了解下 java.util.Optional\<T\> 类
直接看下JDK里面的源代码, 很好理解. 它就是对一个可能不存在的值, 做了一层封装. 真的这么简单 ?
为什么 Java8 里面加入了 Optional ?

>“我把Null引用称为自己的十亿美元错误。它的发明是在1965年，那时我用一个面向对象语言( ALGOL W )设计了第一个全面的引用类型系统。我的目的是确保所有引用的使用都是绝对安全的，编译器会自动进行检查。但是我未能抵御住诱惑，加入了Null引用，仅仅是因为实现起来非常容易。它导致了数不清的错误、漏洞和系统崩溃，可能在之后40年中造成了十亿美元的损失。近年来，大家开始使用各种程序分析程序，比如微软的 PREfix 和 PREfast 来检查引用，如果存在为非null的风险时就提出警告。最新的一些程序设计语言比如Spec#已经引入了非Null引用的声明。这正是我在1965年拒绝的解决方案。”

了解一位计算机的传奇人物 [东尼·霍尔](https://zh.wikipedia.org/wiki/%E6%9D%B1%E5%B0%BC%C2%B7%E9%9C%8D%E7%88%BE)
有兴趣的话, 看下这个演讲视频: [Null References: The Billion Dollar Mistake](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare)

###### 具体怎么使用呢 ?
~~~
// 先看一段代码, 这是我们平常最常见的 null 处理.
String s = getSomeString();
if (null == s) {
  return -1;
} else {
  return s.length();
}
// 如果改用 Optional, 看上去好像没有什么改观 ?
Optional<String> opt = getSomeString();
if(opt.isPresent()) {
  return opt.get().length();
} else {
  return -1;
}
// 其它用法
// 如果存在, 做点事情
opt.ifPresent(m -> System.out.println(m));
// 存在返回, 不存在返回默认值.
return opt.orElse("Unknown");
// 存在返回, 不存在由另外的调用产生.
opt.orElseGet(() -> { String t = "Sorry, "; return t + "UnKnown"; });
// 连续的 null 检查
Optional<Student> stuOpt = Optional.ofNullable(new Student(5, "S"));
return stuOpt.map(s -> s.getName())
             .map(n -> n.toUpperCase())
             .orElse("UnKnow");

// 上面最后一种情况, 如果没有 Optional 和 lambda 表达式, 那会像这样
if (null != student) {
  String name = student.getName();
  if (null != name) {
    return name.toUpperCase();
  } else {
    return null;
  }
} else {
  return null;
}
// 当然, 还是可以更简单点的
if (null != student && null != student.getName()) {
  String name = student.getName();
  return name.toUpperCase();
} else {
  return null;
}
// 看上去还可以, 但是明显不如 lambda 表达式流畅.
// 如果有更多层次属性的null检查, 第一个 if 里面将会很长.
~~~

###### 其它语言的改进
关于 NPE, 许多语言都做出了改进.
~~~
// Haskell
data Maybe a = Just a | Nothing
~~~