原文链接： [Pragmatic Functional Programming](http://blog.cleancoder.com/uncle-bob/2017/07/11/PragmaticFunctionalProgramming.html) - _Robert C. Martin (Uncle Bob)_，2017-07  
译于 2018-10-19，基于[liuchengxu](https://www.jianshu.com/u/daf68451f175)的译文稿：[实用的函数式编程](https://www.jianshu.com/p/b14bdc4d1fd3)

<img src="clojure-logo.png" width="20%" align="right" >

## 🍎 译序

`Bob`大叔的短文，`FP`在软件开发优点上务实的思考，引导大家理解、学习和使用`FP`，文章后半篇还用`FP`语言`Clojure`简约演示了一番。在文末不忘呼吁学习`FP`，并推荐`Clojure`语言。

# 务实的函数式编程

函数式编程（`Functional Programming`/`FP`）的风潮讲真大概是从10年前开始。我们开始关注像`Scala`、`Clojure`和`F#`这样的语言。这个风潮并非只是『哇酷～一门新语言！』这样平平常常的热情。确实有些实在的原因在推动 —— 或者我们是这么想的。

摩尔定律告诉过我们，每隔18个月计算机的速度就会翻倍。这个定律从60年代到2000年都是有效的，但之后失效了。大家冷静想一下。时钟频率到达了3G HZ，进入了平台期。这已经触及了光速的物理极限，而在芯片表面上的信号传播速度限制住了计算速度的提升。

所以硬件设计师改变了策略。为了获得更大的吞吐量，在芯片上添加了更多的处理器（核心数）；同时为了给新加的核腾出空间，在芯片上去掉了很多缓存（`cacheing`）和流水线（`pipelining`）硬件。结果是，单个处理器较之前要慢一些；但是由于有了更多的处理器，吞吐量仍然是提升的。

> 【译注】：关于流水线（`pipeline`）参见[指令流水线（`instruction pipeline`） - zh.wikipedia.org](https://zh.wikipedia.org/wiki/%E6%8C%87%E4%BB%A4%E7%AE%A1%E7%B7%9A%E5%8C%96)。

8年前我有了第一台双核机器，两年后有了一台4核的机器。也就是说，核数开始进入了激增的时期。那时候我们都认识到，这将会以我们无法想象的方式影响软件开发。

一个对策就是学习`FP`。`FP`强烈不鼓励在变量（`variable`）初始化之后再改变其状态（`state`）。这对并发（`concurrency`）有着深刻影响。如果不能改变变量的状态，就不会有竞争条件（`race condition`）。如果不能更新变量的值（`value`），也就不会有并发更新（`concurrent update`）的问题。

当然了，这一直被认为是多核问题的解决方案。当核数激增，并发，甚至是**共时性**（`simultaneity`），都会成为一个大问题。`FP`可以说是提供了一种编程风格（`the programming style`），可以减轻当一个处理器中有1024核时所出现的问题。

所以所有人都开始学习`Clojure`、`Scala`、`F#`或是`Haskell`；因为大家相信冲锋号即将吹响，都想提前做好准备！

然而，这一天一直没有到来。六年前我有了一台4核的笔记本，比上一台多了两个核。而我的下一台笔记本估计还会是4核。我们又进入了另一个平台期？

> 说个题外话，昨晚我看了一部2007年的电影。女主角在用笔记本在一个时髦的浏览器里面浏览网页、使用`Google`、用翻盖手机收发短信。一切都是那么的熟悉。只不过都过时了 —— 我可以看出笔记本是老型号，浏览器是个老版本，而翻盖手机比起今天的智能手机就像个古董。尽管如此，这些变化却比不上从2000到2011年这段时间的变化那么翻天覆地。更是远比不上从1990年到2000年间的变化。难道我们正在见证计算机和软件技术发展的平台期吗？

所以，或许，`FP`并不是一个我们之前想的那样关键的技能。或许，我们将不会被那么多的核包围。或许，也不用去担心会有32768个核的芯片。或许，我们都可以放松一下，又回到以前那样去更新变量。

但是，我觉得这会是一个错误，一个严重的错误。我觉得这个错误会和滥用`goto`一样严重。我觉得这会和放弃动态派发（`dynamic dispatch`）一样危险。

> 【译注】：关于动态派发（`dynamic dispatch`）参见[动态调度（`dynamic dispatch`） - zh.wikipedia.org](https://zh.wikipedia.org/wiki/%E5%8A%A8%E6%80%81%E8%B0%83%E5%BA%A6)。

为什么呢？让我们回到`FP`起初引起我们兴趣的原因上 —— `FP`使得并发变得安全得多。如果你要搭建一个有很多线程或是进程的系统，使用`FP`会大大减少可能由竞争条件和并发更新而引发的问题。

还有其它理由吗？嗯～`FP`更易写、易读、易于测试和易于理解。听到这些，我能想象到，有些读者比如你已经掀桌子咆哮了。你尝试过`FP`，『有毛容易啊』是你的感受。`map`、`reduce`和递归 —— 尤其是**尾**递归，有哪个说得上容易？你说得没错，收到收到。但是，这其实只是个是否熟悉的问题。一旦你熟悉了这些概念以后（建立起熟悉的过程其实并不需要太长时间），编程就会变得**更容易得多**。

为什么会更容易呢？因为你不再需要跟踪系统的状态。由于变量的状态无法改变，所以系统的状态也就维持不变。不需要跟踪的不仅仅是系统，还有列表、集合、栈、队列等等通通都不需要跟踪状态，因为这些数据结构也无法改变。在`FP`语言中，当向一个栈`push`一个元素，得到是一个新的栈，并不会改变原来的栈。这意味着，像接抛球杂耍那样，程序员在空中需要同时控制好的球变少了；需要记忆的东西更少了；需要跟踪的东西更少了。因而代码的编写、阅读、理解和测试变得简单得多。

那么，你应该使用哪种`FP`语言呢？我最喜欢的是`Clojure`。因为`Clojure`难以置信的简单。它是`Lisp`的一个方言，`Lisp`是一个简单至美的语言。让我来展示一下吧：

在`Java`中的函数：`f(x)`；

转换成`Lisp`的函数就是简单地将第一个括号移到左边即可：`(f x)`。

现在，你已经学会了 95% 的`Lisp`和 90% 的`Clojure`。对这些语言而言，这些傻傻的小括号真就是全部的语法了。**难以置信**的简单。

你可能见过`Lisp`程序，不过不喜欢这些括号。也可能你不喜欢`CAR`、`CDR`和`CADR`。别担心。`Clojure`有着比`Lisp`更多的符号，所以括号相对少一些。`Clojure`用`first`、`rest`和`second`代替了`CAR`、`CDR`和`CADR`。此外，`Clojure`基于`JVM`，完全可以使用所有的`Java`库和任何其他你想要的`Java`框架和库。与`Java`互操作性快速而便捷。更好的是，`Clojure`能够使用`JVM`所有的面向对象功能。

『等一下！』你可能会说，『`FP`和面向对象是相互不兼容的！』谁告诉你的？胡说八道！在`FP`中，你的确无法改变一个对象的状态。但是那又怎样呢？就像`push`一个整数进栈后会返回一个新的栈，当调用一个方法调整一个对象的值时，返回的是一个新对象而不是改变原来的对象。一旦你习惯了这样的做法，处理起来是很容易的。

再回到面向对象。我觉得面向对象最有用的一个特性，在软件架构层面，是动态多态性（`dynamic polymorphism`）。`Clojure`提供了对`Java`动态多态性的完全的使用能力。举个例子解释起来可能最方便：

```clojure
(defprotocol Gateway
  (get-internal-episodes [this])
  (get-public-episodes [this]))
```

上面的代码定义了一个`JVM`的多态接口。在`Java`中，这个接口看起来可能像这样：

```java
public interface Gateway {
    List<Episode> getInternalEpisodes();
    List<Episode> getPublicEpisodes();
}
```

在`JVM`层面所生成的字节码是完全相同的。实际上，`Java`写的程序可以实现这个接口，就像这个接口是用`Java`写的一样。对等的，`Clojure`程序也可以实现一个`Java`写的接口。看起来大概这个样子：

```clojure
(deftype Gateway-imp [db]
  Gateway
  (get-internal-episodes [this]
    (internal-episodes db))

  (get-public-episodes [this]
    (public-episodes db)))
```

注意构造函数的`db`参数，以及在方法中是如何访问这个参数的。在上面的例子中，接口的实现只是简单地委托给了本地函数，并传入`db`参数。

（也许）最大的优点，来自于`Lisp`进而也是`Clojure`的，那就是 —— **[同像性](https://www.wikiwand.com/zh/%E5%90%8C%E5%83%8F%E6%80%A7)**（`Homoiconic`），是指代码本身就是程序能够操作的数据。这点不难看出。下面的代码：`(1 2 3)` 表示一个三个整数的列表。如果该列表的第一个元素变成了一个函数，也就是`(f 2 3)`，那么它就变成了一个函数调用。可见，在`Clojure`中所有的函数调用都是列表，而列表可以直接被代码操作。所以，一个程序也可以构造和执行其它的程序。

最后说一句，`FP`是重要的。你应该去学习。如果你还在纠结要用哪个语言来学`FP`，我推荐`Clojure`。
