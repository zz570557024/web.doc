# 前端测试

## ESLint

ESLint最初是由Nicholas C. Zakas 于2013年6月创建的开源项目。它的目标是提供一个插件化的javascript代码检测工具。
代码检查是一种静态的分析，常用于寻找有问题的模式或者代码，并且不依赖于具体的编码风格。对大多数编程语言来说都会有代码检查，一般来说编译程序会内置检查工具。

JavaScript 是一个动态的弱类型语言，在开发中比较容易出错。因为没有编译程序，为了寻找 JavaScript 代码错误通常需要在执行过程中不断调适。像 ESLint 这样的可以让程序员在编码的过程中发现问题而不是在执行的过程中。

ESLint 使用 Node.js 编写，这样既可以有一个快速的运行环境的同时也便于安装。

## Karma

Karma是一个基于Node.js的JavaScript测试执行过程管理工具（Test Runner）。该工具可用于测试所有主流Web浏览器，也可集成到CI（Continuous integration）工具，也可和其他代码编辑器一起使用。这个测试工具的一个强大特性就是，它可以监控(Watch)文件的变化，然后自行执行，通过console.log显示测试结果。

## Nightwatch

Nightwatch是一套新近问世的基于Node.js的验收测试框架，使用Selenium WebDriver API以将Web应用测试自动化。它提供了简单的语法，支持使用JavaScript和CSS选择器，来编写运行在Selenium服务器上的端到端测试

**nightwatch的API分为四个部分**
>Expect
BDD（行为驱动测试）风格的接口

>Assert
有两套同样方法的方法库，assert如果断言失败则退出整个测试用例所有步，verify则打印后继续进行，个人感觉verify是更好的选择……

>commands
很多命令的读写，可以操作很多的bom、dom对象

>webdriver protocol
可以操作一些更底层的东西，比如alert、session、屏幕中的位置等

## E2E

E2E(end to end)测试是指端到端测试又叫功能测试，站在用户视角，使用各种功能、各种交互，是用户的真实使用场景的仿真。在产品高速迭代的现在，有个自动化测试，是重构、迭代的重要保障。对web前端来说，主要的测试就是，表单、动画、页面跳转、dom渲染、Ajax等是否按照期望。

**E2E测试驱动重构**

重构代码的目的是什么？是为了满足功能需求的代码能够像奥林匹克精神描述的那样：质量更高，性能更好，拓展性更强，以及让代码看上去更优雅。如果你重构了代码，却无法实现需要的功能，纵使再优雅，性能再高，又有何用？E2E测试正是保证功能的最高层测试，不着眼于代码细节，而是专注于代码能否实现对应的功能，相比于单元测试、集成测试更灵活，你可以彻底改变编码的语法、架构甚至编程范式而不用重新写测试用例。

既然有需求，就会有轮子，不爱造轮子的不是合格的程序员。

不同于行为驱动测试（BDD）和单元测试独立运行并使用模拟/存根，端到端测试将试着尽可能从用户的视角，对真实系统的访问行为进行仿真。

简化了编写自动化测试的过程外，Nightwatch还能够与持续集成的流水作业结合，从而对开发中的系统进行完整的诊断：
从Nightwatch网站找到当前提供特性的列表：

简单但强大的语法。只需要使用JavaScript和CSS选择器，开发者就能够非常迅捷地撰写测试。开发者也不必初始化其他对象和类，只需要编写测试规范即可。

内建命令行测试运行器，允许开发者同时运行全部测试——分组或单个运行。

自动管理Selenium服务器；如果Selenium运行在另一台机器上，那么也可以禁用此特性。

支持持续集成：内建JUnit XML报表，因此开发者可以在构建过程中，将自己的测试与系统（例如Hudson 或Teamcity等）集成。

使用CSS选择器或Xpath，定位并验证页面中的元素或是执行命令。

易于扩展，便于开发者根据需要，实现与自己应用相关的命令。

## Selenium/PhantomJS

Selenium是JavaScript的世界里验收测试方面最流行的工具之一，类似的还有PhantomJS。二者都有其独到的方法：Selenium使用其WebDriver API，而PhantomJS使用无界面的WebKit浏览器。它们都是非常成熟的工具，都具有强大的社区支持。它们与Nightwatch之间最大的不同，主要是在于语法的简易度以及对持续集成的支持。与Nightwatch相比，Selenium和PhantomJS都拥有更加冗长的语法，这会让编码变得更庞大，而且不支持从命令行中进行开箱即用的持续集成（JUnit XML或其他标准输出）。

## 其它

前端是一种特殊的GUI软件
API测试方法论在测试GUI时并不能解决所有问题。
前端GUI测试的特殊性
GUI（Graphical User Interface，图形用户界面）是计算机软件与用户进行交互的主要方式。GUI软件测试是指对使用GUI的软件进行的软件测试。
**GUI软件的难点**

1. 测试用例需要专门的定义。
2. 测试用例的生成变得复杂。

个人认为一般前端自动化测试大致包括

* 类库单元测试自动化
* UI组件测试自动化

作為國內來說還算少數的Harmony使用者，我可以說說自己的經驗。

1. Promise經歷了好幾個版本的製定，從很久之前就已經存在，它解決的是串行的異步嵌套問題
2. yield關鍵字把Generator Function切割為擁有多個出口的Generation

