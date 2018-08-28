###### 先看一些简单的函数示例
~~~
-- 函数 doubleMe 接受一个 Num 类型的参数 a, 返回值类型也是 Num
-- 第一行是函数的声明(可以不写, 但是强烈建议写上它)
--     :: 符号前面的 是函数名称
--     (Num a) 是类型约束, 表示参数 a 的类型必须是 Num
--     => 符号右边的是函数的参数列表 和 返回值(最后一个)
-- 第二行是函数的具体实现
--     = 符号左边是函数名称和形式参数
--     = 符号右边是函数体
-- 调用方式:  doubleMe 5
-- Preload> doubleMe 5
-- 10
doubleMe :: (Num a) => a -> a
doubleMe x = x + x

-- 函数调用另一个函数的示例
doubleUs :: (Num a) => a -> a -> a
doubleUs x y = doubleMe x + doubleMe y

-- if 语句在 Haskell 中是一个表达式, 也就是说它不是一个语法结构
-- 所以, else 分支是不能省略的. 因为表达式必须有返回值.
doubleIf :: (Num a, Ord a) => a -> a
doubleIf x = (if x > 100 then x + 1 else x * 2) + 1

-- 没有参数的函数, 也可以把它看做 "常量定义"
-- 这种情况下, 第一行声明一般不需要写. 因为 Haskell 有类型推导.
str :: [Char]
str = "String Constant ..."
~~~


###### 常用数据结构 List
~~~
-- 一个空的 List
listOfEmpty :: [a]
listOfEmpty = []

-- 一个 Integer 类型的 List
listOfInt = [1,2,3]
-- 或者像下面(实际上 [1,2,3] 只是 1:2:3:[] 的语法糖):
listOfInt = 1:2:3:[]
-- 所以, 可以用 : 符号在 list 的头部插入一个元素
0:listOfInt
-- 想在 list 尾部连接另外一个 list 可以用 ++ 运算符
listOfInt ++ [7,8,9]

-- 两个 List 可以比较大小

-- 一个 Char 类型的 List, 其实就是一个 String 类型的值.
-- 所以可以用操作 List 的所有函数操作 String
-- 比如:
hello = "Hello"
Preload> :t hello
hello :: [Char]
Preload> 'A':hello
"AHello"
Preload> 
~~~

###### 常用数据结构 Tuple
~~~

~~~