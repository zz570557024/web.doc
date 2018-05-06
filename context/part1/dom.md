# DOM/BOM相关

## DOM

* DOM是JavaScript操作网页的接口，全称为“文档对象模型”（Document Object Model）。它的作用是将网页转为一个JavaScript对象，从而可以用脚本进行各种操作（比如增删内容）。
浏览器会根据DOM模型，将结构化文档（比如HTML和XML）解析成一系列的节点，再由这些节点组成一个树状结构（DOM Tree）。
	* Document：整个文档树的顶层节点
	* DocumentType：doctype标签
	* Element：网页的各种HTML标签
	* Attribute：网页元素的属性（比如class="right"）
	* Text：标签之间或标签包含的文本
	* Comment：注释
	* DocumentFragment：文档的片段

> * 父节点关系（parentNode）：直接的那个上级节点
> * 子节点关系（childNodes）：直接的下级节点
> * 同级节点关系（sibling）：拥有同一个父节点的节点

* DOM提供操作接口，用来获取三种关系的节点。其中，子节点接口包括firstChild（第一个子节点）和lastChild（最后一个子节点）等属性，同级节点接口包括nextSibling（紧邻在后的那个同级节点）和previousSibling（紧邻在前的那个同级节点）属性。

****************************************************************************************************
****************************************************************************************************

### Node对象

#### Node.nodeName，Node.nodeType

* nodeName属性返回节点的名称，nodeType属性返回节点类型的常数值。
* Node.nodeValue属性返回一个字符串，表示当前节点本身的文本值，该属性可读写。由于只有Text节点、Comment节点、XML文档的CDATA节点有文本值，因此只有这三类节点的nodeValue可以返回结果，其他类型的节点一律返回null

#### Node.textContent

* Node.textContent属性返回当前节点和它的所有后代节点的文本内容。
* Node.baseURI属性返回一个字符串，表示当前网页的绝对路径。如果无法取到这个值，则返回null。浏览器根据这个属性，计算网页上的相对路径的URL。该属性为只读。
* 该属性的值一般由当前网址的URL（即window.location属性）决定，但是可以使用HTML的<base>标签，改变该属性的值。

#### Node.parentNode

* parentNode属性返回当前节点的父节点。对于一个节点来说，它的父节点只可能是三种类型：element节点、document节点和documentfragment节点。

#### Node.parentElement

* parentElement属性返回当前节点的父Element节点。如果当前节点没有父节点，或者父节点类型不是Element节点，则返回null。
* 在IE浏览器中，只有Element节点才有该属性，其他浏览器则是所有类型的节点都有该属性。
firstChild属性返回当前节点的第一个子节点，如果当前节点没有子节点，则返回null（注意，不是undefined）

#### 节点对象的方法

* Node.appendChild方法接受一个节点对象作为参数，将其作为最后一个子节点，插入当前节点。
* Node.hasChildNodes方法返回一个布尔值，表示当前节点是否有子节点。
- Node.insertBefore方法用于将某个节点插入当前节点的指定位置。它接受两个参数，第一个参数是所要插入的节点，第二个参数是当前节点的一个子节点，新的节点将插在这个节点的前面。该方法返回被插入的新节点
- Node.removeChild方法接受一个子节点作为参数，用于从当前节点移除该子节点。它返回被移除的子节点。
- Node.contains方法接受一个节点作为参数，返回一个布尔值，表示参数节点是否为当前节点的后代节点。
- compareDocumentPosition方法的用法，与contains方法完全一致，返回一个7个比特位的二进制值，表示参数节点与当前节点的关系。
- isEqualNode方法返回一个布尔值，用于检查两个节点是否相等。所谓相等的节点，指的是两个节点的类型相同、属性相同、子节点相同。
- normailize方法用于清理当前节点内部的所有Text节点。它会去除空的文本节点，并且将毗邻的文本节点合并成一个。
节点都是单个对象，有时会需要一种数据结构，能够容纳多个节点。DOM提供两种集合对象，用于实现这种节点的集合：NodeList和HTMLCollection。
- NodeList实例对象是一个类似数组的对象，它的成员是节点对象。Node.childNodes、document.querySelectorAll()返回的都是NodeList实例对象。遍历NodeList实例对象的首选方法，是使用for循环。
ES6新增的for...of循环，也可以正确遍历NodeList实例对象。
HTMLCollection实例对象与NodeList实例对象类似，也是节点的集合，返回一个类似数组的对象。document.links、docuement.forms、document.images等属性，返回的都是HTMLCollection实例对象。

