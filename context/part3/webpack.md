# Webpack

webpack 是一个现代 JavaScript 应用程序的模块打包器(module bundler)。

当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成少量的 bundle - 通常只有一个，由浏览器加载。

## 入口(Entry)

webpack 创建应用程序所有依赖的关系图(dependency graph)。图的起点被称之为入口起点(entry point)。入口起点告诉 webpack 从哪里开始，并根据依赖关系图确定需要打包的内容。可以将应用程序的入口起点认为是根上下文(contextual root) 或 app 第一个启动文件。

*webpack.config.js*

## 出口(Output)

将所有的资源(assets)归拢在一起后，还需要告诉 webpack 在哪里打包应用程序。webpack 的 output 属性描述了如何处理归拢在一起的代码(bundled code)。

*你可能看到项目生成(emitted 或 emit)贯穿我们整个文档和插件 API。它是“生产(produced)”或“排放(discharged)”的特殊术语。*

## Loader

webpack 的目标是，让 webpack 聚焦于项目中的所有资源(asset)，而浏览器不需要关注考虑这些（明确的说，这并不意味着所有资源(asset)都必须打包在一起）。webpack 把每个文件(.css, .html, .scss, .jpg, etc.) 都作为模块处理。然而 webpack 自身只理解 JavaScript。

webpack loader 在文件被添加到依赖图中时，其转换为模块。

1. 识别出(identify)应该被对应的 loader 进行转换(transform)的那些文件。(test 属性)

2. 转换这些文件，从而使其能够被添加到依赖图中（并且最终添加到 bundle 中）(use 属性)

重要的是要记得，在 webpack 配置中定义 loader 时，要定义在 module.rules 中，而不是 rules。然而，在定义错误时 webpack 会给出严重的警告。为了使你受益于此，如果没有按照正确方式去做，webpack 会“给出严重的警告”

## 插件(Plugins)

### 剖析

webpack 插件是一个具有 apply 属性的 JavaScript 对象。apply 属性会被 webpack compiler 调用，并且 compiler 对象可在整个编译生命周期访问。

“可扩展的 webpack 配置”是指，可重用并且可以与其他配置组合使用。这是一种流行的技术，用于将关注点(concern)从环境(environment)、构建目标(build target)、运行时(runtime)中分离。然后使用专门的工具（如 webpack-merge）将它们合并。
分离 应用程序(app) 和 第三方库(vendor) 入口

在 webpack 中配置 output 属性的最低要求是，将它的值设置为一个对象，包括以下两点：

> filename 用于输出文件的文件名。

> 目标输出目录 path 的绝对路径。

在编译时不知道最终输出文件的 publicPath 的情况下，publicPath 可以留空，并且在入口起点文件运行时动态设置。如果你在编译时不知道 publicPath，你可以先忽略它，并且在入口起点设置` __webpack_public_path__`。

`__webpack_public_path__ = myRuntimePublicPath`

## 使用 Loader

在你的应用程序中，有三种使用 loader 的方式：

> 配置（推荐）：在 webpack.config.js 文件中指定 loader。

> 内联：在每个 import 语句中显式指定 loader。

> CLI：在 shell 命令中指定它们。

### 内联

可以在 import 语句或任何等效于 "import" 的方式中指定 loader。使用 ! 将资源中的 loader 分开。分开的每个部分都相对于当前目录解析。

尽可能使用 module.rules，因为这样可以减少源码中的代码量，并且可以在出错时，更快地调试和定位 loader 中的问题。

* loader 支持链式传递。能够对资源使用流水线(pipeline)。一组链式的 loader 将按照先后顺序进行编译。loader 链中的第一个 loader 返回值给下一个 loader。在最后一个 loader，返回 webpack 所预期的 JavaScript。

* loader 可以是同步的，也可以是异步的。

* loader 运行在 Node.js 中，并且能够执行任何可能的操作。

* loader 接收查询参数。用于对 loader 传递配置。

* loader 也能够使用 options 对象进行配置。

除了使用 package.json 常见的 main 属性，还可以将普通的 npm 模块导出为 loader，做法是在 package.json 里定义一个 loader 字段。

**插件(plugin)**可以为 loader 带来更多特性。
loader 能够产生额外的任意文件。

