###### 方法引用
为什么要加入这么一个新的语法呢 ?
在最开始, 我们说: lambda 表达式可以理解为 "匿名方法", 并且这些方法可以赋值给变量, 然后当做参数传递给其它的方法.
那么, 难道只能是 @FunctionalInterface 接口类型才能这么做吗 ?
定义在类里面的那些方法呢 ? 
它们也是方法, 让它们也升级为一等公民既合理也划算.

######  语法 [文档](https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html)

|  Kind  |  Example  |
| --- | --- |
|  Reference to a static method  |  ContainingClass::staticMethodName  |
|  Reference to an instance method of a particular object  |  containingObject::instanceMethodName  |
|  Reference to an instance method of an arbitrary object of a particular type  |  ContainingType::methodName  |
|  Reference to a constructor  |  ClassName::new  |


~~~
// Reference to a Static Method
// 第一类: 静态方法引用
// Stream 类,  void forEach(Consumer<? super T> action);
List<String> fruits = Arrays.asList("apple", "orange", "banana");
fruits.stream().forEach(m -> System.out.println(m));
fruits.forEach(System.out::println); 
// 或者
Consumer<String> consumer = System.out::println;
fruits.stream().forEach(consumer);

// Reference to an Instance Method of a Particular Object
// 第二类: 引用特定对象的实例方法
@Getter
@Setter
public class Student {
  public int age;
  public String name;
	
  public Student(int age, String name) {
    this.age = age;  this.name = name;
  }
  
  public String toStr(String name, Integer age) {
    return name + "~" + age;
  }
}

Student student = new Student(10000, "God");
BiFunction<String, Integer, String> biFunc = student::toStr;
String lh = biFunc.apply("lihai", 30);
System.out.println(lh); // lihai~30 / God~10000

 // Reference to an Instance Method of an Arbitrary Object of a Particular Type   
// 第三类: 对特定类型的任意对象的实例方法的引用
fruits.sort(String::compareTo);
List<String> names = students.stream().map(Student::getName).collect(Collectors.toList());
// 或者
Function<Student, String> f = Student::getName;
String oldName = f.apply(new Student(88, "old"));
System.out.println(oldName);  // old

// 第四类: 构造方法
@Getter
@Setter
public class StudentScores {
  public int age;
  public String name;
  public int score;

  public StudentScores(Student student) {
    this.age = student.getAge(); 
    this.name = student.getName();
    this.score = -1;
  }
}

List<StudentScores> studentScoresInit =
  students.stream().map(StudentScores::new).collect(Collectors.toList());


~~~
