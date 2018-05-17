# Node.js

## Node基础

Node约定，如果某个函数需要回调函数作为参数，则回调函数是最后一个参数。另外，回调函数本身的第一个参数，约定为上一步传入的错误对象。

db.User.get方法是一个异步操作，等到抛出错误时，可能它所在的try…catch代码块早就运行结束了，这会导致错误无法被捕捉。所以，Node统一规定，一旦异步操作发生错误，就把错误对象传递到回调函数。

Node提供以下几个全局对象，它们是所有模块都可以调用的。

**global**：表示Node所在的全局环境，类似于浏览器的window对象。需要注意的是，如果在浏览器中声明一个全局变量，实际上是声明了一个全局对象的属性，比如
```bash
var x = 1
# 等同于设置
window.x = 1
```
但是Node不是这样，至少在模块中不是这样（REPL环境的行为与浏览器一致）。在模块文件中，声明
`var x = 1`，该变量不是global对象的属性，global.x等于undefined。这是因为模块的全局变量都是该模块私有的，其他模块无法取到。

**process**：该对象表示Node所处的当前进程，允许开发者与该进程互动。

*Node还提供一些全局函数*。

**setTimeout()**：用于在指定毫秒之后，运行回调函数。实际的调用间隔，还取决于系统因素。间隔的毫秒数在1毫秒到2,147,483,647毫秒（约24.8天）之间。如果超过这个范围，会被自动改为1毫秒。该方法返回一个整数，代表这个新建定时器的编号。
**clearTimeout()，setInterval()，clearInterval()，require()**

**Buffer()**：用于操作二进制数据

`__filename`：指向当前运行的脚本文件名。

`__dirname`：指向当前运行的脚本所在的目录。

**require**方法的参数是模块文件的名字。它分成两种情况，第一种情况是参数中含有文件路径（比如上例），这时路径是相对于当前脚本所在的目录，第二种情况是参数中不含有文件路径，这时Node到模块的安装目录，去寻找已安装的模块

有时候，一个模块本身就是一个目录，目录中包含多个文件。这时候，Node在package.json文件中，寻找main属性所指明的模块入口文件。
如果模块目录中没有package.json文件，node.js会尝试在模块目录中寻找index.js或index.node文件进行加载。

你的好友会随时的推送新的状态，然后你的新鲜事会实时自动刷新。要达成这个需求，我们需要让用户一直与服务器保持一个有效连接。目前最简单的实现方法，就是让用户和服务器之间保持长轮询（long polling）。

这种情况怎么解决？解决方法就是刚才上边说的：非阻塞和事件驱动。这些概念在我们谈的这个情景里面其实没那么难理解。你把非阻塞的服务器想象成一个loop循环，这个loop会一直跑下去。一个新请求来了，这个loop就接了这个请求，把这个请求传给其他的进程（比如传给一个搞数据库查询的进程），然后响应一个回调（callback）。完事了这loop就接着跑，接其他的请求。这样下来。服务器就不会像之前那样傻等着数据库返回结果了。

如果数据库把结果返回来了，loop就把结果传回用户的浏览器，接着继续跑。在这种方式下，你的服务器的进程就不会闲着等着。从而在理论上说，同一时刻的数据库查询数量，以及用户的请求数量就没有限制了。服务器只在用户那边有事件发生的时候才响应，这就是事件驱动。

FriendFeed是用基于Python的非阻塞框架Tornado (知乎也用了这个框架) 来实现上面说的新鲜事功能的。不过，Node.js就比前者更妙了。Node.js的应用是通过javascript开发的，然后直接在Google的变态V8引擎上跑。用了Node.js，你就不用担心用户端的请求会在服务器里跑了一段能够造成阻塞的代码了。因为javascript本身就是事件驱动的脚本语言。你回想一下，在给前端写javascript的时候，更多时候你都是在搞事件处理和回调函数。javascript本身就是给事件处理量身定制的语言。

如果只是在服务器运行JavaScript代码，用处并不大，因为服务器脚本语言已经有很多种了。Node.js的用处在于，它本身还提供了一系列功能模块，与操作系统互动。这些核心的功能模块，不用安装就可以使用，下面是它们的清单。

>http：提供HTTP服务器功能。

