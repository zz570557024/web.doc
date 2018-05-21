# 其它

## 浏览器优化

* 页面缓存

*单个设定*

单个设定就是我们常见的，在html的meta区域设置cache-control，max-age，expires等。应用服务器在解析到这些参数之后，会在响应报文中明确指出，告诉浏览器当前页面的缓存规则。

*批量设定*

    * 过滤器
	对同一类页面元素实施相同的缓存策略，可以使用过滤器来实现，通过判定缓存对象类型，在相应中添加相同的缓存规则。

	* eTags
	IE前进，后退
	一般来说这种重新浏览方式数据都是取自缓存。比如Cache-control设定为private、must-revalidate、max-age的内容。Cache-control为no-cache，还需要再次请求服务器，因为这种情况是不会在浏览器缓存区留下任何痕迹。

	IE新窗口，如链接打开
	如果页面指定cache-control的值为private、no-cache、must-revalidate，那么打开新窗口访问时都会重新访问服务器。如果指定了max-age值，那么在此值内的时间里就不会重新访问服务器。

	IE刷新
	不管cache-control的为何值，都会检查服务器。检查要访问网页的最新版本，如果存在最新版本，则返回最新网页的内容，并更新缓存；如果不存在最新版本，则只返回标题，内容去缓存中获取。

	IE强制刷新（CTRL+F5）
	获取要访问网页的服务器最新版本，返回最新的版本，并删除缓存中的相应内容，将最新的内容写入缓存。其实强制刷新情况下，请求的HTTP报文头里，Cache-control为no-cache。

	脱机工作
	这是一种比较特殊的重新浏览方式，这种情况下是没有任何面向服务器的请求过程的，直接去浏览器的缓存寻找数据，如果缓存没有命中，脱机就没办法工作了。显而易见的是，no-cache方式，是没办法脱机工作的。

* 在action，使用如下声明
```bash
  response.setHeader("Pragma","No-cache");
  response.setHeader("Cache-Control","no-cache"); 
  response.setDateHeader("Expires", 0);
```
* 随机参数document.write(`"<script src='test.js?rnd="+Math.random()+"'></s"+"cript>"`)。

* jquery ajax清除浏览器缓存的两种方法:

1. 通过$.ajaxSetup 设置属性cache:false,让ajax不调用浏览的缓存.
     jQuery.ajaxSetup ({cache:false})
 
2. 可以在ajax的url后加上随机串来避免浏览缓存,如`$.ajax({url:'test.php?'+parseInt(Math.random()*100000)})`缓存

* 相对路径-顾名思义，相对路径就是相对于当前文件的路径。网页中一般表示路径使用这个方法。
* 绝对路径-绝对路径就是你的主页上的文件或目录在硬盘上真正的路径。绝对路径就是你的主页上的文件或目录在硬盘上真正的路径，比如，你的Perl 程序是存放在 c:/apache/cgi-bin 下的，那么 `c:/apache/cgi-bin`就是cgi-bin目录的绝对路径

`
"./"：代表目前所在的目录。
"../"：代表上一层目录。
以"/"开头：代表根目录。`

> 浏览器是好人（meta），

> 浏览器有点坏（版本号），

> 浏览器老子看你不爽了，我要“欺骗你”（md5）！

