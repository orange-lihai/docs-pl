###### 首先看看 java.util.function 包里面有什么 ?
> java.util.function 包里面预定义了一些常见的函数式接口, 很方便.
> 它们大致分类下面几中
* 最通用函数式接口: Function\<T, R\>, BiFunction\<T, U, R\>
* 消费者, 没有返回值. 比如: Consumer\<T\>, BiConsumer\<T, U\>
* 供应商, 没有参数. 比如: Supplier\<T\>
* 谓词, 返回 boolean. 比如: Predicate\<T\>, BiPredicate\<T, U\>
* 还有一些封装好的 XXXOperator, 比如: LongBinaryOperator, 接受两个long类型的参数, 返回值类型也是long.
* 下面一个不完整的表格


|  返回值(行)/参数(列)  |  无  | 一个 | 两个 | int | double |
| --- | --- | --- | --- | ---  | ---  | --- |
|  无  |  -  |  Consumer\<T>  | BiConsumer<T, U> |  IntConsumer |  DoubleConsumer  |
|  一个  |  Supplier\<T>  |  Function<T, R>  | BiFunction<T, U, R> |  IntFunction\<R> |  DoubleFunction\<R>  |
| boolean | BooleanSupplier | Predicate\<T> | BiPredicate<T, U> |  IntPredicate |  DoublePredicate  |
| int | IntSupplier | ToIntFunction\<T> | - |  -  |  -  |
| double | BooleanSupplier | ToDoubleFunction\<T> | - |  - |  -  |

###### 大于2个参数的函数式接口怎么办呢 ?
~~~
// 当然是自己定义了.
@FunctionalInterface
public interface MyFuncInterface<A, B, C, D> {
    D accept(A a, B b, C c);
}
~~~

###### 看一些和 Steams API 组合使用的例子
~~~
// lambda表达式和Steams API几乎是形影不离
// 先给定一个非空的 Student 列表
List<Student> students = studentService.queryStudents(cond);

// 0, 过滤掉 students 里面为 null 的元素
students = students.stream().filter(f -> null != f).collect(Collectors.toList());

// 1, 将 students 里面的每个学生, 年龄加上 1
students.forEach(e -> e.setAge(e.getAge() + 1));

// 2, 筛选 students 里面英文名不为空的元素, 将英文名变大写, 并排序返回这些英文名.
List<String> names = students.stream()
                             .filter(f -> null != f)
                             .filter(f -> null != f.getName())
                             .map(m -> m.getName().toUpperCase())
                             .sorted()
                             .collect(Collectors.toList());
// 3, 将 students 按年龄分组
Map<Integer, List<Student>> groupByAges = 
        students.stream().collect(Collectors.groupingBy(e -> e.getAge()));

// 4, 将 students 按年龄分组, 并只取姓名最大的那个.
// 类似SQL: SELECT age, MAX(name) FROM students GROUP BY age
Map<Integer, String> maxName = students.stream().collect(Collectors.toMap(
        e -> e.getAge(), 
        e -> e.getName(),
        (u1, u2) -> u1.compareTo(u2) >= 0 ? u1 : u2));
~~~