#### ParentNode接口

* ParentNode接口用于获取Element子节点。Element节点、Document节点和DocumentFragment节点，部署了ParentNode接口。

#### ChildNode接口

* ChildNode接口用于处理子节点（包含但不限于Element子节点）。Element节点、DocumentType节点和CharacterData接口，部署了ChildNode接口。
* 方法： remove() before() after() replaceWith()

****************************************************************************************************
****************************************************************************************************

### Document对象

* document节点是文档的根节点，每张网页都有自己的document节点。window.document属性就指向这个节点。只要浏览器开始载入HTML文档，这个节点对象就存在了，可以直接调用。
* 对于正常的网页，直接使用document或window.document。对于iframe载入的网页，使用iframe节点的contentDocument属性。
* 对Ajax操作返回的文档，使用XMLHttpRequest对象的responseXML属性。
* 对于包含某个节点的文档，使用该节点的ownerDocument属性。
* document.defaultView属性，在浏览器中返回document对象所在的window对象，否则返回null。
* document.head属性返回当前文档的<head>节点，document.body属性返回当前文档的<body>。
* document.activeElement属性返回当前文档中获得焦点的那个元素。用户通常可以使用Tab键移动焦点，使用空格键激活焦点。
* document.links属性返回当前文档所有设定了href属性的a及area元素。
* document.forms属性返回页面中所有表单元素form。
* document.images属性返回页面所有图片元素（即img标签）
* document.embeds属性返回网页中所有嵌入对象，即embed标签。
* document.scripts属性返回当前文档的所有脚本（即script标签）。
* document.styleSheets属性返回一个类似数组的对象，代表当前网页的所有样式表。每个样式表对象都有cssRules属性，返回该样式表的所有CSS规则，这样这可以操作具体的CSS规则了。
* document.documentURI属性和document.URL属性都返回一个字符串，表示当前文档的网址。不同之处是documentURI属性可用于所有文档（包括 XML 文档），URL属性只能用于 HTML 文档。
* document.domain属性返回当前文档的域名。
* document.lastModified属性返回当前文档最后修改的时间戳，格式为字符串。Date.parse方法转成时间戳格式，才能进行比较。
> location.assign()
> location.reload()
> location.toString()
`document.location = '#top'`
* 如果指定的是锚点，浏览器会自动滚动到锚点处。
* 如果不希望用户看到前一个网页，可以使用location.replace方法，浏览器history对象就会用新的网址，取代当前网址，这样的话，“后退”按钮就不会回到当前网页了。它的一个应用就是，当脚本发现当前是移动设备时，就立刻跳转到移动版网页。
* document.referrer属性返回一个字符串，表示当前文档的访问来源，如果是无法获取来源或是用户直接键入网址，而不是从其他网页点击，则返回一个空字符串。
* document.referrer的值，总是与HTTP头信息的Referer保持一致，但是它的拼写有两个r
* document.title属性返回当前文档的标题，该属性是可写的。
* document.readyState属性返回当前文档的状态：
> loading：加载HTML代码阶段（尚未完成解析）
> interactive：加载外部资源阶段时
> complete：加载完成时
* 浏览器开始解析HTML文档，document.readyState属性等于loading。
* HTML文档解析完成，document.readyState属性变成interactive。
* 浏览器等待图片、样式表、字体文件等外部资源加载完成，一旦全部加载完成，document. readyState属性变成complete。
* document.designMode属性控制当前文档是否可编辑，通常用在制作所见即所得编辑器。打开iframe元素包含的文档的designMode属性，就能将其变为一个所见即所得的编辑器。
* compatMode属性返回浏览器处理文档的模式，可能的值为BackCompat（向后兼容模式）和CSS1Compat（严格模式）。一般来说，如果网页代码的第一行设置了明确的DOCTYPE（比如<!doctype html>），document.compatMode的值都为CSS1Compat。document.cookie属性用来操作浏览器Cookie
* document.open方法用于新建一个文档，供write方法写入内容。它实际上等于清除当前文档，重新写入内容。不要将此方法与window.open()混淆，后者用来打开一个新窗口，与当前文档无关。
document.write方法用于向当前文档写入内容。只要当前文档还没有用close方法关闭，它所写入的内容就会追加在已有内容的后面。document.querySelector方法接受一个CSS选择器作为参数，返回匹配该选择器的元素节点。如果有多个节点满足匹配条件，则返回第一个匹配的节点。如果没有发现匹配的节点，则返回null。
* document.querySelectorAll方法与querySelector用法类似，区别是返回一个NodeList对象，包含所有匹配给定选择器的节点。
* 这两个方法都支持复杂的CSS选择器。
* 不支持CSS伪元素的选择器（比如:first-line和:first-letter）和伪类的选择器（比如:link和:visited），即无法选中伪元素和伪类。
* document.getElementsByTagName方法返回所有指定HTML标签的元素，返回值是一个类似数组的HTMLCollection对象
* HTML元素本身也定义了getElementsByTagName方法，返回该元素的后代元素中符合指定标签的元素。也就是说，这个方法不仅可以在document对象上调用，也可以在任何元素节点上调用。
* document.getElementsByClassName方法返回一个类似数组的对象（HTMLCollection实例对象），包括了所有class名字符合指定条件的元素，元素的变化实时反映在返回结果中。
* document.getElementsByName方法用于选择拥有name属性的HTML元素（比如<form>、<radio>、<img>、<frame>、<embed>和<object>等），返回一个类似数组的的对象（NodeList对象的实例），因为name属性相同的元素可能不止一个。
* getElementById方法返回匹配指定id属性的元素节点。如果没有发现匹配的节点，则返回null。
* document.elementFromPoint方法返回位于页面指定位置最上层的Element子节点。
* document.createElement方法用来生成网页元素节点。
* createElement方法的参数为元素的标签名，即元素节点的tagName属性，对于 HTML 网页大小写不敏感，即参数为div或DIV返回的是同一种节点。如果参数里面包含尖括号（即<和>）会报错。
* document.createTextNode方法用来生成文本节点，参数为所要生成的文本节点的内容。
`var newDiv = document.createElement('div');var newContent =document.createTextNode('Hello');
newDiv.appendChild(newContent)`
* document.createAttribute方法生成一个新的属性对象节点，并返回它。
* document.createNodeIterator方法返回一个DOM的子节点遍历器。所谓“遍历器”，在这里指可以用nextNode方法和previousNode方法依次遍历根节点的所有子节点。
* document.createTreeWalker方法返回一个DOM的子树遍历器。它与createNodeIterator方法的区别在于，后者只遍历子节点，而它遍历整个子树。
* document.adoptNode方法将某个节点，从其原来所在的文档移除，插入当前文档，并返回插入后的新节点
* document.importNode方法从外部文档拷贝指定节点，插入当前文档。importNode方法只是拷贝外部节点，这时该节点的父节点是null。下一步还必须将这个节点插入当前文档的DOM树。
`document.getElementById("container").appendChild(newNode)`
* Element对象对应网页的HTML标签元素。每一个HTML标签元素，在DOM树上都会转化成一个Element节点对象（以下简称元素节点）。

