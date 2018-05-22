# 网页模板

模板引擎就是基于模板配合数据构造出字符串输出的一个组件。

## Nunjucks

我们选择Nunjucks作为模板引擎。Nunjucks是Mozilla开发的一个纯JavaScript编写的模板引擎，既可以用在Node环境下，又可以运行在浏览器端。但是，主要还是运行在Node环境下，因为浏览器端有更好的模板解决方案，例如MVVM框架。

如果你使用过Python的模板引擎jinja2，那么使用Nunjucks就非常简单，两者的语法几乎是一模一样的，因为Nunjucks就是用JavaScript重新实现了jinjia2。

最后我们要考虑一下Nunjucks的性能。
对于模板渲染本身来说，速度是非常非常快的，因为就是拼字符串嘛，纯CPU操作。