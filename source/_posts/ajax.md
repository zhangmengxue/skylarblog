title: 再看Ajax
date: 2015-02-01 14:00:35
categories: 
- articles
tags: 
- 博客
---

再回顾Ajax相关的内容，再次梳理学习还是很有必要的，尤其是实际的开发中，ajax更是必不可少，仔细学习以便避免不必要的错误。

##一.使用XMLHttpRequest

从Ajax的工作原理来看，它是一种可以使用脚本操纵HTTP和Web服务器进行数据交换并且不会导致页面重载的技术。Ajax的核心就是XMLHttpRequest对象。

	var xhr = new XMLHttpRequest();

这样就实例化了一个XMLHttpRequest对象，每个xhr对象都表示一个独立的请求响应对，通过这个对象的属性和方法我们可以查看请求的细节和得到响应的数据。

1.1 必备知识点

下面举个梨子：

	function getText(url,callback){
        var xhr = new XMLHttpRequest();
        xhr.open('GET',url);
        xhr.onreadystatechange = function(){
          if(xhr.readyState === 4 && xhr.status === 200){
            var type = xhr.getResponseHeader('Content-Type');
            if(type.match(/^text/)){
              callback(xhr.responseText);
            }
         }
       };
       xhr.send(null);
     }
     function callback(obj){
       console.log(obj);
     }


从中可以看到一些xhr对象的方法和属性：

---  xhr.setRequestHeader("Content-Type","text/plain");它用来设置请求头的信息，post请求通常会设置前面的Content-Type的请求头，比如说我想跨域发送请求，那你可能就要设置xhr.setRequestHeader("Origin","url")了(要真的跨域还需要在服务器端设置Access-Control-Allow-Origin)。但是并不能设置所有的请求头信息，你可以设置除了下面这些头之外的信息：Accept-Charset,Accept-Encoding,Connection,Content-Length,Cookie,Cookie2,Content-Transfer-Encoding,Date,Expect,Host,Keep-Alive,Referrer,User-Agent,Trailer,Transfer-Encoding,Upgrade,Via.

---  xhr.getResponseHeader()和xhr.getAllResponseHeader()用来查询响应头信息。

---  xhr.status(数字)和xhr.statusText(文本)返回HTTP状态码(200,404等)；

---  xhr.responseText(文本形式)和xhr.responseXML(document形式)得到响应主体；

---  xhr.readyState得到一个整数，指定了HTTP请求的状态，有如下对应关系：

	+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
	常量                            值                    含义   
	-----------------------------------------------------------------------------
	UNSENT                         0                    open()尚未调用
	OPENED                         1                    open()已调用
	HEADERS_RECEIVED    　　　　　   2                    接收到头信息
	LOADING                        3                     接收到响应主体
	DONE                           4                     响应完成
	+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

--- xhr.overrideMimeType('text/plain;charset=x-user-defined');overrideMimeType方法用来指定服务器返回的数据的Mime类型；

--- xhr.responseType属性，它用来指定服务器返回的数据的类型，xhr2允许用户自行设置这个属性了(上面的overrideMimeType方法就是一般在不支持这个属性的情况下使用的)。

　　xhr.responseType='text'(字符串)/'arrayBuffer'/'blob'(二进制数据)/'document'/'json'(JSON对象)

---  事件：XMLHttpRequest对象原本只有一个事件xhr.onreadystatechange = function(){}，但是XMLHttpRequest2扩展了几个新的事件：

	++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
	事件                        描述
	------------------------------------------------------------------------------
	onloadstart                请求开始时触发
	onprogress                 开始加载并发送数据时触发
	onabort                    请求终止时出发，比如调用了abort()方法
	onerror                    请求失败时触发
	onload                     请求成功完成时触发
	ontimeout                  自定义的超时时间超过，请求还没完成时触发
	onloadend                  请求完成时触发（success or failure）
	++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

1.2 send()方法

请求中的send()方法，通常在发送POST请求时用于发送主体数据，发送GET请求时通常为空。

在XHR2中send()方法可以发送很多类型的数据。

	void send();
	void send(ArrayBuffer data);
	void send(Blob data); //用来发送二进制数据（这使得通过Ajax上传文件成为了可能）
	void send(Document data);
	void send(DOMString data);
	void send(FormData data); //用于构造表单数据

