# 第3章 HTTP 报文

## 概念

HTTP 报文是网络中的数据块，服务器、客户端和代理都既扮演着报文的发送者，也扮演着接收者。发送报文的称为上游，接受报文的称为下游，报文始终是向下游流动的。

报文分为请求报文和响应报文。每条报文包含三部分，分别是起始行，首部块，以及主体。起始行和首部都是 ASCII 文本（ASCII 仅包含26个基本拉丁字母、阿拉伯数字和英式标点符号），每行都以回车符和换行符结尾，也称作 CRLF（CR 是 carriage return 回车符，LF 是 line feed换行符）。而且首部需要以一个CRLF空行结束。但是由于历史包袱，关于 CRLF 的规则并不严格。而主体可以包含文本或二进制数据，也可以是空的，在首部中会说明主体相关的信息。

请求报文的起始行说明了要做什么，响应报文的起始行说明了发生了什么。这主要分别对应着明天要介绍的请求的方法 和 响应的状态码。

### **报文流动方式**

HTTP 报文是网络中的数据块，发送报文的称为上游，接受报文的称为下游，报文始终是向下游流动的。

## **报文的组成部分**

报文分为两类，请求报文和响应报文。每条报文包含三部分，分别是起始行，首部块，以及主体。起始行和首部都是 ASCII 文本（26个基本拉丁字母、阿拉伯数字和英式标点符号），每行都以回车符和换行符结尾，也称作 CRLF，CR 是 carriage return 回车符，LF 是 line feed换行符。而且首部需要以一个空行，也就是一个 CRLF 结束。但是因为历史包袱，所以关于 CRLF 的规则并不严格。而主体可以包含文本或二进制数据，也可以是空的。

## **请求报文和响应报文的区别**

请求报文的起始行说明了要做什么，响应报文的起始行说明了发生了什么。

## **请求报文支持的功能**

在 HTTP 请求的方法中，包含GET、HEAD、POST、PUT、TRACE、OPTIONS、DELETE，其中 GET 和 HEAD 为安全方法，POST 和 PUT 会包含主体，而PUT 和 DELETE 都因为要操作服务器所以不安全，会受到较大的限制。GET 和 POST 是安全的，OPTIONS 用于请求服务器所支持的方法，用来判定获取资源的最优方式。

[参见 MDN options](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS)

`存疑：POST 什么情况下是 cacheable的？`

## **响应报文的状态码**

1xx 是信息性的状态码

2xx 是成功状态码

3xx 是重定向的状态码

### 小知识点：302 303 307的区别

302是 HTTP1.0中的状态码，那个时候它还被称作 Moved Temporarily。当HTTP1.0客户端发起一个 POST 请求，并且收到302重定向状态码的响应时，它可能会向响应中的指定的重定向 URL发起一个 GET 请求，而不是原来的 POST 请求。规范中明确了这是一个错误的行为。这个错误
但是这样的错误实在是太多了，因此 HTTP1.1把1.0中 302 的行为拆成了303和307两个状态码，其中303就是明确的会将POST 请求转为 GET 请求，而307则是明确不对方法做任何改变，原样的重新发起一个指向重定向URL请求。

更多详见：

[HTTP请求返回代码](%E7%AC%AC3%E7%AB%A0%20HTTP%20%E6%8A%A5%E6%96%87%200154aa2843ac4a2cbd39941aca2a0af1/HTTP%E8%AF%B7%E6%B1%82%E8%BF%94%E5%9B%9E%E4%BB%A3%E7%A0%81%2048f0c3aa1e34448f9d7a3b5deb93e1fb.md)

## **HTTP 首部作用**

首部分为五种，通用首部、请求首部、响应首部、实体首部和扩展首部。

