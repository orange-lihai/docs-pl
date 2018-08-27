###### Haskell 是什么 ?
> 它不过是又一门编程语言而已. 不过它有一些特立独行的特征, 很值得程序员们学习和了解它. 
> * 它是一门纯粹函数式编程语言(purely functional programming language)
> * 它是惰性(lazy)的.
> * 它是静态类型 (statically typed) 的.

###### 从哪里开始呢 ?
* 首先, 当然是找到它的[官方站点](https://www.haskell.org/). 
* 在[下载页](https://www.haskell.org/downloads) 下载 Haskell Platform (包含了 Stack 和 Cabal) 并安装它们.
* 在Haskell Platform的安装目录(比如: C:\Program Files\Haskell Platform\8.4.3\bin)找到 ghci.exe 并运行它:
~~~
GHCi, version 7.10.2: http://www.haskell.org/ghc/  :? for help
Prelude> 1 + 1
2
Prelude> "Hello, World!"
"Hello, World!"
Prelude>
~~~

###### 可能有用的推荐
* 在 Windows 7+ 或者 OS X 平台上, 推荐 Intellij Idea + HaskForce 插件
* 如果你在 Linux 或者 Unix 平台上工作, 我不相信你需要别人的推荐
* 网络不好的话可以参考使用: [Hackage 源使用帮助](http://mirrors.ustc.edu.cn/help/hackage.html)