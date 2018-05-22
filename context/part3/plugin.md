# Tools

## Grunt

安装 Grunt 前，你需要首先下载并安装 node.js （npm 已经包含在内）。npm 是 node packaged modules 的简称，它的作用是基于 node.js 管理扩展包之间的依赖关系。

Bootstrap 使用 Grunt 作为编译系统，并且对外提供了一些方便的方法用于编译整个框架。

## Redux

随着 JavaScript 单页应用开发日趋复杂，JavaScript 需要管理比任何时候都要多的 state （状态）。 这些 state 可能包括服务器响应、缓存数据、本地生成尚未持久化到服务器的数据，也包括 UI 状态，如激活的路由，被选中的标签，是否显示加载动效或者分页器等等。

这里的复杂性很大程度上来自于：我们总是将两个难以厘清的概念混淆在一起：变化和异步。 我称它们为曼妥思和可乐。如果把二者分开，能做的很好，但混到一起，就变得一团糟。

**Redux**可以用这三个基本原则来描述：

> 单一数据源

整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中。

> State 是只读的

惟一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象。

> 使用纯函数来执行修改

为了描述 action 如何改变 state tree ，你需要编写 reducers。

Redux 的灵感来源于 Flux 的几个重要特性。和 Flux 一样，Redux 规定，将模型的更新逻辑全部集中于一个特定的层（Flux 里的 store，Redux 里的 reducer）。

Elm 是一种函数式编程语言，由 Evan Czaplicki 受 Haskell 语言的启发开发。它执行一种 “model view update” 的架构 ，更新遵循 (state, action) => state 的规则。 Elm 的 “updater” 与 Redux 里的 reducer 服务于相同的目的。

Immutable 是一个可实现持久数据结构的 JavaScript 库。它性能很好，并且命名符合 JavaScript API 的语言习惯 。

Action 是把数据从应用（译者注：这里之所以不叫 view 是因为这些数据有可能是服务器响应，用户输入或其它非 view 的数据 ）传到 store 的有效载荷。它是 store 数据的唯一来源。

Action 创建函数 就是生成 action 的方法。“action” 和 “action 创建函数” 这两个概念很容易混在一起，使用时最好注意区分。
Action 只是描述了有事情发生了这一事实，并没有指明应用如何更新 state。而这正是 reducer 要做的事情。

### 设计 State 结构

在 Redux 应用中，所有的 state 都被保存在一个单一对象中。建议在写代码前先想一下这个对象的结构。如何才能以最简的形式把应用的 state 用对象描述出来？

以 todo 应用为例，需要保存两种不同的数据：

> 当前选中的任务过滤条件；

> 完整的任务列表。

通常，这个 state 树还需要存放其它一些数据，以及一些 UI 相关的 state。这样做没问题，但尽量把这些数据与 UI 相关的 state 分开。

现在我们已经确定了 state 对象的结构，就可以开始开发 reducer。reducer 就是一个纯函数，接收旧的 state 和 action，返回新的 state。

现在只需要谨记 reducer 一定要保持纯净。只要传入参数相同，返回计算得到的下一个 state 就一定相同。没有特殊情况、没有副作用，没有 API 请求、没有变量修改，单纯执行计算。

注意每个 reducer 只负责管理全局 state 中它负责的一部分。每个 reducer 的 state 参数都不同，分别对应它管理的那部分 state 数据。

Store 就是把它们联系到一起的对象。Store 有以下职责：

> 维持应用的 state；

> 提供 getState() 方法获取 state；

> 提供 dispatch(action) 方法更新 state；

> 通过 subscribe(listener) 注册监听器;

> 通过 subscribe(listener) 返回的函数注销监听器。

createStore() 的第二个参数是可选的, 用于设置 state 初始状态。这对开发同构应用时非常有用，服务器端 redux 应用的 state 结构可以与客户端保持一致, 那么客户端可以将从网络接收到的服务端 state 直接用于本地数据初始化。

**严格的单向数据流是 Redux 架构的设计核心。**

这意味着应用中所有的数据都遵循相同的生命周期，这样可以让应用变得更加可预测且容易理解。同时也鼓励做数据范式化，这样可以避免使用多个且独立的无法相互引用的重复数据。

这里需要再强调一下：Redux 和 React 之间没有关系。Redux 支持 React、Angular、Ember、jQuery 甚至纯 JavaScript。
尽管如此，Redux 还是和 React 和 Deku 这类框架搭配起来用最好，因为这类框架允许你以 state 函数的形式来描述界面，Redux 通过 action 的形式来发起 state 变化。

当调用异步 API 时，有两个非常关键的时刻：**发起请求的时刻，和接收到响应的时刻 （也可能是超时）**。

这两个时刻都可能会更改应用的 state；为此，你需要 dispatch 普通的同步 action。一般情况下，每个 API 请求都需要 dispatch 至少三种 action：
*
一种通知 reducer 请求开始的 action。
>
对于这种 action，reducer 可能会切换一下 state 中的 isFetching 标记。以此来告诉 UI 来显示进度条。
*
一种通知 reducer 请求成功结束的 action。
>
对于这种 action，reducer 可能会把接收到的新数据合并到 state 中，并重置 isFetching。UI 则会隐藏进度条，并显示接收到的数据。
*
一种通知 reducer 请求失败的 action。
>
对于这种 action，reducer 可能会重置 isFetching。或者，有些 reducer 会保存这些失败信息，并在 UI 里显示出来。

默认情况下，createStore() 所创建的 Redux store 没有使用 middleware，所以只支持 同步数据流。

你可以使用 applyMiddleware() 来增强 createStore()。虽然这不是必须的，但是它可以帮助你用简便的方式来描述异步的 action。

#### "如果你不知道是否需要 Redux，那就是不需要它。"
Redux 的创造者 Dan Abramov 又补充了一句。
#### "只有遇到 React 实在解决不了的问题，你才需要 Redux 。"

如果你的UI层非常简单，没有很多互动，Redux 就是不必要的，用了反而增加复杂性。

> 用户的使用方式非常简单
> 用户之间没有协作
> 不需要与服务器大量交互，也没有使用 WebSocket
> 视图层（View）只从单一来源获取数据

> 用户的使用方式复杂
> 不同身份的用户有不同的使用方式（比如普通用户和管理员）
> 多个用户之间可以协作
> 与服务器大量交互，或者使用了WebSocket
> View要从多个来源获取数据
**Redux 的适用场景：多交互、多数据源。**

> 某个组件的状态，需要共享
> 某个状态需要在任何地方都可以拿到
> 一个组件需要改变全局状态
> 一个组件需要改变另一个组件的状态

### Redux 的设计思想很简单
1. Web 应用是一个状态机，视图与状态是一一对应的。
2. 所有的状态，保存在一个对象里面。