Promise是JavaScript社區的研發產物，而yield則是ECMA-262從別處參考而來的「類協程」實現。

yield可以做到非串行，而Promise很難。Promise兼容性強，yield只能繼續等。
各有千秋

關於Generation，請參考[我的博客](http://lifemap.in/koa-co-and-coruntine/)

### Promise最大的特点有以下几个： 

1. 原本嵌套式的callback模型变成“看上去线性”的模型，以此提供代码逻辑的顺畅性 
2. 异常传递，即当任何一个Promise失败时，异常会透过那些没有reject处理的节点一直到最后去，这是NodeJS的callback模型没有做到的。异常传递更接近正常代码中的try/catch，你可以有N行代码，任何一行代码都可能出错，但总能被后面的catch捕获，而NodeJS的callback模型要在每个callback中处理err参数，这是我一直反对NodeJS的异步模型的很重要的原因之一 

而以上几点，其实通过Generator都能做，但这并不代表Generator和Promise是一类东西。Generator可以做更多的事，比如： 

1. 生成一个无限列表，每次获取都递增1 
2. 完成类似C#的Linq的工作，即多个对数组元素的操作（Iterator）只需要遍历一次数组，比如这样：asGenerator(array).each(o -> o.x++).where(o -> o.x > 10).map(o -> o.x)，只有一次遍历 
3. 产生一个每次获取一个元素都有重要、高消耗的资源访问的列表，完成延迟加载模型来将高消耗的元素获取延迟到真正访问时，而不需要一开始就获取N次形成静态的数组 

### Generator

**Generator**的用法非常非常多，上面说的也只是经典场景而已，绝对不要只把它当成一个coroutine的玩法，会被自己关在笼子里玩不开的
最近一直在研究Generator。本身协程（yield）的引入并不是用来解决异步问题的。而异步回调的本质就是接受异步产生的结果，继续完成接下来的任务。而yield的next接口恰巧提供了这种便利，这也是Co实现的本质。

Generator是为JavaScript设计的一种轻量级的协程。它通过yield关键字，可以控制一个函数暂停或者继续执行Generator函数有一个特别的语法`function* ()`，借助这种超能力，我们还可以暂停或者继续执行异步操作，使用像promise或者”thunks”这样的结构来写出看起来像同步的代码。

### Co
神奇的Co登场了！这是一个tj大神写的库。使用方法很简单，在Github上的README也讲得很清楚了。主要就是两点：

1.
Co函数里面包裹一个generator函数，在generator函数里面可以yield promise对象，从而达到异步的目的。在Co的内部实现里面是通过递归调用next函数，把每一个promise的值返回出来，从而实现异步转“同步”的写法。

2. 
Co函数返回一个promise对象，可以调用then，catch方法来对Generator函数返回的结果进行传递。方便进行后续的成功处理或者错误处理。

3. 如何用同步的写法写异步的代码

**DRY（不要重复自己，don't repeat yourself）**

**高内聚低耦合（loose coupling high cohesion）**

**YAGNI （你不会用到它的，ya ain't gonna need it）**

**最小意外原则（Principle of least surprise）**

**单一责任（single responsibility）**
等等。

### curry

curry 的概念很简单：只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。

戏剧性的是：纯函数就是数学上的函数，而且是函数式编程的全部。

我最喜欢的名言之一是 Erlang 语言的作者 Joe Armstrong 说的这句话：“面向对象语言的问题是，它们永远都要随身携带那些隐式的环境。你只需要一个香蕉，但却得到一个拿着香蕉的大猩猩...以及整个丛林”。

### Quickcheck

Quickcheck——一个为函数式环境量身定制的测试工具。

等式推导带来的分析代码的能力对重构和理解代码非常重要。

*引用透明性（referential transparency）*。如果一段代码可以替换成它执行所得的结果，而且是在不改变整个程序行为的前提下替换的，那么我们就说这段代码是引用透明的。

由于纯函数总是能够根据相同的输入返回相同的输出，所以它们就能够保证总是返回同一个结果，这也就保证了引用透明性。
纯函数根本不需要访问共享的内存，而且根据其定义，纯函数也不会因副作用而进入竞争态（race condition）。
通过简单地传递几个参数，就能动态创建实用的新函数；而且还能带来一个额外好处，那就是保留了数学的函数定义，尽管参数不止一个。

### TS

TypeScript 是 JavaScript 的强类型版本。然后在编译期去掉类型和特有语法，生成纯粹的 JavaScript 代码。由于最终在浏览器中运行的仍然是 JavaScript，所以 TypeScript 并不依赖于浏览器的支持，也并不会带来兼容性问题。

TypeScript 是 JavaScript 的强类型版本。然后在编译期去掉类型和特有语法，生成纯粹的 JavaScript 代码。由于最终在浏览器中运行的仍然是 JavaScript，所以 TypeScript 并不依赖于浏览器的支持，也并不会带来兼容性问题。

差距已经越来越小了。native app最终还是归于硬件调用，而与网站相关联的APP，未来基本是html5的天下。

* [Link1](https://www.zhihu.com/question/28016252/answer/39056940)
* [Link2](https://www.zhihu.com/question/21879449?sort=created)