通过Ajax提交表单数据还是很有意义的。传统的做法中，点击页面中表单的submit按钮，需要跳转到一个空页面，然后再对原页面的数据进行处理。ajax则不用，可以直接在原页面进行提交后的处理。

发送FormData类型的数据为我们提供了便利：

	var formData = new FormData();
  
	formData.append('username','mengxue');//第一个参数为<input>标签的name属性
	formData.append('email','673013891@qq.com');//第二个参数为value属性
	formData.append('city','hangzhou');
 
	//现有表单中没有的数据也可以构造在请求中
	formData.append('birthday', '930920');
  
	xhr.send(formData);

1.3  再看CORS

CORS（跨域资源共享），它的出现就是为了解救Ajax受限于同源策略，实现跨域。 

预检机制：

我们可能只知道要实现跨域要请求头带上Origin头，并且服务器响应Access-Control-Request-Method头表示同意跨域的请求。但其实我们忽略了它背后的一个预检机制，也就是说会有类似这样一个预检请求：

	1 OPTIONS /resources/post-here/ HTTP/1.1
 	2 Host: www.google.com
 	3 User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) Gecko/20081130 Minefield/3.1b3pre
 	4 Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
 	5 Accept-Language: en-us,en;q=0.5
 	6 Accept-Encoding: gzip,deflate
 	7 Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
 	8 Connection: keep-alive
	9 Origin: http://www.example.com
	10 Access-Control-Request-Method: POST
	11 Access-Control-Request-Headers: X-PINGOTHER

OPTIONS就是告诉我们这是一个“预检”请求。里面有关键的Origin头信息。

	Access-Control-Request-Method: POST
	Access-Control-Request-Headers: X-PINGOTHER

这两个头信息就告诉服务器，如果我发送请求的话将会是POST请求，我还可能自己定义个X-PINGOTHER的请求头信息。

同样的，服务器也会响应这个预检的请求：

	HTTP/1.1 200 OK
	Date: Mon, 01 Dec 2008 01:15:39 GMT
	Server: Apache/2.0.61 (Unix)
	Access-Control-Allow-Origin: http://www.example.com
	Access-Control-Allow-Methods: POST, GET, OPTIONS
	Access-Control-Allow-Headers: X-PINGOTHER
	Access-Control-Max-Age: 1728000
	Vary: Accept-Encoding, Origin
	Content-Encoding: gzip
	Content-Length: 0
	Keep-Alive: timeout=2, max=100
	Connection: Keep-Alive
	Content-Type: text/plain

我们期待服务器同意我的跨域请求的Access-Control-Allow-Origin头也有了，告诉我服务器同意了。也同意了我想自定义一个请求头信息的愿望。

下面还有一个响应头信息：

	Access-Control-Max-Age: 1728000

它呢，是用来控制这条预检请求的缓存时长的，在这1728000秒之内，都不用再发送预检请求了。

因为整个预检的过程都是浏览器在后台神不知鬼不觉完成的，所以我们都不知道。


CORS中的Cookie：

CORS机制默认是不会发送cookie信息和HTTP的认证信息的，除非我们改变设置。可以这样：

	xhr.withCredentials = true;

同时服务器响应：

	Access-Control-Allow-Credentials:true //这里值不能为*必须明确指定域名

这时，我们在发送请求的时候才会捎带上cookie的信息，而且这里的cookie也遵循同源策略，只有服务器域名下的cookie信息才会被捎带。

##二、HTTP请求和响应

因为Ajax就是用来与服务器通信的，那么这里它就离不开HTTP，也就会去请求然后获得响应。这里稍微详细的了解下请求头与响应头相关的信息。

一个 HTTP请求由4部分组成：

　　-- HTTP请求方法或“动作”；

　　-- 正在请求的URL；

　　-- 一个可选的请求头集合，其中可能包括身份验证信息；

　　-- 一个可选的请求主体 ；

服务器返回的HTTP响应包含3部分：

　　-- 一个数字和文字组成的状态码，用来显示请求的成功和失败

　　-- 一个响应头集合

　　-- 响应主体

 

我在网上找了一个参数尽可能多的请求，下面是它的请求头信息和响应头信息：

![图2.1](/img/21.png) 

2.1 Request Headers中的一些参数：

1. Accept:text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8

