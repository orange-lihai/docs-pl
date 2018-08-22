###### 可以用 lambda 表达式替换匿名内部类吗 ?
> 我的理解: 用lambda表达式确实可以完全代替匿名内部类. 但是有些匿名内部类里面实现很复杂, 如果用lambda表达式替换的话, 反而会导致业务结构不紧凑, 不清晰.

~~~
// 怎么理解 map 方法里面的 lambda 表达式呢 ?
List<String> fruits = Arrays.asList("apple", "orange", "banana");
fruits = fruits.stream()
                     .map(m -> m + "!")
                     .map(m -> m + "!!")
                     /* 这里还可以有更多的 lambda 表达式 */
                     .collect(Collectors.toList());

// 写成匿名内部类的使用方式, 在复杂情况下, 更紧凑.
// 尤其是 MyInnerClass 是已经存在的类, 只需要简单扩展或者重写某些方法.
(new SomeClass()).doWithInnerClass(new MyInnerClass() {
  @Override public String stepOne(String s) {
    return s + "!";
  }
  @Override public String stepTwo(String s) {
    return s + "!!";
  }
  /* ...  这里也许还有很多业务方法 ... */
});
~~~