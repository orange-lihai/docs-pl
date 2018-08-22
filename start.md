###### 什么是 lambda 表达式 ?>
> 简单来说, lambda 表达式是指编程语言中的 "匿名函数" 或 "匿名方法"
为什么要给函数或者方法省略名字呢? 主要针对一些 "比较简单, 并且只用一次" 的语句块, 提供比较友好的语法支持. 避免将它们封装成一个特别的并且只用一次的微型方法.

###### lambda 表达式有什么用 ? 
> lambda 表达式最直观的作用就是使得代码变得很简洁.
~~~ 
// 在 Java8 中, 下面的 map 方法里面传递的就是 "匿名函数". 
List<String> fruits = Arrays.asList("apple", "orange", "banana");
fruits = fruits.stream().map(m -> m + "!").collect(Collectors.toList());

// forEach 方法里面传递的是 "方法引用", 用来简写lambda表达式中已经存在的方法.
fruits.forEach(System.out::println); // apple! orange! banana!

// 其实也可以给 lambda 表达式先起个名字:
Function<String, Integer> f0 = s -> (null == s) ? -1 : s.length();
int len = f0.apply("abc");
System.out.println("length of string => " + len);   // length of string => 3
~~~
~~~
# 在 Python 中, 语法是像这样
Python 2.7.10 (default, May 23 2015, 09:44:00) [MSC v.1500 64 bit (AMD64)] on win32  
Type "help", "copyright", "credits" or "license" for more information. 
 
>>> lambda x: x + 2          # lambda 表达式的类型是 "function"  
<function <lambda> at 0x00000000024787B8>
>>>  (lambda x: x + 2)(5)    # 传参调用            
7                       
>>> f = lambda x: x + 2      # 给 lambda 表达式命名
>>> map(f, [1,3,5])          # 将命名的 lambda 表达式 f 作为参数传递给方法
[3, 5, 7]
>>> map(lambda x: x + 2, [1,3,5])    # 将未命名的 lambda 表达式作为参数传递给方法
[3, 5, 7]
~~~

###### lambda 表达式和 lambda 演算有什么关系 ?
> 打个可能不恰当的比方: 它们的关系像是原子弹和质能方程的关系. lambda 演算也可以说是一个最小的通用程序设计语言. 来源于数学家的一个数理逻辑形式系统, 现在已经是计算理论绕不过去的主题. 
> 有兴趣的话, 读一些这方面的科普.