因为 webpack 配置是标准的 Node.js CommonJS 模块，你可以做到以下事情：

> 通过 require(...) 导入其他文件

> 通过 require(...) 使用 npm 的工具函数

> 使用 JavaScript 控制流表达式，例如 ?: 操作符

> 对常用值使用常量或变量

> 编写并执行函数来生成部分配置

对比 Node.js 模块，webpack 模块能够以各种方式表达它们的依赖关系，几个例子如下：

> ES2015 import 语句

> CommonJS require() 语句

> AMD define 和 require 语句

> css/sass/less 文件中的 @import 语句。

> 样式(url(...))或 HTML 文件(<img src=...>)中的图片链接(image url)

## 支持的模块类型

webpack 通过 loader 可以支持各种语言和预处理器编写模块。loader 描述了 webpack 如何处理 非 JavaScript(non-JavaScript) 模块，并且在bundle中引入这些依赖。 webpack 社区已经为各种流行语言和语言处理器构建了 loader，包括：

### 依赖图(Dependency Graph)

任何时候，一个文件依赖于另一个文件，webpack 就把此视为文件之间有依赖关系。这使得 webpack 可以接收非代码资源(non-code asset)（例如图像或 web 字体），并且可以把它们作为依赖提供给你的应用程序。

webpack 从命令行或配置文件中定义的一个模块列表开始，处理你的应用程序。 从这些入口起点开始，webpack 递归地构建一个依赖图，这个依赖图包含着应用程序所需的每个模块，然后将所有这些模块打包为少量的 bundle- 通常只有一个 - 可由浏览器加载。

### Manifest

runtime，以及伴随的 manifest 数据，主要是指：在浏览器运行时，webpack 用来连接模块化的应用程序的所有代码。runtime 包含：在模块交互时，连接模块所需的加载和解析逻辑。包括浏览器中的已加载模块的连接，以及懒加载模块的执行逻辑。

一旦你的应用程序中，形如 index.html 文件、一些 bundle 和各种资源加载到浏览器中，会发生什么？你精心安排的 /src 目录的文件结构现在已经不存在，所以 webpack 如何管理所有模块之间的交互呢？这就是 manifest 数据用途的由来……
当编译器(compiler)开始执行、解析和映射应用程序时，它会保留所有模块的详细要点。这个数据集合称为 "Manifest"，当完成打包并发送到浏览器时，会在运行时通过 Manifest 来解析和加载模块。无论你选择哪种模块语法，那些 import 或 require 语句现在都已经转换为 __webpack_require__ 方法，此方法指向模块标识符(module identifier)。通过使用 manifest 中的数据，runtime 将能够查询模块标识符，检索出背后对应的模块。

答案是大多数情况下没有。runtime 做自己该做的，使用 manifest 来执行其操作，然后，一旦你的应用程序加载到浏览器中，所有内容将展现出魔幻般运行。然而，如果你决定通过使用浏览器缓存来改善项目的性能，理解这一过程将突然变得尤为重要。

通过使用 bundle 计算出内容散列(content hash)作为文件名称，这样在内容或文件修改时，浏览器中将通过新的内容散列指向新的文件，从而使缓存无效。一旦你开始这样做，你会立即注意到一些有趣的行为。即使表面上某些内容没有修改，计算出的哈希还是会改变。这是因为，runtime 和 manifest 的注入在每次构建都会发生变化。

在应用程序中置换(swap in and out)模块：

1. 应用程序代码要求 HMR runtime 检查更新。
2. HMR runtime（异步）下载更新，然后通知应用程序代码。
3. 应用程序代码要求 HMR runtime 应用更新。
4. HMR runtime（异步）应用更新。

你可以设置 HMR，以使此进程自动触发更新，或者你可以选择要求在用户交互时进行更新。

### 在编译器中

除了普通资源，编译器(compiler)需要发出 "update"，以允许更新之前的版本到新的版本。"update" 由两部分组成：

1. 更新后的 manifest(JSON)
2. 一个或多个更新后的 chunk (JavaScript)

### 在模块中

HMR 是可选功能，只会影响包含 HMR 代码的模块。

在开发过程中，可以将 HMR 作为 LiveReload 的替代。webpack-dev-server 支持 hot 模式，在试图重新加载整个页面之前，热模式会尝试使用 HMR 来更新。





