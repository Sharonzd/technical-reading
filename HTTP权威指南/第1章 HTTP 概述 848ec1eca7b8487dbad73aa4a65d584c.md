# 第1章 HTTP 概述

前 3 章的电子书可以在图灵社区阅读：[https://www.ituring.com.cn/book/miniarticle/44574](https://www.ituring.com.cn/book/miniarticle/44574)

## 阅读前的提问

- Web 客户端与服务器是如何通信的；
- （表示 Web 内容的）资源来自何方；
- Web 事务是怎样工作的；
- HTTP 通信所使用的报文格式；
- 底层 TCP 网络传输；
- 不同的 HTTP 协议变体；
- 因特网上安装的大量 HTTP 架构组件中的一部分。

# HTTP 历史

HTTP 的起源需要追溯到蒂姆伯纳斯李在1989年发明万维网的时候，他将超文本技术结合到网络，并发明了三项关键技术：URI、HTML、HTTP。HTML 作为信息资源，URI 作为资源的地址定位符，而 HTTP 则作为网络的信使，负责将 HTML 从网络传送到客户端的浏览器。这三项技术，奠定了 Web 的基础。

最早的 HTTP 0.9版本发布于1991年，非常简单，只有由 GET +目标资源路径组成的单行指令，响应也只包含HTML文档本身。
到了1996年，发布了HTTP/1.0，增加了协议号、HTTP 头，额外的方法，还支持了其他媒体类型的资源。它是第一个得到了广泛使用的 HTTP 版本。

（但同时服务器和客户端都自行添加特性，比如持久连接，代理连接等，这种非正式的扩展版本被称为了 hTTp 1.0+）

没过多久，HTTP1.1就发布了，删除了一些不好的特性，增加了一些更复杂场景的支持。比如复用连接、管线技术、缓存机制、内容协商机制等。

- 连接可以复用，节省了多次打开TCP连接加载网页文档资源的时间。
- 增加管线化技术，允许在第一个应答被完全发送之前就发送第二个请求，以降低通信延迟。
- 支持响应分块。
- 引入额外的缓存控制机制。
- 引入内容协商机制，包括语言，编码，类型等，并允许客户端和服务器之间约定以最合适的内容进行交换。
- 感谢[Host](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Host)头，能够使不同域名配置在同一个IP地址的服务器上。

经过十多年的发展，在2015年发布了 HTTP2，重点关注性能优化，不同于以往的文本协议，HTTP2是二进制格式，而且作为复用协议，解决了HTTP/1 .x 中顺序和阻塞 的问题，此外HTTP 头能被压缩， 资源能通过服务端推送提前到达客户端。这些特性都大幅优化了 HTTP 的性能。

- 新的二进制格式（Binary Format），HTTP1.x的解析是基于文本。基于文本协议的格式解析存在天然缺陷，文本的表现形式有多样性，要做到健壮性考虑的场景必然很多，二进制则不同，只认0和1的组合。基于这种考虑HTTP2.0的协议解析决定采用二进制格式，实现方便且健壮。
- 多路复用（MultiPlexing），即连接共享，即每一个request都是是用作连接共享机制的。一个request对应一个id，这样一个连接上可以有多个request，每个连接的request可以随机的混杂在一起，接收方可以根据request的 id将request再归属到各自不同的服务端请求里面。
- header压缩，如上文中所言，对前面提到过HTTP1.x的header带有大量信息，而且每次都要重复发送，HTTP2.0使用encoder来减少需要传输的header大小，通讯双方各自cache一份header fields表，既避免了重复header的传输，又减小了需要传输的大小。
- 服务端推送（server push），同SPDY一样，HTTP2.0也具有server push功能。

## 阅读思考的参考答案

1. Web 客户端和服务器的通信方式：在浏览页面时，浏览器向服务器发送一条 HTTP 请求，服务器会去寻找所期望的对象，如果成功，就将对象、对象类型、对象长度以及其他信息放在 HTTP 响应中发给客户端。
2. 所有能够提供 Web 内容的东西都是 **Web 资源**。包括文件、Web 网关、搜索引擎。资源通过 MIME 标识数据类型，通过 URI 来标识名字，其中 URI 又分为 URL 和 URN 两种格式。
3. 一个 HTTP 事务由一条（从客户端发往服务器的）请求命令和一个（从服务器发回客户端的）响应结果组成。这种通信是通过名为 HTTP 报文（HTTP message）的格式化数据块进行的。
4. HTTP 报文是纯文本，包括起始行、首部字段、主体三部分。
5. HTTP 通过 TCP/IP 进行通信。具体步骤是，
    1. 用户输入 URL
    2. 获取主机名
    3. DNS 解析
    4. 获取端口号
    5. 连接到 IP 的对应端口
    6. 发送一条 HTTP GET 请求
    7. 从服务器读取 HTTP 响应
    8. 关闭连接
    9. 浏览器显示页面
6. HTTP 协议版本，参考 [https://www.w3.org/Protocols/](https://www.w3.org/Protocols/)
7. Web 应用程序，除了客户端与服务器以外，还有代理、缓存、网管、隧道、Agent 代理。

### 

参考：[https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP)

协议历史

1. HTTP/0.9 1991
2. HTTP/1.0 1996：[https://tools.ietf.org/html/rfc1945](https://tools.ietf.org/html/rfc1945)
3. HTTP/1.1 [https://tools.ietf.org/html/rfc2616](https://tools.ietf.org/html/rfc2616) 1997 rfc2086-1999
4. HTTP/2.0 [https://tools.ietf.org/html/rfc7540](https://tools.ietf.org/html/rfc7540) 2015

参考：[https://mp.weixin.qq.com/s/GICbiyJpINrHZ41u_4zT-A?](https://mp.weixin.qq.com/s/GICbiyJpINrHZ41u_4zT-A?)

比较 wiki：[https://zh.wikipedia.org/wiki/HTTP/2](https://zh.wikipedia.org/wiki/HTTP/2)

阮一峰入门：[https://www.ruanyifeng.com/blog/2016/08/http.html](https://www.ruanyifeng.com/blog/2016/08/http.html)