>url：解析URL。

>fs：与文件系统交互

>querystring：解析URL的查询字符串

>child_process：新建子进程。

>util：提供一系列实用小工具。

>path：处理文件路径。

>crypto：提供加密和解密功能，基本上是对OpenSSL的包装

上面这些核心模块，源码都在Node的lib子目录中。为了提高运行速度，它们安装时都会被编译成二进制文件。
module变量是整个模块文件的顶层变量，它的exports属性就是模块向外输出的接口。如果直接输出一个函数（就像上面的foo.js），那么调用模块就是调用一个函数。但是，模块也可以输出一个对象。

### 异常处理

Node有三种方法，传播一个错误。

>使用throw语句抛出一个错误对象，即抛出异常。

>将错误对象传递给回调函数，由回调函数负责发出错误。

>通过EventEmitter接口，发出一个error事件。

最常用的捕获异常的方式，就是使用try…catch结构。但是，这个结构无法捕获异步运行的代码抛出的异常。
上面代码分别用process.nextTick和setTimeout方法，在下一轮事件循环抛出两个异常，代表异步操作抛出的错误。它们都无法被catch代码块捕获，因为catch代码块所在的那部分已经运行结束了。
一种解决方法是将错误捕获代码，也放到异步执行。

### 回调函数

Node采用的方法，是将错误对象作为第一个参数，传入回调函数。这样就避免了捕获代码与发生错误的代码不在同一个时间段的问题。
读取文件foo.txt是一个异步操作，它的回调函数有两个参数，第一个是错误对象，第二个是读取到的文件数据。如果第一个参数不是null，就意味着发生错误，后面代码也就不再执行了。

发生错误的时候，也可以用EventEmitter接口抛出error事件。

当一个异常未被捕获，就会触发uncaughtException事件，可以对这个事件注册回调函数，从而捕获异常。

iojs有一个unhandledRejection事件，用来监听没有捕获的Promise对象的rejected状态。

### 命令行脚本

node脚本可以作为命令行脚本使用。

## Buffer对象

**Buffer**对象是Node处理二进制数据的一个接口。它是Node原生提供的全局对象，可以直接使用，不需要require('buffer')。

Buffer对象支持以下编码格式。

>ascii

>utf8

>utf16le：UTF-16的小端编码，支持大于U+10000的四字节字符。

>ucs2：utf16le的别名。

>base64

>hex：将每个字节转为两个十六进制字符。

### 实例方法

new Uint32Array(new Buffer([1, 2, 3, 4]))，生成一个4个成员的二进制数组。注意，新数组的成员有四个，而不是只有单个成员（[0x1020304]或者[0x4030201]）。

Buffer作为构造函数，可以用new命令生成一个实例，它可以接受多种形式的参数。

### 类的方法

Buffer.isEncoding方法返回一个布尔值，表示Buffer实例是否为指定编码。

Buffer.isBuffer方法接受一个对象作为参数，返回一个布尔值，表示该对象是否为Buffer实例。

Buffer.byteLength方法返回字符串实际占据的字节长度，默认编码方式为utf8。

Buffer.concat方法将一组Buffer对象合并为一个Buffer对象。

### 实例属性

length属性返回Buffer对象所占据的内存长度。注意，这个值与Buffer对象的内容无关。

### 实例方法

**write**方法可以向指定的Buffer对象写入数据。它的第一个参数是所写入的内容，第二个参数（可省略）是所写入的起始位置（默认从0开始），第三个参数（可省略）是编码方式，默认为utf8。

**slice**方法返回一个按照指定位置、从原对象切割出来的Buffer实例。它的两个参数分别为切割的起始位置和终止位置。

**toString**方法将Buffer实例，按照指定编码（默认为utf8）转为字符串。

**toJSON**方法将Buffer实例转为JSON对象。如果JSON.stringify方法调用Buffer实例，默认会先调用toJSON方法。

## Child Process模块

**child_process**模块用于新建子进程。子进程的运行结果储存在系统缓存之中（最大200KB），等到子进程运行结束以后，主进程再用回调函数读取子进程的运行结果。

**exec**方法用于执行bash命令，它的参数是一个命令字符串。