****************************************************************************************************
****************************************************************************************************

### 盒状模型相关属性

* Element.clientHeight属性返回元素节点可见部分的高度，Element.clientWidth属性返回元素节点可见部分的宽度。所谓“可见部分”，指的是不包括溢出（overflow）的大小，只返回该元素在容器中占据的大小，对于有滚动条的元素来说，它们等于滚动条围起来的区域大小。这两个属性的值包括Padding、但不包括滚动条、边框和Margin，单位为像素。
* Element.clientLeft属性等于元素节点左边框（left border）的宽度，Element.clientTop属性等于网页元素顶部边框的宽度，单位为像素。
* Element.scrollHeight属性返回某个网页元素的总高度，Element.scrollWidth属性返回总宽度，可以理解成元素在垂直和水平两个方向上可以滚动的距离。
* 存在溢出时，当滚动条滚动到内容底部时，下面的表达式为true。
`element.scrollHeight - element.scrollTop === element.clientHeight`
* 如果滚动条没有滚动到内容底部，上面的表达式为false。这个特性结合onscroll事件，可以判断用户是否滚动到了指定元素的底部，比如向用户展示某个内容区块时，判断用户是否滚动到了区块的底部。
* Element.offsetHeight属性返回元素的垂直高度，Element.offsetWidth属性返回水平宽度。
* Element.children属性返回一个HTMLCollection对象，包括当前元素节点的所有子元素。它是一个类似数组的动态对象（实时反映网页元素的变化）。如果当前元素没有子元素，则返回的对象包含零个成员。
* Element.addEventListener()：添加事件的回调函数
* Element.removeEventListener()：移除事件监听函数
* Element.dispatchEvent()：触发事件
* Element.scrollIntoView方法滚动当前元素，进入浏览器的可见区域，类似于设置window.location.hash的效果。
* 该方法可以接受一个布尔值作为参数。如果为true，表示元素的顶部与当前区域的可见部分的顶部对齐（前提是当前区域可滚动）；如果为false，表示元素的底部与当前区域的可见部分的尾部对齐（前提是当前区域可滚动）。如果没有提供该参数，默认为true。
* Element.getBoundingClientRect方法返回一个对象，该对象提供当前元素节点的大小、位置等信息，基本上就是CSS盒状模型提供的所有信息。
* 元素相对于视口（viewport）的位置，会随着页面滚动变化，因此表示位置的四个属性值，都不是固定不变的。如果想得到绝对位置，可以将left属性加上window.scrollX，top属性加上window.scrollY。
* text/javascript：这是默认值，也是历史上一贯设定的值。如果你省略type属性，默认就是这个值。对于老式浏览器，设为这个值比较好。
`application/javascript：对于较新的浏览器，建议设为这个值。
浏览器一边下载HTML网页，一边开始解析
解析过程中，发现<script>标签
暂停解析，网页渲染的控制权转交给JavaScript引擎
如果<script>标签引用了外部脚本，就下载该脚本，否则就直接执行执行完毕，控制权交还渲染引擎，恢复往下解析HTML网页`
* 加载外部脚本时，浏览器会暂停页面渲染，等待脚本下载并执行完成后，再继续渲染。原因是JavaScript可以修改DOM（比如使用document.write方法），所以必须把控制权让给它，否则会导致复杂的线程竞赛的问题。
`为了避免这种情况，较好的做法是将<script>标签都放在页面底部，而不是头部。这样即使遇到脚本失去响应，网页主体的渲染也已经完成了，用户至少可以看到内容，而不是面对一张空白的页面。`
* 如果某些脚本代码非常重要，一定要放在页面头部的话，最好直接将代码嵌入页面，而不是连接外部脚本文件，这样能缩短加载时间。为了解决脚本文件下载阻塞网页渲染的问题，一个方法是加入defer属性。
`浏览器开始解析HTML网页
解析过程中，发现带有defer属性的script标签
浏览器继续往下解析HTML网页，同时并行下载script标签中的外部脚本
浏览器完成解析HTML网页，此时再执行下载的脚本`
* **解决“阻塞效应”的另一个方法是加入async属性。**
* async属性的作用是，使用另一个进程下载脚本，下载时不会阻塞渲染。
* async属性可以保证脚本下载的同时，浏览器继续渲染。需要注意的是，一旦采用这个属性，就无法保证脚本的执行顺序。哪个脚本先下载结束，就先执行那个脚本。另外，使用async属性的脚本文件中，不应该使用document.write方法。

