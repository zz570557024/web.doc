# 函数式编程

函数式编程
（Functional programming）

1. 与面向对象编程（Object-oriented programming）和过程式编程（Procedural programming）并列的编程范式
2. 最主要的特征是，函数是第一等公民。
3. 强调将计算过程分解成可服用的函数，典型例子map方法和reduce方法组合而成MapReduce算法
4. 只有纯的，没有副作用的函数才是合格的函数

**范畴论**
函数式编程的起源，是一门叫做范畴论（Category Theory）的数学分支。
什么是范畴呢？
"范畴就是使用箭头连接的物体。"（In mathematics, a category is an algebraic structure that comprises "objects" that are linked by "arrows". ）
彼此之间存在某种关系的概念、事物、对象等等，都构成"范畴"。
箭头表示范畴成员之间的关系，正式的名称叫做"态射"（morphism）。范畴论认为，同一个范畴的所有成员，就是不同状态的"变形"（transformation）。通过"态射"，一个成员可以变形成另一个成员。

虽然面向对象编程（Object-oriented programing）主导着业界，但很明显这种范式在 JavaScript 里非常笨拙，用起来就像在高速公路上露营或者穿着橡胶套鞋跳踢踏舞一样。我们还发明了各种变通方法来应对忘记调用 new 关键字后的怪异行为，私有成员只能通过闭包（closure）才能实现，等等。对大多数人来说，函数式编程看起来更加自然。

强类型的函数式语言毫无疑问将会成为本书所示范式的最佳试验场。JavaScript 是我们学习这种范式的一种手段，将它应用于什么地方则完全取决于你自己。幸运的是，所有的接口都是数学的，因而也是普适的。最终你会发现你习惯了 swiftz、scalaz、haskell 和 purescript，以及其他各种数学偏向的语言。
通用的编程原则：

> DRY（不要重复自己，don't repeat yourself）

> 高内聚低耦合（loose coupling high cohesion）

> YAGNI （你不会用到它的，ya ain't gonna need it）

> 最小意外原则（Principle of least surprise）

> 单一责任（single responsibility）

## 高阶函数

高阶函数应该是指：

1. 可以接受函数作为输入参数
2. return 一个函数

函数 callMe 被以参数的形式将一段可被执行的代码块的引用传递给另一个函数，callMe 就被叫做一个回调函数。
如果 callMe 被省略了，这个函数没有名字，就是一个匿名函数。同时它是一个回调，所以又叫匿名回调

## 闭包

JS闭包（JavaScript closure）
Wikipedia对闭包的定义是这样的：
In computer science, a closure is a function together with a referencing environment for the nonlocal names (free variables) of that function.
从技术上来讲，在JS中，每个function都是闭包，因为它总是能访问在它外部定义的数据。
Since scope-defining construction in Javascript is a function, not a code block like in many other languages, what we usually mean by closure in Javascript is a fuction working with nonlocal variables defined in already executed surrounding function.
闭包经常用于创建含有隐藏数据的函数（但并不总是这样）。

闭包的总结如下：

1. 闭包是一种设计原则，它通过分析上下文，来简化用户的调用，让用户在不知晓的情况下，达到他的目的；
2. 网上主流的对闭包剖析的文章实际上是和闭包原则反向而驰的，如果需要知道闭包细节才能用好的话，这个闭包是设计失败的；
3. 尽量少学习。

把闭包从一个语法机制提升为一种设计原则：
闭包是从用户角度考虑的一种设计概念，它基于对上下文的分析，把龌龊的事情、复杂的事情和外部环境交互的事情都自己做了，留给用户一个很自然的接口。

各种专业文献上的"闭包"（closure）定义非常抽象，很难看懂。我的理解是，闭包就是能够读取其他函数内部变量的函数。
由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。
所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。
它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。
f1是f2的父函数，而f2被赋给了一个全局变量，这导致f2始终在内存中，而f2的存在依赖于f1，因此f1也始终在内存中，不会在调用结束后，被垃圾回收机制（garbage collection）回收。

**使用闭包的注意点**

1. 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
2. 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

### 语法糖

单的说，语法糖就是一种便捷写法。
例如：
```bash
input.map(item => item + 1);
# 他表示的意思是
input.map(function (item) {
  return item + 1;
});
```
通过例子你可以看出来，语法糖的使用其实就是让我们的写的代码更简单，看起来也更容易理解。
箭头函数的初衷是为了解决this问题吧。。。。也不全是单纯的语法糖。。。。

这里有一个地方需要注意，函数内部声明变量的时候，一定要使用var命令。如果不用的话，你实际上声明了一个全局变量！
出于种种原因，我们有时候需要得到函数内的局部变量。但是，前面已经说过了，正常情况下，这是办不到的，只有通过变通方法才能实现。
那就是在函数的内部，再定义一个函数。
Javascript语言特有的"链式作用域"结构（chain scope），子对象会一级一级地向上寻找所有父对象的变量。所以，父对象的所有变量，对子对象都是可见的，反之则不成立。
既然f2可以读取f1中的局部变量，那么只要把f2作为返回值，我们不就可以在f1外部读取它的内部变量了吗！