上面代码的exec方法用于新建一个子进程，然后缓存它的运行结果，运行结束后调用回调函数。
exec方法最多可以接受两个参数，第一个参数是所要执行的shell命令，第二个参数是回调函数，该函数接受三个参数，分别是发生的错误、标准输出的显示结果、标准错误的显示结果。

由于标准输出和标准错误都是流对象（stream），可以监听data事件，因此上面的代码也可以写成下面这样。
监听data事件以后，可以实时输出结果，否则只有等到子进程结束，才会输出结果。
在bash环境下，ls -l; user input会直接运行。如果用户输入恶意代码，将会带来安全风险。因此，在有用户输入的情况下，最好不使用exec方法，而是使用execFile方法。

**execSync**是exec的同步执行版本。
它可以接受两个参数，第一个参数是所要执行的命令，第二个参数用来配置执行环境。

execSync方法的第二个参数是一个对象。该对象的cwd属性指定脚本的当前目录，env属性指定环境变量。
spawn方法创建一个子进程来执行特定命令，用法与execFile方法类似，但是没有回调函数，只能通过监听事件，来获取运行结果。它属于异步执行，适用于子进程长时间运行的情况。

**spawn**方法接受两个参数，第一个是可执行文件，第二个是参数数组。
spawn对象返回一个对象，代表子进程。该对象部署了EventEmitter接口，它的data事件可以监听，从而得到子进程的输出结果。
fork方法直接创建一个子进程，执行Node脚本，fork('./child.js') 相当于 spawn('node', ['./child.js']) 。与spawn方法不同的是，fork会在父进程与子进程之间，建立一个通信管道，用于进程之间的通信。
fork方法返回一个代表进程间通信管道的对象，对该对象可以监听message事件，用来获取子进程返回的信息，也可以向子进程发送信息。
cluster模块允许设立一个主进程和若干个worker进程，由主进程监控和协调worker进程的运行。worker之间采用进程间通信交换消息，cluster模块内置一个负载均衡器，采用Round-robin算法协调各个worker进程之间的负载。运行时，所有新建立的链接都由主进程完成，然后主进程再把TCP连接分配给指定的worker进程。
上面这段代码有一个缺点，就是一旦work进程挂了，主进程无法知道。为了解决这个问题，可以在主进程部署online事件和exit事件的监听函数。

**worker对象**是cluster.fork()的返回值，代表一个worker进程。

> worker.id
worker.id返回当前worker的独一无二的进程编号。这个编号也是cluster.workers中指向当前进程的索引值。

> worker.process
所有的worker进程都是用child_process.fork()生成的。child_process.fork()返回的对象，就被保存在worker.process之中。通过这个属性，可以获取worker所在的进程对象。

> worker.send()
该方法用于在主进程中，向子进程发送信息。
通常，我们在Shell启动Node脚本。

### Shell

Shell，壳，顾名思义就是机器外面的一层壳，用于人机交互，只要是人与电脑之间交互的接口，就可以称为 Shell，所以我们熟悉的 GNOME、KDE等图形界面也都是 Shell，只不过是 GUI Shell。所以像Bash 等 Shell 当初发明的原因当然也就很容易理解了，就是为了人与机器之间交互的问题，只不过当时的技术还不能做出 GUI
可以把 shell 理解为 命令解释器。的作用是解释执行用户的命令,用户输入一条命令,Shell就解释执行一条,这种方式称为交互式(Interactive)。

Shell，译为外壳，是用户直接连入计算机所使用的计算机程序，负责解析用户提供的命令，如词法分析、语法分析、句法分析。
为了让Node进程在后台长期启动，需要一个daemon（即常驻的服务进程）。

### Forever

## events模块

**events模块**的EventEmitter是一个构造函数，可以用来生成事件发生器的实例emitter。

EventEmitter对象的事件触发和监听是同步的，即只有事件的回调函数执行以后，函数f才会继续执行。
```bash
var EventEmitter = require('events').EventEmitter;
function Dog(name) {
  this.name = name;}
Dog.prototype.__proto__ = EventEmitter.prototype;
# 另一种写法 
Dog.prototype = Object.create(EventEmitter.prototype);
var simon = new Dog('simon');
simon.on('bark', function () {
  console.log(this.name + ' barked');});
setInterval(function () {
  simon.emit('bark');}, 500);
```
上面代码新建了一个构造函数Dog，然后让其继承EventEmitter，因此Dog就拥有了EventEmitter的接口。最后，为Dog的实例指定bark事件的监听函数，再使用EventEmitter的emit方法，触发bark事件。

