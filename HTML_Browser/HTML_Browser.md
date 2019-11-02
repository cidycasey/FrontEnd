# HTML&浏览器

## 前端性能优化：细说浏览器渲染的重排（=回流）与重绘

https://www.imooc.com/article/45936

### **浏览器渲染HTML的步骤**

HTML渲染大致分为如下几步：

1. HTML被HTML解析器解析成DOM Tree, css则被css解析器解析成CSSOM Tree。
2. DOM Tree和CSSOM Tree解析完成后，被附加到一起，形成渲染树（Render Tree）。
3. 节点信息计算(重排)，这个过程被叫做Layout(Webkit)或者Reflow(Mozilla)。即根据渲染树计算每个节点的几何信息。
4. 渲染绘制(重绘)，这个过程被叫做(Painting 或者 Repaint)。即根据计算好的信息绘制整个页面。

### 总结会引起重排的操作有：

1. 页面首次渲染。
2. 浏览器窗口大小发生改变。
3. 元素尺寸或位置发生改变。
4. 元素内容变化（文字数量或图片大小等等）。
5. 元素字体大小变化。
6. 添加或者删除可见的DOM元素。
7. 激活CSS伪类（例如：:hover）。
8. 设置style属性
9. 查询某些属性或调用某些方法。

## TCP三次握手与四次挥手

 （1）序号：Seq序号，占32位，用来标识从TCP源端向目的端发送的字节流，发起方发送数据时对此进行标记。

 （2）确认序号：Ack序号，占32位，**只有ACK标志位为1时，确认序号字段才有效，Ack=Seq+1**。
 （3）标志位：共6个，即URG、ACK、PSH、RST、SYN、FIN等，具体含义如下：
                （A）URG：紧急指针（urgent pointer）有效。
                **（B）ACK：确认序号有效**。
                （C）PSH：接收方应该尽快将这个报文交给应用层。
                （D）RST：重置连接。
                **（E）SYN：发起一个新连接**。
                **（F）FIN：释放一个连接。**

​        需要注意的是：
​                （A）不要将确认序号Ack与标志位中的ACK搞混了。
​                （B）**确认方Ack=发起方Seq+1**，两端配对。   

![three](E:\MarkDown\FrontEnd\HTML_Browser\images\three.png)

![four](E:\MarkDown\FrontEnd\HTML_Browser\images\four.png)

​	为什么建立连接是三次握手，而关闭连接却是四次挥手呢？
​    这是因为服务端在LISTEN状态下，收到建立连接请求的SYN报文后，把ACK和SYN放在一个报文里发送给客户端。

​	而关闭连接时，当收到对方的FIN报文时，仅仅表示对方不再发送数据了但是还能接收数据，己方也未必全部数据都发送给对方了，所以己方可以立即close，也可以发送一些数据给对方后，再发送FIN报文给对方来表示同意现在关闭连接，因此，己方ACK和FIN一般都会分开发送

## TCP和UDP区别

TCP：**面向连接，提供可靠的服务，**有流量控制，拥塞控制，无重复、无丢失、无差错，**面向字节流**(把应用层传下来的报文看成字节流，把字节流组织成大小不等的数据块)，**只能是点对点，首部 20 字节**，全双工（打电话）。

UDP：**无连接，尽最大努力交付**，没有拥塞控制，**面向报文**（IP数据报）(对于应用程序传下来的报文不合并也不拆分，只是添加 UDP 首部)，**支持一对一、一对多、多对多，首部 8 字节**。

UDP应用：**多媒体数据流，**不产生任何额外的数据，即使知道有破坏的包也不进行重发。当强调**传输性能而不是传输的完整性时，**如：**音频和多媒体应用**，UDP是最好的选择。在数据传输时间很短，以至于此前的连接过程成为整个流量主体的情况下，UDP也是一个好的选择。



## 网络的五层划分是什么？

https://blog.csdn.net/SYP35/article/details/80746020

![ipPackage](E:\MarkDown\FrontEnd\HTML_Browser\images\ipPackage.png)

1、应用层

应用层是体系结构中的最高层。应用层确定进程之间通信的性质以满足用户的需要。这里的进程就是指正在运行的程序。

2、传输层（端对端的）