****************************************************************************************************
****************************************************************************************************

## BOM

* **浏览器的核心是两部分：渲染引擎和JavaScript解释器（又称JavaScript引擎）。**

> Firefox：Gecko引擎
> Safari：WebKit引擎
> Chrome：Blink引擎
> IE: Trident引擎
> Edge: EdgeHTML引擎

* **渲染引擎处理网页，通常分成四个阶段：**

> * 解析代码：HTML代码解析为DOM，CSS代码解析为CSSOM（CSS Object Model）
> * 对象合成：将DOM和CSSOM合成一棵渲染树（render tree）
> * 布局：计算出渲染树的布局（layout）
> * 绘制：将渲染树绘制到屏幕

#### 重流和重绘

* 渲染树转换为网页布局，称为“布局流”（flow）；布局显示到页面的这个过程，称为“绘制”（paint）。它们都具有阻塞效应，并且会耗费很多时间和计算资源。
* 页面生成以后，脚本操作和样式表操作，都会触发重流（reflow）和重绘（repaint）。用户的互动，也会触发，比如设置了鼠标悬停（a:hover）效果、页面滚动、在输入框中输入文本、改变窗口大小等等
* 读取DOM或者写入DOM，尽量写在一起，不要混杂
* 缓存DOM信息
* 不要一项一项地改变样式，而是使用CSS class一次性改变样式使用document fragment操作DOM动画时使用absolute定位或fixed定位，这样可以减少对其他元素的影响,只在必要时才显示元素
* 使用window.requestAnimationFrame()，因为它可以把代码推迟到下一次重流时执行，而不是立即要求页面重流
使用虚拟DOM（virtual DOM）库
* **JavaScript是一种解释型语言**，也就是说，它不需要编译，由解释器实时运行。这样的好处是运行和修改都比较方便，刷新页面就可以重新解释；缺点是每次运行都要调用解释器，系统开销较大，运行速度慢于编译型语言。
* 读取代码，进行词法分析（Lexical analysis），将代码分解成词元（token）。
* 对词元进行语法分析（parsing），将代码整理成“语法树”（syntax tree）。
* 使用“翻译器”（translator），将代码转为字节码（bytecode）
使用“字节码解释器”（bytecode interpreter），将字节码转为机器码。
* 逐行解释将字节码转为机器码，是很低效的。为了提高运行速度，现代浏览器改为采用“即时编译”（Just In Time compiler，缩写JIT），即字节码只在运行时编译，用到哪一行就编译哪一行，并且把编译结果缓存（inline cache）。
* 字节码不能直接运行，而是运行在一个虚拟机（Virtual Machine）之上，*一般也把虚拟机称为JavaScript引擎。*