Node 内置模块util的**inherits**方法，提供了另一种继承 Event Emitter 接口的方法。

>emitter.on(name, f) 对事件name指定监听函数f

>emitter.addListener(name, f) addListener是on方法的别名

>emitter.once(name, f) 与on方法类似，但是监听函数f是一次性的，使用后自动移除

>emitter.listeners(name) 返回一个数组，成员是事件name所有监听函数

>emitter.removeListener(name, f) 移除事件name的监听函数f

>emitter.removeAllListeners(name) 移除事件name的所有监听函数

EventEmitter实例对象的**emit**方法，用来触发事件。它的第一个参数是事件名称，其余参数都会依次传入回调函数。

Events模块默认支持两个事件。

1. newListener事件：添加新的回调函数时触发。
2. removeListener事件：移除回调时触发。

## Express

**Express**是目前最流行的基于Node.js的Web开发框架，可以快速地搭建一个完整功能的网站。

在项目根目录下，新建一个启动文件，假定叫做index.js

启动脚本index.js的app.get方法，用于指定不同的访问路径所对应的回调函数，这叫做“路由”（routing）。

http模块的**createServer**方法，表示生成一个HTTP服务器实例。该方法接受一个回调函数，该回调函数的参数，分别为代表HTTP请求和HTTP回应的request对象和response对象。

Express框架的核心是对http模块的再包装。

原来是用**http.createServer**方法新建一个app实例，现在则是用Express的构造方法，生成一个Epress实例。两者的回调函数都是相同的。Express框架等于在http模块之上，加了一个中间层。

### 中间件

中间件（middleware）就是处理HTTP请求的函数。它最大的特点就是，一个中间件处理完，再传递给下一个中间件。App实例在运行过程中，会调用一系列的中间件。

每个中间件可以从App实例，接收三个参数，依次为request对象（代表HTTP请求）、response对象（代表HTTP回应），next回调函数（代表下一个中间件）。每个中间件都可以对HTTP请求（request对象）进行加工，并且决定是否调用next方法，将request对象再传给下一个中间件。

除了get方法以外，Express还提供post、put、delete方法，即HTTP动词都是Express的方法。

**response对象**

response.redirect方法允许网址的重定向。

response.sendFile方法用于发送文件。

response.render方法用于渲染网页模板。

上面代码使用render方法，将message变量传入index模板，渲染成HTML网页。

request.ip属性用于获得HTTP请求的IP地址。

request.files用于获取上传的文件。

将**hbs模块**，安装在项目目录的子目录node_modules之中。save-dev参数表示，将依赖关系写入package.json文件。运行hbs模块app.engine('html', `hbs.__express`);

从**Express 4.0**开始，路由器功能成了一个单独的组件Express.Router。它好像小型的express应用程序一样，有自己的use、get、param和route方法。
服务器脚本建立指向/upload目录的路由。这时可以安装multer模块，它提供了上传文件的许多功能。

## Koa

Koa是一个类似于Express的Web开发框架，创始人也是同一个人。它的主要特点是，使用了ES6的Generator函数，进行了架构的重新设计。也就是说，Koa的原理和内部结构很像Express，但是语法和内部结构进行了升级。

### 中间件

Koa的中间件很像Express的中间件，也是对HTTP请求进行处理的函数，但是必须是一个Generator函数。而且，Koa的中间件是一个级联式（Cascading）的结构，也就是说，属于是层层调用，第一个中间件调用第二个中间件，第二个调用第三个，以此类推。上游的中间件必须等到下游的中间件返回结果，才会继续执行，这点很像递归。

app.use方法的参数就是中间件，它是一个Generator函数，最大的特征就是function命令与参数之间，必须有一个星号。Generator函数的参数next，表示下一个中间件。

Generator函数内部使用yield命令，将程序的执行权转交给下一个中间件，即yield next，要等到下一个中间件返回结果，才会继续往下执行。
 
### 路由

可以通过this.path属性，判断用户请求的路径，从而起到路由作用。

