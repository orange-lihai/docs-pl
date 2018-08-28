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
str :: [Char]
str = "String Constant ..."
~~~


###### List 入门
~~~

~~~