> Chakra(Microsoft Internet Explorer)
> Nitro/JavaScript Core (Safari)
> Carakan (Opera)
> SpiderMonkey (Firefox)
> V8 (Chrome, Chromium)

****************************************************************************************************
****************************************************************************************************

### History 对象

* history.go(0)相当于刷新当前页面。
`window.history.back()`
* 返回上一页时，页面通常是从浏览器缓存之中加载，而不是重新要求服务器发送新的网页。
* pushState方法不会触发页面刷新，只是导致history对象发生变化，地址栏会有反应。
* **URLSearchParams有以下方法，用来操作某个参数。**

> has()：返回一个布尔值，表示是否具有某个参数
> get()：返回指定参数的第一个值
> getAll()：返回一个数组，成员是指定参数的所有值
> set()：设置指定参数
> delete()：删除指定参数
> append()：在查询字符串之中，追加一个键值对
> toString()：返回整个查询字符串
> keys()：遍历所有参数名
> values()：遍历所有参数值
> entries()：遍历所有参数的键值对

****************************************************************************************************
****************************************************************************************************

### Cookie

* Cookie 是服务器保存在浏览器的一小段文本信息，每个 Cookie 的大小一般不能超过4KB。浏览器每次向服务器发出请求，就会自动附上这段信息。

> Cookie的名字
> Cookie的值
> 到期时间
> 所属域名（默认是当前域名）
> 生效的路径（默认是当前网址）

* 浏览器可以设置不接受 Cookie，也可以设置不向服务器发送 Cookie。

#### expires属性

* 如果不设置该属性，或者设为null，Cookie只在当前会话（session）有效，浏览器窗口一旦关闭，当前Session结束，该Cookie就会被删除。

#### secure 属性

* secure属性用来指定Cookie只能在加密协议HTTPS下发送到服务器。
* 该属性只是一个开关，不需要指定值。如果通信是HTTPS协议，该开关自动打开。

#### max-age

* max-age属性用来指定Cookie有效期，比如60 * 60 * 24 * 365（即一年31536e3秒）。

#### HttpOnly

* HttpOnly属性用于设置该Cookie不能被JavaScript读取
只要有一个属性不同，就会生成一个全新的Cookie，而不是替换掉原来那个Cookie。
* **删除一个Cookie的简便方法，就是设置expires属性等于0，或者等于一个过去的日期。**
`document.cookie = 'fontSize=;expires=Thu, 01-Jan-1970 00:00:01 GMT'`
* 名为fontSize的Cookie的值为空，过期时间设为1970年1月1月零点，就等同于删除了这个Cookie。

#### 同源政策

