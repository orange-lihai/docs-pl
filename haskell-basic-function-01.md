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
List 是非常常用的数据结构. 把它想象成一个容器, 它可以存储多个类型相同的元素.
~~~
-- 创建一个空的 List
listOfEmpty :: [a]
listOfEmpty = []

-- 创建一个 Integer 类型的 List
listOfInt = [1,2,3]
-- 或者像下面(实际上 [1,2,3] 只是 1:2:3:[] 的语法糖):
listOfInt = 1:2:3:[]
-- 所以, 可以用 : 符号在 List 的头部插入一个元素
0:listOfInt
-- 想在 list 尾部连接另外一个 List 可以用 ++ 运算符
listOfInt ++ [7,8,9]
-- 按下标索引取 List 中的元素, 下标从 0 开始
listOfInt !! 1

-- 还可以用 Range 创建一个 List
[1..20]
['A'..'Z']
[2,4..30]

-- 可以得到无限长的 List, 比如: [1..]
-- 不要尝试运行它, 而是配合其它操作, 比如:
take 10 [1..] 

-- 如果 List 中的元素可以比较大小, 那么可以比较这些 List 的大小
-- 首先比较第一个元素, 如果相等, 则比较它们的第二个元素, 依次类推
[9,8,7] > [9,8,6]
"DE" > "ABC"

-- 一个 Char 类型的 List, 其实就是一个 String 类型的值.
-- 所以可以用操作 List 的所有函数操作 String
-- 比如:
hello = "Hello"
Preload> :t hello
hello :: [Char]
Preload> 'A':hello
"AHello"
Preload> 

-- 列举一些操作或者创建 List 的常用函数(看源码, 把它们自己实现一遍)
-- head , tail , last , init , length , null , reverse , take 
-- maxinum , sum , elem 
-- cycle , repeat , replicate 
~~~

###### List Comprehension (列表推导式)
~~~
-- Haskell 的 List Comprehension 和数学上的集合定义非常相似
-- 只需要一个例子就能明白
Preload> [x * 2 | x <- [1..100], x >= 50, x < 60]
[100,102,104,106,108,110,112,114,116,118]
Preload>
~~~

###### 常用数据结构 Tuple
Tupe 和 List 很像, 它也是能存入多个值的容器. 但是它存入的元素的数量是固定的, 不过这些元素的类型可以不相同.
~~~
-- 创建一个 Tuple
Prelude> let t = ("Peter", 30)
Prelude> :t t
t :: Num b => ([Char], b)
Prelude>

-- 列举一些操作或者生成 Tuple 的函数(看源码, 把它们自己实现一遍)
-- fst , snd , zip , 
~~~