含义：浏览器支持的 MIME 类型分别是 text/html、application/xhtml+xml、application/xml 和 */*，优先顺序是它们从左到右的排列顺序。

详解：

　　-- Accept表示浏览器支持的 MIME 类型；

　　-- MIME的英文全称是 Multipurpose Internet Mail Extensions（多功能 Internet 邮件扩充服务），它是一种多用途网际邮件扩充协议，在1992年最早应用于电子邮件系统，但后来也应用到浏览器;

　　-- text/html,application/xhtml+xml,application/xml 都是 MIME 类型，也可以称为媒体类型和内容类型，斜杠前面的是 type（类型），斜杠后面的是 subtype（子类型）,type 指定大的范围，subtype 是 type 中范围更明确的类型，即大类中的小类;

　　-- Text用于标准化地表示的文本信息，文本消息可以是多种字符集和或者多种格式的;

　　-- text/html表示 html 文档;

　　-- Application：用于传输应用程序数据或者二进制数据;

　　-- application/xhtml+xml表示 xhtml 文档;

　　-- application/xml表示 xml 文档;

　　-- q是权重系数，范围 0 =< q <= 1，q 值越大，请求越倾向于获得其“;”之前的类型表示的内容，若没有指定 q 值，则默认为1，若被赋值为0，则用于提醒服务器哪些是浏览器不接受的内容类型。

2.Accept-Encoding: gzip, deflate

含义：标示浏览器支持的压缩编码是gzip和deflate和sdcn。

3.Accept-Language:zh-CN,zh;q=0.8

含义：浏览器支持的语言，zh-CN是简体中文，zh是中文。

4.Cache-Control:max-age=0(请求头中的Cache-Control)

含义：Cache-Control头是用来在请求/响应这个过程中控制缓存的。

他可以有的参数:

　　max-age=0单位是秒，它表示客户端可接受的响应等待时间；

　　max-stale表示客户端愿意接受一个响应，即使它已经超过了它新鲜的寿命（可以有值也可以没有值）；

　　min-fresh=10他表示客户端愿意接受一个响应的保鲜寿命不小于目前的年龄加上指定的时间在几秒钟内；

　　no-cache表示不接受缓存的响应；

　　no-store表示缓存不可以保存请求和响应的任何部分；

　　only-if-cache:它表示客户端只想要获得一个被缓存起来了的响应。

5.Host

含义：客户端指定自己想访问的WEB服务器的域名/IP 地址和端口号.

6.Content-Type:application/x-www-form-urlencoded

含义：请求头的Content-Type定义传递到服务器的数据类型，数据由服务器端处理！Content-Type后面的值就是一个MIME类型。application/x-www-form-urlencoded实际上就是指表单提交的数据，如果上传附件，就会是multipart/form-data。响应头的Content- Type定义返回到客户端的数据类型。

其他一些常见易理解的参数，这里就不列举啦。

 

2.2 Response Headers中的一些参数：

1.Accept-Ranges:bytes

含义：WEB服务器表明自己是否接受获取其某个实体的一部分（比如文件的一部分）的请求。bytes：表示接受，none：表示不接受。

2.Cache-Control：Public

　　   public(可以用 Cached 内容回应任何用户)
         private（只能用缓存内容回应先前请求该内容的那个用户）
         no-cache（可以缓存，但是只有在跟WEB服务器验证了其有效后，才能返回给客户端）
         max-age：（本响应包含的对象的过期时间）
         ALL:  no-store（不允许缓存）

3.ETag

含义：它是一个对象的标志值，用于确认请求的文件是否被更改。比如一个 html 文件，如果被修改了，其 Etag 也会别修改，所以，ETag 的作用跟 Last-Modified 的作用差不多，主要供 WEB 服务器判断一个对象是否改变了。比如前一次请求某个 html 文件时，获得了其 ETag，当这次又请求这个文件时，浏览器就会把先前获得的 ETag 值发送给  WEB 服务器，然后 WEB 服务器会把这个 ETag 跟该文件的当前 ETag 进行对比，然后就知道这个文件有没有改变了。

4.Expires

含义：WEB服务器表明该实体将在什么时候过期，对于过期了的对象，只有在跟WEB服务器验证了其有效性后，才能用来响应客户请求

5.Last-Modified

含义：WEB 服务器认为对象的最后修改时间，比如文件的最后修改时间，动态页面的最后产生时间等等

[更为详细些的头部参数的说明](http://yolcy.blog.163.com/blog/static/105307937201022471913971/)

2.3 GET请求和POST请求的区别

也许你也曾想过GET和POST方法的区别，这里整理一下我了解到的：

GET：

	++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
	GET方法用于向指定的资源或服务器请求数据
	--------------------------------------------------------------------------------
	1.GET请求可以被浏览器缓存
	2.GET请求保留在浏览器的历史记录中
	3.GET请求可以被收藏为书签
	4.GET请求不应该在处理敏感数据时使用，它的安全性有待商榷
	5.GET请求有长度的限制
	6.GET请求应当只用于取回数据
	7.GET请求从来没有主体，需要的查询参数要编码放在url后面作为查询字符串
	++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

POST：

	++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
	POST方法向指定的资源或服务器提交要被处理的数据
	------------------------------------------------------------------------------------
	1.POST请求不会被缓存
	2.POST请求不会保留在浏览器的历史记录中
	3.POST请求不能被收藏为书签
	4.POST请求对数据的长度没有要求
	5.POST请求通常有主体，用来提交要被处理的数据，xhr.send('数据')
	++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

如果想更加详细的学习HTTP协议相关的内容可以查看RFC：[RFC7230](https://tools.ietf.org/html/rfc7230#section-4.3.3) [RFC7231](https://tools.ietf.org/html/rfc7231) [RFC7232](https://tools.ietf.org/html/rfc7231) 等等


##三.JQuery中的Ajax

我们一般实际应用的时候大部分都是jQuery或Zepto中的Ajax了。

我给自己列了些细节或要点，有时很有用，有时会避免错误：

1.$.ajax()会返回创建的XMLHttpRequest对象，然后我们可以就可以应用xhr对象的一些属性或方法了，但大多数情况下我们不会用到，像这样：

	htmlobj=$.ajax({url:"/jquery/test1.txt",async:false});
  	$("#myDiv").html(htmlobj.responseText);　

2.cache：false

　　有时候我们发送了个请求，返回的数据也得到了，但是后端同学在相同的接口中准备返回给你跟之前不同的数据比如说多一个字段，或少一个字段时。你肯定期待拿到新的数据了，但如果的你请求是get请求的话那么浏览器就会缓存你的Ajax请求，post请求一般不存在这个问题，于是你就纳闷了为什么数据没有变化。于是我们可以设置cache：false，使每次都发送请求而不是取缓存中的。

　　3.context:document.body

　　context参数可以指定回调函数的上下文，使回调函数中的$(this)指向context参数对应的DOM元素。

　　4.dataType：'xml'/'jsonp'/'html'/'script'/'json'/'text'

　　这是最简单务实的一个参数了，用于定于预期服务器返回的数据类型。设置值为‘script’时，将返回纯文本js代码，并且不会自动缓存结果，除非显示的设置cache为true，并且所有的POST请求都会被转为GET请求。设置值为‘jsonp’时，如果我们自己没有显示的定义回调函数的名字，jQuery会自动的帮我们添加，以正确的执行回调函数。这样我们回调函数的参数data就是json类型的数据了。

##四.待扩展和实战的学习要点

进一步准备学习和实战练习的：

　　利用XMLHttpRequest对象特别是扩展了的二级XMLHttpRequest对象，我们还可以做很多事情，比如说：①Ajax上传文件（可以使用FormData接口或File API）②获得progress元素的上传进度（xhr对象的upload属性的progress事件）③使用abort()方法终止请求和实现超时 等等。

　　HTTP用来在客户端和服务器之间建立连接进行交流，那么利用HTTP请求头信息和响应头信息其实也能做很多事情，比如：①正确的配置信息头实现请求的缓存，加快响应用户的速度②控制安全的请求同服务器交互等等。

　　Jquery源码中ajax的实现方式，思想学习以及与Zepto中Ajax实现的不同。


参考资料&相关阅读：

[Ajax-Javascript标准参考教程](http://javascript.ruanyifeng.com/bom/ajax.html)
[MDN UsingXMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest)
[使用FormData对象](https://developer.mozilla.org/zh-CN/docs/Web/Guide/Using_FormData_Objects)
[Web API接口查询表](https://developer.mozilla.org/zh-CN/docs/Web/API)
[Using CORS](http://www.html5rocks.com/en/tutorials/cors/)　
[http协议请求响应头中参数的疑问](http://www.zhihu.com/question/22073045)
[JQuery源码分析](http://www.cnblogs.com/aaronjs/p/3279314.html)
[Zeptojs Ajax API](http://www.css88.com/doc/zeptojs_api/#$.ajax)