传输层的任务就是负责主机中两个进程之间的通信。因特网的传输层可使用两种不同协议：即面向连接的[传输控制协议](https://www.baidu.com/s?wd=传输控制协议&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)TCP(Transmission Control Protocol )（三次握手。四次挥手），和无连接的[用户数据报协议](https://www.baidu.com/s?wd=用户数据报协议&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)UDP（User Datagram Protocol）。

3、网络层（路由）

网络层负责为分组交换网上的不同主机提供通信。在发送数据时，网络层将运输层产生的报文段或用户数据报封装成分组或包进行传送。

4、数据链路层

当发送数据时，数据链路层的任务是将在**网络层**交下来的[IP数据报](https://www.baidu.com/s?wd=IP数据报&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)组装成帧，在两个相邻结点间的链路上传送以帧为单位的数据。每一帧包括数据和必要的控制信息（如同步信息、地址信息、差错控制、以及[流量控制](https://www.baidu.com/s?wd=流量控制&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)信息等）。

5、物理层

物理层的任务就是透明地传送比特流。在物理层上所传数据的单位是比特。传递信息所利用的一些物理媒体，如双

绞线、同轴电缆、光缆等，并不在物理层之内而是在物理层的下面。

**Socket是传输控制层协议，WebSocket是应用层协议。**

## get 和 post 的区别

后退/刷新；书签bookmark、缓存、历史；编码类型；数据长度；数据类型；安全性；

![get-post](E:\MarkDown\FrontEnd\HTML_Browser\images\get-post.png)

-  **GET和POST长度的限制问题**

​    **GET：**

​    1、GET是通过URL提交数据，因此GET可提交的数据量就跟URL所能达到的最大长度有直接关系

​    2、实际上HTTP协议对URL长度是没有限制的；限制URL长度大多数是**浏览器或者服务器的配置参数**

​    **POST：**

​    1、同样的，HTTP协议没有对POST进行任何限制，一般是受服务器配置限制或者内存大小

- **GET和POST的安全性**

​    1、GET是通过URL方式请求，可以直接看到，明文传输

​    2、POST是通过请求header请求，可以开发者工具或者抓包可以看到，同样也是明文的

​    3、GET请求会保存在浏览器历史纪录中，还可能会保存在Web的日志中

​	4、GET参数通过URL传递，POST放在**Request body**中。

- **GET幂等/POST不幂等**

​    幂等是指同一个请求方法执行多次和仅执行一次的效果完全相同。 

- **POST 方法会产生两个 TCP 数据包？**

  有些文章中提到，post 会将 header 和 body 分开发送，先发送 header，服务端返回 100 状态码再发送 body。

  HTTP 协议中没有明确说明 POST 会产生两个 TCP 数据包，而且实际测试(Chrome)发现，header 和 body 不会分开发送。

  所以，header 和 body 分开发送是**部分浏览器或框架的请求方法，不属于 post 必然行为。**

面试必备：GET和POST的区别详细解说：

https://baijiahao.baidu.com/s?id=1626599028653203490&wfr=spider&for=pc

## HTTP协议以及HTTP请求中8种请求方法

什么是协议？

　　协议，是指通信的双方，在通信流程或内容格式上，共同遵守的标准。

什么是http协议？

　　http协议，是互联网中最常见的网络通信标准。

http协议的特点

　　①通信流程：断开式（无状态）

　　　　　　　　断开式：http协议每次响应完成后，会断开与客户端的连接

　　　　　　　　无状态：由于服务器断开了之前的连接，就无法知晓连接间的关系

　　②内容格式：消息头和消息体

**HTTP请求的方法：**

HTTP/1.1协议中共定义了八种方法（有时也叫“动作”），**来表明Request-URL指定的资源不同的操作方式**
1、OPTIONS
返回服务器针对特定资源所**支持的HTTP请求方法**，也可以利用向web服务器发送‘*’的请求来测试服务器的功能性
2、HEAD
向服务器索与GET请求相一致的响应，只不过**响应体将不会被返回**。这一方法可以再不必传输整个响应内容的情况下，就可以获取包含在响应消息头中的元数据。
3、GET
向特定的资源发出请求。它本质就是发送一个请求来取得服务器上的某一资源。资源通过一组HTTP头和呈现数据（如HTML文本，或者图片或者视频等）返回给客户端。GET请求中，永远不会包含呈现数据。
4、POST
向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在**请求体**中。**POST请求可能会导致新的资源的建立和/或已有资源的修改。**
5、PUT
向指定资源位置上传其最新内容
6、DELETE
请求服务器删除Request-URL所标识的资源
7、TRACE
**回显**服务器收到的请求，主要用于测试或诊断
8、CONNECT
HTTP/1.1协议中预留给能够将**连接改为管道方式**的代理服务器。
**注意：**
	1）方法名称是区分大小写的，

​	当某个请求所针对的资源不支持对应的请求方法的时候，服务器应当返回状态码405（Method Not Allowed）；

​	当服务器不认识或者不支持对应的请求方法时，应返回状态码501（Not Implemented）。
​	2）HTTP服务器至少应该实现GET和HEAD/POST方法，其他方法都是可选的，此外除上述方法，特定的HTTP服务器支持扩展自定义的方法。

## cookie和session的区别

[session和cookie的区别](https://www.cnblogs.com/8023-CHD/p/11067141.html)

**前言**
		HTTP是一种无状态的协议，为了分辨链接是谁发起的，需自己去解决这个问题。不然有些情况下即使是同一个网站每打开一个页面也都要登录一下。而Session和Cookie就是为解决这个问题而提出来的两个机制。
**应用场景**

- 登录网站，今输入用户名密码登录了，第二天再打开很多情况下就直接打开了。这个时候用到的一个机制就是cookie。
- session一个场景是购物车，添加了商品之后客户端处可以知道添加了哪些商品，而服务器端如何判别呢，所以也需要存储一些信息就用到了session。

1、cookie数据存放在客户的浏览器上，session数据存放在服务器上；

2、cookie不是很安全，别人可以分析存放在本地的cookie并进行cookie欺骗；考虑到安全应当使用session；

3、session会在一定时间内保存在服务器上，当访问量增加时，会比较占用服务器的性能；考虑到减轻服务器性能方面，应当使用cookie；

4、单个cookie保存的数据不能超过**4K，**很多浏览器都限制一个站点最多保存20个cookie；

5、将登录信息等重要信息存放在cookie，其他信息如果需要保留，可以放在cookie中；

**理解Cookie和Session的区别及使用**

**1.Cookie**

- 通俗讲，是访问某些网站后在本地存储的一些网站相关信息，下次访问时减少一些步骤。更准确的说法是：Cookies是服务器在本地机器上存储的小段文本并随每一个请求发送至同一服务器，**是在客户端保持状态的方案**。
- Cookie的主要内容包括：名字，值，过期时间，路径(path)和域。使用Fiddler抓包就可以看见，比方说我们打开百度的某个网站可以看到Headers包括Cookie，如下： 

```c
BIDUPSID: 9D2194F1CB8D1E56272947F6B0E5D47E 
PSTM: 1472480791 
BAIDUID: 3C64D3C3F1753134D13C33AFD2B38367:FG 
ispeed_lsm: 2 
MCITY: -131: 
pgv_pvi: 3797581824 
pgv_si: s9468756992 
BDUSS: JhNXVoQmhPYTVENEdIUnQ5S05xcHZMMVY5QzFRNVh5SzZoV0xMVDR6RzV-bEJZSVFBQUFBJCQAAAAAAAAAAAEAAACteXsbYnRfY2hpbGQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAALlxKVi5cSlYZj 
BD_HOME: 1 
H_PS_PSSID: 1423_21080_17001_21454_21408_21530_21377_21525_21193_21340 
BD_UPN: 123253 
sug: 3 
sugstore: 0 
ORIGIN: 0 
bdime: 0
```

- key, value形式。
- 过期时间可设置的，
  - 如不设，则浏览器关掉就消失了。
  - 存储在内存当中，否则就按设置的时间来存储在硬盘上的，过期后自动清除，比方说开关机关闭再打开浏览器后他都会还存在，
  - 前者称之为Session cookie 又叫 transient cookie，后者称之为Persistent cookie 又叫 permenent cookie。
- 路径和域就是对应的域名，a网站的cookie自然不能给b用。

**2.Session**
存在服务器的一种用来存放用户数据的类HashTable结构。

- 浏览器第一次发送请求时，服务器自动生成了一HashTable和一Session ID来唯一标识这个HashTable，并将其通过响应发送到浏览器。

  浏览器第二次发送请求会将前一次服务器响应中的Session ID放在请求中一并发送到服务器上，服务器从请求中提取出Session ID，并和保存的所有Session ID进行对比，找到这个用户对应的HashTable。 
  一般这个值会有个时间限制，超时后毁掉这个值，默认30分钟。

- 当用户在应用程序的 Web页间跳转时，**存储在 Session 对象中的变量不会丢失而是在整个用户会话（打开浏览器到关闭浏览器）中一直存在下去。**

- Session的实现方式和Cookie有一定关系。**建立一个连接就生成一个session id，打开几个页面就好几个了，这里就用到了Cookie，把session id存在Cookie中，每次访问的时候将Session id带过去就可以识别了.**

  **注：**

## WebStorage（sessionStorage  localStorage）和cookie之间的区别

共同点：都是保存在浏览器端，且同源的

区别：

1、**cookie数据自始至终在 同源 的http请求中携带，**即cookie在浏览器和服务器间来回传递，而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。**cookie数据还有路径的概念，可以限制cookie只属于某个路径下。**

2、**存储大小限制也不同，**cookie数据不能超过4K，同时每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识，sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或者更大。

3、**数据有效期不同，**sessionStorage仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持。localStorage始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭

4、作用域不同，sessionStorage不在不同的 浏览器窗口 中共享，即使是同一个页面；localStorage在所有同源窗口中都是共享的，cookie也是在所有同源窗口中都是共享的

5、WEB Storage支持事件通知机制，可以将数据更新的通知发送给监听者。HTML5 Web Storage事件https://blog.csdn.net/ixygj197875/article/details/80114096

6、WEB Storage的api接口使用更方便，cookie需要自己封装。

```js
 保存数据：localStorage.setItem( key, value );      sessionStorage.setItem( key, value );
 读取数据：localStorage.getItem( key );     sessionStorage.getItem( key );  
 删除单个数据：localStorage.removeItem( key );     sessionStorage.removeItem( key );
 删除所有数据：localStorage.clear( );     sessionStorage.clear( );
 得到某个索引的key：localStorage.key( index );     sessionStorage.key( index );
```

## H5新特性

​		在H5中，article元素可以看成是一种特殊类型的section元素，它比section元素更强调独立性。即section元素强调分段或分块，而article强调独立性。具体来说，如果一块内容相对来说比较独立的、完整的时候，应该使用article元素，但是如果你想将一块内容分成几段的时候，应该使用section元素。
[HTML5的十大新特性](https://www.cnblogs.com/vicky1018/p/7705223.html)

## 介绍一下你对浏览器内核的理解？

主要分成两部分：渲染引擎(layout engineer或Rendering Engine)和JS引擎。

渲染引擎：负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏**览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。**所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。

JS引擎：解析和执行javascript来实现网页的动态效果。

最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，**内核就倾向于只指渲染引擎**。

常见内核

- Trident内核：**IE,**MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML]
- Gecko内核：Netscape6及以上版本，FF（**Firefox**）,MozillaSuite/SeaMonkey等
- Presto内核：Opera7及以上。      [**Opera内核原为：Presto，现为：Blink;**]
- Webkit内核：**Safari, Chrome**等。   [ Chrome的：**Blink（WebKit的分支）**]

## 前端路由

利用Hash实现前端路由：

​          设计思路：当浏览器地址栏url的hash值发生变化时，就会触发onhashchange事件，这时通过window.location.hash可以拿到当前浏览器的url的hash值，注意此时的hash值是带有#的，因此需要对其值进行相应的处理，去掉#，并且如果当前url不含hash值的话，就将其当做根目录处理。之后将url和相应的组件函数进行映射，根据不同的hash值执行不同的回调函数，也就是加载相应的组件。

利用history实现前端路由：        

​    设计思路：当想要跳转到指定url的时候，将目标url通过pushState()或者replaceState()方法填入到history和地址栏中，此时由于上述两种方法不会主动进行页面刷新，因此页面仍停留在当前页面，只是url地址发生了改变。之后通过popstate事件响应，执行相应的回调函数，实现前端组件间的切换。

## TCP拥塞控制

慢开始     拥塞避免       快重传        快恢复  

## **http 和 https 有何区别？如何灵活使用？**

- http是HTTP协议运行在TCP之上。所有传输的内容都是明文，客户端和服务器端都无法验证对方的身份。
- https是HTTP运行在SSL/TLS之上，SSL/TLS运行在TCP之上。所有传输的内容都经过加密，加密采用对称加密，但对称加密的密钥用服务器方的证书进行了非对称加密。此外客户端可以验证服务器端的身份，如果配置了客户端验证，服务器方也可以验证客户端的身份

## 浏览器是如何渲染页面的？

渲染的流程如下：

1.解析HTML文件，创建DOM树。

   自上而下，遇到任何样式（link、style）与脚本（script）都会阻塞（外部样式不阻塞后续外部脚本的加载）。

2.解析CSS。优先级：浏览器默认设置<用户设置<外部样式<内联样式<HTML中的style样式；

3.将CSS与DOM合并，构建渲染树（Render Tree）

4.布局和绘制，重绘（repaint）和重排（reflow）

## 请解释JSONP的工作原理，以及它为什么不是真正的AJAX。

JSONP (JSON with Padding)是一个简单高效的跨域方式，HTML中的script标签可以加载并执行其他域的javascript，于是我们可以通过script标记来动态加载其他域的资源。例如我要从域A的页面pageA加载域B的数据，那么在域B的页面pageB中我以JavaScript的形式声明pageA需要的数据，然后在 pageA中用script标签把pageB加载进来，那么pageB中的脚本就会得以执行。JSONP在此基础上加入了回调函数，pageB加载完之后会执行pageA中定义的函数，所需要的数据会以参数的形式传递给该函数。JSONP易于实现，但是也会存在一些安全隐患，如果第三方的脚本随意地执行，那么它就可以篡改页面内容，截获敏感数据。但是在受信任的双方传递数据，JSONP是非常合适的选择。

AJAX是不跨域的，而JSONP是一个是跨域的，还有就是二者接收参数形式不一样！

## HTTP常见相应状态码

<img src="C:\Users\CIDY CASEY\AppData\Roaming\Typora\typora-user-images\1569583895852.png" alt="1569583895852" style="zoom:67%;" />

## HTTP协议原理

https://blog.csdn.net/weixin_41964994/article/details/81784381

![img](https://img-blog.csdn.net/20170908102023076?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM5OTg5MDAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/)

## 浏览器从发起请求到页面显示，中间发生了什么？

1：浏览器根据**请求的URL**交给**DNS**进行域名解析，找到真实的**IP地址**，通过创建**TCP链接**，向服务器**发送请求**

2：服务器交给后台处理后返回数据，浏览器接收资源文件（html css Js 图像等）

3：浏览器对加载到的资源（html css Js 图像等）进行**语法解析**，建立相应的**内部数据结构**（如HTML的DOM结构）

4：载入解析到的资源文件，页面**渲染**，完成

## 解决跨域问题

理解跨域的概念：协议、域名、端口都相同才同域，否则都是跨域；

出于安全考虑，服务器不允许ajax跨域获取数据，但是可以跨域获取文件内容。

所以基于这一点，可以动态创建script标签，使用标签的src属性访问js文件的形式获取js脚本，并且这个js脚本中的内容是函数调用，该函数调用的参数是服务器返回的数据，为了获取这里的参数数据，需要事先在页面中定义回调函数，在回调函数中处理服务器返回的数据，这就是解决跨域问题的主流解决方案。

## 标签分类

行内元素：a、img、input、span、label

块级元素：div、h1、p、fieldset、table、hr、dl(dt dd)、ul(li）

空元素： ( 在 HTML[1] 元素中，没有内容的 HTML 元素被称为空元素 )   br、hr、img、input、link、meta

## 常见前端缓存

**cookie**、**localStorage**本地存储(在HTML5中，新加入了一个localStorage特性，这个特性主要是用来作为本地存储来使用的，解决了cookie存储空间不足的问题(cookie中每条cookie的存储空间为4k)，localStorage中一般浏览器支持的是5M大小，这个在不同的浏览器中localStorage会有所不同。)、**sessionStorage**会话存储(sessionStorage用于本地存储一个会话（session）中的数据，这些数据只有在同一个会话中的页面才能访问并且当会话结束后数据也随之销毁。)、**ApplicationCache**(h5 离线应用，离线浏览: 用户可以在离线状态下浏览网站内容。)

## HTML5的新特性：

新的内容标签：header nav content footer article aside

更好的单元格体系:

音频、视频API:video radio

画布(Canvas) API

地理(Geolocation) API

网页存储(Web storage) API:localStorage,sessionStorage

拖拽释放(Drag and drop) API