* 浏览器的同源政策规定，两个网址只要域名相同和端口相同，就可以共享Cookie。
* 这个API的作用是，使得网页可以在浏览器端储存数据。它分成两类：**sessionStorage**和**localStorage**。
* sessionStorage保存的数据用于浏览器的一次会话，当会话结束（通常是该窗口关闭），数据被清空；localStorage保存的数据长期存在，下一次访问该网站的时候，网页可以直接读取以前保存的数据。除了保存期限的长短不同，这两个对象的属性和方法完全一样。
* A 网页设置的 Cookie，B 网页不能打开，除非这两个网页“同源”。所谓“同源”指的是”三个相同“。

> * 协议相同
> * 域名相同
> * 端口相同

* 同源政策的目的，是为了保证用户信息的安全，防止恶意的网站窃取数据。
* 如果 Cookie 包含隐私（比如存款总额），这些信息就会泄漏。更可怕的是，Cookie 往往用来保存用户的登录状态，如果用户没有退出登录，其他网站就可以冒充用户，为所欲为。因为浏览器同时还规定，提交表单不受同源政策的限制。
* “同源政策”是必需的，否则 Cookie 可以共享，互联网就毫无安全可言了。

#### 限制范围

* 随着互联网的发展，“同源政策”越来越严格。目前，如果非同源，共有三种行为受到限制。

> Cookie、LocalStorage 和 IndexedDB 无法读取。
> DOM 无法获得。
> AJAX 请求无效（可以发送，但浏览器会拒绝接受响应）。

* 同源政策规定，AJAX请求只能发给同源的网址，否则就报错。
* 除了架设服务器代理（浏览器请求同源服务器，再由后者请求外部服务），有三种方法规避这个限制。

> JSONP
> WebSocket
> CORS

`它的基本思想是，网页通过添加一个<script>元素，向服务器请求JSON数据，这种做法不受同源政策限制；服务器收到请求后，将数据放在一个指定名字的回调函数里传回来。`
* WebSocket是一种通信协议，使用ws://（非加密）和wss://（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。
* 有一个字段是Origin，表示该请求的请求源（origin），即发自哪个域名。正是因为有了Origin这个字段，所以WebSocket才没有实行同源政策。因为服务器可以根据这个字段，判断是否许可本次通信。如果该域名在白名单内，服务器就会做出如下回应。
* **CORS是跨源资源分享（Cross-Origin Resource Sharing）的缩写。**它是W3C标准，是跨源AJAX请求的根本解决方法。相比JSONP只能发GET请求，CORS允许任何类型的请求。
* 实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。
* 浏览器将CORS请求分成两类：简单请求（simple request）和非简单请求（not-so-simple request）。
* **如果Origin指定的源，不在许可范围内，服务器会返回一个正常的HTTP回应。浏览器发现，这个回应的头信息没有包含Access-Control-Allow-Origin字段（详见下文），就知道出错了，从而抛出一个错误，被XMLHttpRequest的onerror回调函数捕获。注意，这种错误无法通过状态码识别，因为HTTP回应的状态码有可能是200。**
* 非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为“预检”请求（preflight）。

****************************************************************************************************
****************************************************************************************************

### AJAX

* 浏览器与服务器之间，采用HTTP协议通信。用户在浏览器地址栏键入一个网址，或者通过网页表单向服务器提交内容，这时浏览器就会向服务器发出HTTP请求。
* 1999年，微软公司发布IE浏览器5.0版，第一次引入新功能：允许JavaScript脚本向服务器发起HTTP请求。2005年2月，AJAX这个词第一次正式提出，指围绕这个功能进行开发的一整套做法。从此，AJAX成为脚本发起HTTP通信的代名词，W3C也在2006年发布了它的国际标准。

> 创建AJAX对象
> 发出HTTP请求
> 接收服务器传回的数据
> 更新网页数据

* AJAX通过原生的XMLHttpRequest对象发出HTTP请求，得到服务器返回的数据后，再进行处理。

#### XMLHttpRequest实例的属性

* readyState是一个只读属性，用一个整数和对应的常量，表示XMLHttpRequest请求当前所处的状态。