上面代码中，每一个中间件负责一个路径，如果路径不符合，就传递给下一个中间件。

复杂的路由需要安装**koa-router**插件。

Koa-router实例提供一系列动词方法，即一种HTTP动词对应一种方法。典型的动词方法有以下五种。

>router.get()

>router.post()

>router.put()

>router.del()

>router.patch()

**router.get**方法的第一个参数是根路径，第二个参数是对应的函数方法。

路径模式\users\:id的名字就是user。路径的名称，可以用来引用对应的具体路径，比如url方法可以根据路径名称，结合给定的参数，生成具体的路径。

Koa-router允许为路径统一添加前缀。

路径的参数通过*this.params*属性获取，该属性返回一个对象，所有路径参数都是该对象的成员。

### context对象

中间件当中的this表示上下文对象context，代表一次HTTP请求和回应，即一次访问/回应的所有信息，都可以从上下文对象获得。context对象封装了request和response对象，并且提供了一些辅助方法。每次HTTP请求，就会创建一个新的context对象。

context对象的全局属性。

>request：指向Request对象

>response：指向Response对象

>req：指向Node的request对象

>res：指向Node的response对象

>app：指向App对象

>state：用于在中间件传递信息。

解析POST请求的数据。
`co-body`
`https://github.com/koajs/body-parser`
`https://github.com/koajs/body-parsers`

### 错误处理机制

Koa提供内置的错误处理机制，任何中间件抛出的错误都会被捕捉到，引发向客户端返回一个500错误，而不会导致进程停止，因此也就不需要forever这样的模块重启进程。

### cookie

cookie的读取和设置。

>this.cookies.get('view');

>this.cookies.set('view', n);

>signed：cookie是否加密。

>expires：cookie何时过期

>path：cookie的路径，默认是“/”。

>domain：cookie的域名。

>secure：cookie是否只有https请求下才发送。

>httpOnly：是否只有服务器可以取到cookie，默认为true。

### Request对象
```bash
this.request.header
this.request.method
this.request.length
this.request.path
this.request.href
this.request.querystring
this.request.search
this.request.host
this.request.hostname
this.request.type
this.request.charset
this.request.query
this.request.fresh
this.request.stale
this.request.protocol
this.request.secure
this.request.ip
this.request.subdomains
this.request.is(types…)
```
### Response对象
```bash
this.response.header
this.response.socket
this.response.status
this.response.message
this.response.length
this.response.body
this.response.get(field)
this.response.set()
this.response.remove(field)
this.response.headerSent
this.response.attachment([filename])
this.response.vary(field)
```

### CSRF攻击

CSRF攻击是指用户的session被劫持，用来冒充用户的攻击。

koa-csrf插件用来防止CSRF攻击。原理是在session之中写入一个秘密的token，用户每次使用POST方法提交数据的时候，必须含有这个token，否则就会抛出错误。

* 表单的_csrf字段
* 查询字符串的_csrf字段
* HTTP请求头信息的x-csrf-token字段
* HTTP请求头信息的x-xsrf-token字段
* app.listen()就是http.createServer(app.callback()).listen(...)的缩写。

## CommonJS规范

变量x和函数addX，是当前文件example.js私有的，其他文件不可见。

如果想在多个文件分享变量，必须定义为**global**对象的属性。

CommonJS规范规定，每个模块内部，module变量代表当前模块。这个变量是一个对象，它的exports属性（即module.exports）是对外的接口。加载某个模块，其实是加载该模块的**module.exports**属性。

### CommonJS模块的特点如下

所有代码都运行在模块作用域，不会污染全局作用域。

模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。

模块加载的顺序，按照其在代码中出现的顺序。

>module.id 模块的识别符，通常是带有绝对路径的模块文件名。

>module.filename 模块的文件名，带有绝对路径。

>module.loaded 返回一个布尔值，表示模块是否已经完成加载。

>module.parent 返回一个对象，表示调用该模块的模块。

>module.children 返回一个数组，表示该模块要用到的其他模块。

>module.exports 表示模块对外输出的值。module.exports属性表示当前模块对外输出的接口，其他文件加载该模块，实际上就是读取module.exports变量。

### AMD规范与CommonJS规范的兼容性

CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是非同步加载模块，允许指定回调函数。由于Node.js主要用于服务器编程，模块文件一般都已经存在于本地硬盘，所以加载起来比较快，不用考虑非同步加载的方式，所以CommonJS规范比较适用。但是，如果是浏览器环境，要从服务器端加载模块，这时就必须采用非同步模式，因此浏览器端一般采用AMD规范。
Node使用CommonJS模块规范，内置的require命令用于加载模块文件。

**require命令**的基本功能是，读入并执行一个JavaScript文件，然后返回该模块的exports对象。如果没有发现指定模块，会报错。
require命令调用自身，等于是执行module.exports

### 加载规则

require命令用于加载文件，后缀名默认为.js。

根据参数的不同格式，require命令去不同路径寻找模块文件

1. 如果参数字符串以“/”开头，则表示加载的是一个位于绝对路径的模块文件。require('/home/marco/foo.js')将加载/home/marco/foo.js。
2. 如果参数字符串以“./”开头，则表示加载的是一个位于相对路径（跟当前执行脚本的位置相比）的模块文件。require('./circle')将加载当前脚本同一目录的circle.js。
3. 如果参数字符串不以“./“或”/“开头，则表示加载的是一个默认提供的核心模块（位于Node的系统安装目录中），或者一个位于各级node_modules目录的已安装模块（全局安装或局部安装）。脚本/home/user/projects/
foo.js执行了require('bar.js')命令，Node会依次搜索以下文件。

>/usr/local/lib/node/bar.js

>/home/user/projects/node_modules/bar.js

>/home/user/node_modules/bar.js

>/home/node_modules/bar.js

>/node_modules/bar.js

#### 目录的加载规则

我们会把相关的文件会放在一个目录里面，便于组织。这时，最好为该目录设置一个入口文件，让require方法可以通过这个入口文件，加载整个目录。

所有缓存的模块保存在require.cache之中，如果想删除模块的缓存，可以像下面这样写。

**环境变量NODE_PATH**

Node执行一个脚本时，会先查看环境变量NODE_PATH。它是一组以冒号分隔的绝对路径。在其他位置找不到指定模块时，Node会去这些路径查找。
NODE_PATH是历史遗留下来的一个路径解决方案，通常不应该使用，而应该使用node_modules目录机制。

#### 模块的加载机制

CommonJS模块的加载机制是，输入的是被输出的值的拷贝。也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。

上面代码输出内部变量counter和改写这个变量的内部方法incCounter。

**require**的内部处理流程

require命令是CommonJS规范之中，用来加载其他模块的命令。它其实不是一个全局命令，而是指向当前模块的module.require命令，而后者又调用Node的内部命令`Module._load`。

>require函数及其辅助方法主要如下

>require(): 加载外部模块

>require.resolve()：将模块名解析到一个绝对路径

>require.main：指向主模块

>require.cache：指向所有缓存的模块

>require.extensions：根据文件的后缀名，调用不同的执行函数

**函数是 JavaScript 唯一的 Local Scope**

## MongoDB的应用

MongoDB是目前最流行的noSQL数据库之一，它是专为Node开发的。

MongoDB的一条记录叫做文档（document），它是一个包含多个字段的数据结构，很类似于JSON格式。
文档储存在集合（collection）之中，类似于关系型数据库的表。在一个集合之中，记录的格式并不需要相同。每个集合之中的每个文档，必须有一个_id字段作为主键。

### Mongoose

多种中间件可以用于连接node.js与MongoDB，目前比较常用的Mongoose。

mongoose.Schema方法用来定义数据集的格式（schema），mongoose.model方法将格式分配给指定的数据集。

## DNS模块
DNS模块用于解析域名。resolve4方法用于IPv4环境，resolve6方法用于IPv6环境，lookup方法在以上两种环境都可以使用，返回IP地址（address）和当前环境（IPv4或IPv6）。

## NPM模块

npm有两层含义。一层含义是Node的开放式模块登记和管理系统，网址为npmjs.org。另一层含义是Node默认的模块管理器，是一个命令行下的软件，用来安装和管理Node模块。

npm不需要单独安装。在安装Node的时候，会连带一起安装npm。但是，Node附带的npm可能不是最新版本，最好用下面的命令，更新到最新版本。