- 通用首部：包含时间信息、连接方式、缓存指示等信息
- 请求首部：包含
    - 如 ua、host、referer 的信息性的首部。
    - 用于告诉服务器，客户端所支持的媒体类型、字符集、编码方式、语言的 Accept 首部。
    - 还有用于加上请求限制的的条件请求首部，比如 If-Modified-Since，会和实体首部的 last-modified，而If-None-Match一般会和实体首部的ETag一起用于一起用于协商缓存。
    - 还有安全请求首部，比如 Cookie；还有用于于代理交互的代理请求首部。
- 响应首部：包含信息首部、协商首部，安全响应首部
- 实体首部：包含 Content-type, Etag 等。

## HTTP 方法

在 HTTP 的 RFC7231规范中约束了，所有的 HTTP 方法只有 **GET** 和 **HEAD** 是服务器必须实现的，其余都是可选的。

并且HTTP 规范定义了一些方法的通用的属性，比如安全的，幂等的，可缓存的

**安全方法**：是指该方法的请求不会在服务器上产生结果，相当于只读操作，比如 GET 、HEAD、OPTIONS和 TRACE方法。（参见[https://tools.ietf.org/html/rfc7231#section-4.2.1](https://tools.ietf.org/html/rfc7231#section-4.2.1)）

**幂等方法：**是指同样的请求被执行一次与连续执行多次的效果一样，PUT、DELETE和安全方法都是幂等的（参见：[https://tools.ietf.org/html/rfc7231#section-4.2.2](https://tools.ietf.org/html/rfc7231#section-4.2.2)）

可缓存方法：指的是该请求的响应能被存储起来重用，GET、HEAD 和某些情况下的 POST 可以被缓存（参见：[https://tools.ietf.org/html/rfc7231#section-4.2.3](https://tools.ietf.org/html/rfc7231#section-4.2.3) + [https://tools.ietf.org/html/rfc7234](https://tools.ietf.org/html/rfc7234)）

| HTTP方法 | 解释                                         | 安全方法 | 幂等方法 | 可缓存的方法                                  | 请求有主体 | 响应有主体  | 是否支持HTML表单 |
|:---------|:---------------------------------------------|:--------|:--------|:--------------------------------------------|:----------|:----------|:----------------|
| GET      |                                              | √       | √       | √                                           | x         | √         |                 |
| HEAD     | 请求资源的头部信息（可用于节约带宽）             | √       | √       | √                                           | x         | x         | x               |
| OPTIONS  | 用于描述目标资源的通信选项                      | √       | √       | x                                           | x         | x         | x               |
| PUT      | 使用请求中的负载创建或者替换目标资源             | x       | √       | x                                           | √         | x         | x               |
| DELETE   | 删除指定资源                                  | x       | √       | x                                           | x         | √         |                 |
| POST     |                                              | x       | x       | √ Only if freshness information is included | √         | √         |                 |
| CONNECT  | 建立一个到由目标资源标识的服务器的隧道           | x       | x       | x                                           | x         | √         | x               |
| TRACE    | 沿着到目标资源的路径执行一个消息环回测试（debug） | √       | √       | x                                           | x         | √         | x               |
| PATCH    | 对资源应用部分修改                             | x       | x       | x                                           | √         | x         | x               |


## HTTP header

### HTTP header 之 referer
referer 是用于提供当前请求的来源信息的字段，规范中承认了，由于拼写错误，少写了一个字母 r。

但除了 header 的 referer 是错误拼写，其他的大部分场景比如用于获取
referer 信息的 document.referrer 字段是正确的拼写，用于配置 referer
策略的名称也是正确拼写。所以我们在开发中需要注意这个拼写的坑。

对于网页上的链接、表单、或者静态资源请求，浏览器都会默认发送
referer 信息，但是有些时候我们需要改变 referer 或者屏蔽 referer
以应对某些特殊场景，比如不想暴露出内网地址，比如不想暴露来源页面的参数信息等等。

因此规范制定了 referer 策略，使得我们可以通过 meta 标签、或者 http 的 header Referrer-Policy 字段，或者链接标签的referrerpolicy属性，来配置 referr 的策略。有8中 referer 策略，其中**no-referrer-when-downgrade就是浏览器的默认行为，从字面意义可以看出来就是在请求降级的时候不要发送 referer，也就是说，**从 HTTPS 网址链接到 HTTP 网址，不发送Referer字段。

通过查看 referer 我看到了几个出乎我意料的现象：谷歌通过 meta 设置了 referer 为
origin，这样从谷歌跳转的页面获取的 referer 信息就只包含 [www.google.com](http://www.google.com/) 。但是从百度的搜索引擎跳转到页面，能拿到完整的带有 query 信息的 url，看了百度的页面，是设置了
referer 为 always，always 是一个已经被废弃的属性值，代表了无论什么情况都会携带完整的 referer 信息，即便是跳转到不安全的 http 协议的网站也会带上。这样给百度1是会带来安全风险，2是由于搜索关键词被获取到了可能会导致站点分析关键词，可能会对 seo 作弊。

<!--  todo 疏漏讨论-->

由请求的发送方决定是否带上 referer 字段，用户在地址栏输入网址，或者选中浏览器书签，就不发送Referer字段。

主要是以下三种场景，会发送Referer字段。

（1）用户点击网页上的链接。

（2）用户发送表单。

（3）网页加载静态资源，比如加载图片、脚本、样式。

上面这些场景，浏览器都会将当前网址作为Referer字段，放在 HTTP 请求的头信息发送。

浏览器的 JavaScript 引擎提供document.referrer属性，可以查看当前页面的引荐来源。注意，这里采用的是正确拼写。

很多时候需要改变
referer 或者屏蔽 referer，比如内网、比如google

Rel="noreferrer"屏蔽 referer

**（1）no-referrer**

不发送Referer字段。

**（2）no-referrer-when-downgrade**

如果从 HTTPS 网址链接到 HTTP 网址，不发送Referer字段，其他情况发送（包括 HTTP 网址链接到 HTTP 网址）。这是浏览器的默认行为。

**（3）same-origin**

链接到同源网址（协议+域名+端口
都相同）时发送，否则不发送。注意，`https://foo.com` 链接到 `http://foo.com` 也属于跨域。

**（4）origin**

Referer字段一律只发送源信息（协议+域名+端口），不管是否跨域。

**（5）strict-origin**

如果从 HTTPS 网址链接到 HTTP 网址，不发送Referer字段，其他情况只发送源信息。

**（6）origin-when-cross-origin**

同源时，发送完整的Referer字段，跨域时发送源信息。

**（7）strict-origin-when-cross-origin**

同源时，发送完整的Referer字段；跨域时，如果 HTTPS 网址链接到 HTTP 网址，不发送Referer字段，否则发送源信息。

**（8）unsafe-url**

Referer字段包含源信息、路径和查询字符串，不包含锚点、用户名和密码。

设置处：

**（1）HTTP 头信息**

服务器发送网页的时候，通过 HTTP 头信息的Referrer-Policy告诉浏览器。

`Referrer-Policy: origin`

**（2）**<meta>**标签**

也可以使用<meta>标签，在网页头部设置。

```
<meta name="referrer" content="origin">
```

**（3）**referrerpolicy**属性**

`<a>`、`<area>`、`<img>`、`<iframe>`和`<link>`标签，可以设置referrerpolicy 属性。

```
<a href="..." referrerpolicy="origin" target="_blank">xxx</a>
```


### 参考资料：

wikipedia：[https://en.wikipedia.org/wiki/List_of_HTTP_header_fields](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)

iana 注册：[https://www.iana.org/assignments/message-headers/message-headers.xhtml](https://www.iana.org/assignments/message-headers/message-headers.xhtml)

Referer Policy：[https://w3c.github.io/webappsec-referrer-policy/](https://w3c.github.io/webappsec-referrer-policy/)

阮一峰科普入门：[http://www.ruanyifeng.com/blog/2019/06/http-referer.html](http://www.ruanyifeng.com/blog/2019/06/http-referer.html)

Referer always：[https://github.com/nystudio107/seomatic/issues/254](https://github.com/nystudio107/seomatic/issues/254)