> 0，对应常量UNSENT，表示XMLHttpRequest实例已经生成，但是open()方法还没有被调用。
> 1，对应常量OPENED，表示send()方法还没有被调用，仍然可以使用setRequestHeader()，设定HTTP请求的头信息。
> 2，对应常量HEADERS_RECEIVED，表示send()方法已经执行，并且头信息和状态码已经收到。
> 3，对应常量LOADING，表示正在接收服务器传来的body部分的数据，如果responseType属性是text或者空字符串，responseText就会包含已经收到的部分信息。
> 4，对应常量DONE，表示服务器数据已经完全接收，或者本次接收已经失败了。

* **状态码**

| Num           | state              | 状态     |
|:------------- |:------------------:| --------:|
| 200           | OK                 | 访问正常 |
| 301           | Moved Permanently  | 永久移动 |
| 302           | Move temporarily   | 暂时移动 |
| 304           | Not Modified       | 未修改   |
| 307           | Temporary Redirect | 暂时重定向 |
| 401           | Unauthorized       | 未授权 |
| 403           | Forbidden          | 禁止访问 |
| 404           | Not Found          | 未发现指定网址 |
| 500           | Internal Server Error| 服务器发生错误 |

* **timeout**属性等于一个整数，表示多少毫秒后，如果请求仍然没有得到结果，就会自动终止。如果该属性等于0，就表示没有时间限制。
* XMLHttpRequest对象的open方法用于指定发送HTTP请求的参数，它的使用格式如下，一共可以接受五个参数。

> method：表示HTTP动词，比如“GET”、“POST”、“PUT”和“DELETE”。
> url: 表示请求发送的网址。
> async: 格式为布尔值，默认为true，表示请求是否为异步。如果设为false，则send()方法只有等到收到服务器返回的结果，才会有返回值。
> user：表示用于认证的用户名，默认为空字符串。
> password：表示用于认证的密码，默认为空字符串。

* 所有XMLHttpRequest的监听事件，都必须在send()方法调用之前设定。
* FormData类型可以用于构造表单数据，然后使用send方法发送。它的效果与点击下面表单的submit按钮是一样的。
* FormData对象还可以对现有表单添加数据，这为我们操作表单提供了极大的灵活性。
* FormData对象也能用来模拟File控件，进行文件上传。
* FormData也可以加入JavaScript生成的文件

#### setRequestHeader()

* setRequestHeader方法用于设置HTTP头信息。该方法必须在open()之后、send()之前调用。如果该方法多次调用，设定同一个字段，则每一次调用的值会被合并成一个单一的值发送。
* 该方法用来指定服务器返回数据的MIME类型。该方法必须在send()之前调用。
* 上传文件时，XMLHTTPRequest对象的upload属性有一个progress，会不断返回上传的进度。假定网页上有一个progress元素。

#### 文件上传

`HTML网页的<form>元素能够以四种格式，向服务器发送数据。`
> 使用POST方法，将enctype属性设为application/x-www-form-urlencoded，这是默认方法。
> 使用POST方法，将enctype属性设为text/plain
> 使用POST方法，将enctype属性设为multipart/form-data。
> 使用GET方法，enctype属性将被忽略。

* FormData对象的append方法，除了可以添加文件，还可以添加二进制对象（Blob）或者字符串。
* append方法的第一个参数是表单的控件名，第二个参数是实际的值，第三个参数是可选的，通常是文件名。
* *各大浏览器（包括IE 10）都支持Ajax上传文件。*
* 除了使用FormData接口上传，也可以直接使用**File API**上传。
* Fetch API是一种新规范，用来取代XMLHttpRequest对象。
* Fetch API最大的特点是，除了返回Promise对象，还有一点就是数据传送是以数据流（stream）的形式进行的。对于大文件，数据是一段一段得到的。
* Fetch API提供以下五个数据流读取器。

> .text()：返回字符串
> .json()：返回一个JSON对象
> .formData()：返回一个FormData对象
> .blob()：返回一个blob对象
> .arrayBuffer()：返回一个二进制数组

* 数据流只能读取一次，一旦读取，数据流就空了。再次读取就不会得到结果。解决方法是在读取之前，先使用.clone()方法，复制一份一模一样的副本。
* fetch方法的第一个参数可以是URL字符串，也可以是后文要讲到的Request对象实例。Fetch方法返回一个Promise对象，并将一个response对象传给回调函数。
* response对象有一个ok属性，如果返回的状态码在200到299之间（即请求成功），这个属性为true，否则为false。