**live-server模块**有三个功能。

启动一个HTTP服务器，展示指定目录的index.html文件，通过该文件加载各种网络资源，这是file://协议做不到的。
添加自动刷新功能。只要指定目录之中，文件有任何变化，它就会刷新页面。
npm run serve命令执行以后，自动打开浏览器。

## os模块

os.EOL属性是一个常量，返回当前操作系统的换行符（Windows系统是\r\n，其他系统是\n）。

os.arch方法返回当前计算机的架构。

## Stream接口

数据读写可以看作是事件模式（Event）的特例，不断发送的数据块好比一个个的事件。读数据是read事件，写数据是write事件，而数据块是事件附带的信息。Node 为这类情况提供了一个特殊接口Stream。

”数据流“（stream）是处理系统缓存的一种方式。操作系统采用数据块（chunk）的方式读取数据，每收到一次数据，就存入缓存。Node应用程序有两种缓存的处理方式，第一种是等到所有数据接收完毕，一次性从缓存读取，这就是传统的读取文件的方式；第二种是采用“数据流”的方式，收到一块数据，就读取一块，即在数据还没有接收完成时，就开始处理它。

第一种方式先将数据全部读入内存，然后处理，优点是符合直觉，流程非常自然，缺点是如果遇到大文件，要花很长时间，才能进入数据处理的步骤。第二种方式每次只读入数据的一小块，像“流水”一样，每当系统读入了一小块数据，就会触发一个事件，发出“新数据块”的信号。应用程序只要监听这个事件，就能掌握数据读取的进展，做出相应处理，这样就提高了程序的性能。

Unix操作系统从很早以前，就有“数据流”这个概念，它是不同进程之间传递数据的一种方式。管道命令（pipe）就起到在不同命令之间，连接数据流的作用。“数据流”相当于把较大的数据拆成很小的部分，一个命令只要部署了数据流接口，就可以把一个流的输出接到另一个流的输入。Node引入了这个概念，通过数据流接口为异步读写数据提供的统一接口，无论是硬盘数据、网络数据，还是内存数据，都可以采用这个接口读写。

数据流接口最大特点就是通过事件通信，具有readable、writable、drain、data、end、close等事件，既可以读取数据，也可以写入数据。读写数据时，每读入（或写入）一段数据，就会触发一次data事件，全部读取（或写入）完毕，触发end事件。如果发生错误，则触发error事件。

* 文件读写
* HTTP 请求的读写
* TCP 连接
* 标准输入输出

### 可读数据流

* 可读数据流接口，用于对外提供数据。
* 可写数据流接口，用于写入数据。
* 双向数据流接口，用于读取和写入数据，比如Node的tcp sockets、zlib、crypto都部署了这个接口。

“可读数据流”有两种状态：流动态和暂停态。处于流动态时，数据会尽快地从数据源导向用户的程序；处于暂停态时，必须显式调用stream.read()等指令，“可读数据流”才会释放数据。刚刚新建的时候，“可读数据流”处于暂停态。

*三种方法可以让暂停态转为流动态。*

>添加data事件的监听函数

>调用resume方法

>调用pipe方法将数据送往一个可写数据流

*两种方法可以让流动态转为暂停态。*

>不存在pipe方法的目的地时，调用pause方法

>存在pipe方法的目的地时，移除所有data事件的监听函数，并且调用unpipe方法，移除所有pipe方法的目的地

Readable.pause() ：暂停数据流。已经存在的数据，也不再触发data事件，数据将保留在缓存之中，此时的数据流称为静态数据流。如果对静态数据流再次调用pause方法，数据流将重新开始流动，但是缓存中现有的数据，不会再触发data事件。

Readable.resume()：恢复暂停的数据流。

readable.unpipe()：从管道中移除目的地数据流。如果该方法使用时带有参数，会阻止“可读数据流”进入某个特定的目的地数据流。如果使用时不带有参数，则会移除所有的目的地数据流。

### 继承可读数据流接口
### 可写数据流
事件
>drain事件

>finish事件

>pipe事件

>unpipe事件

### pipe方法
### 转换数据流

### HTTP请求

HTTP对象使用Stream接口，实现网络数据的读写。