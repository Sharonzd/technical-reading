# 第14章 安全 HTTP

## HTTP 的安全性要求

- 服务器认证
- 客户端认证
- 完整性
- 加密
- 效率
- 普适性
- 管理的可扩展性
- 适应性
- 社会可行性

## HTTPS 概念

HTTPS （竟然也是网景94年首创，协议见 HTTP Over TLS [https://tools.ietf.org/html/rfc2818](https://tools.ietf.org/html/rfc2818) ）是指在 HTTP 和 TCP 之间多一层安全层，可以是 **SSL**（Secure Sockets Layer, 安全套接层，已经被弃用了） 或者 **TLS**（Transport Layer Security，传输层安全，协议见 [https://tools.ietf.org/html/rfc8446](https://tools.ietf.org/html/rfc8446)）。

## 数字加密的一些概念

**密码**：一套编码方案，一种特殊的报文编码方式和一种之后使用的相应解码方式的结合体。加密前的原始报文叫明文，加密后叫密文。

**密钥**：能够用来改变密码行为的数字化参数。

**对称密钥加密系统**：编码和解码使用相同的密钥。可能会被暴力攻击，不过长度很长的，比如128位的密钥就很难通过暴力攻击破解了。而且由于需要使用对称的共享的密钥，每个用户都需要一个密钥，那么 N 个节点就需要 N²个密钥。

**不对称密钥加密系统**：编码和解码使用不同的密钥。

**公开密钥加密系统**：使用两个非对称的密钥，一个用来对主机的报文编码（公钥），另一个用来对主机报文解码（私钥）。比较流行的算法是 RSA 算法

**数字签名**：用来验证报文未被篡改的校验和，通常是用非对称公开密钥技术产生的。提取报文的摘要，然后使用私有密钥作为参数计算出私有密钥，一起发给接收端。

**数字证书**：由一个可信的组织验证和签发的识别信息，包含对象名称，过期时间，证书发布者，证书发布者的数字签名，对象的公开密钥，签名算法描述信息等等。最常用的标准格式为 **X.509 v3** 证书

## HTTPS 方案过程：

1. 建立安全传输：客户端打开一条到 web 服务器端口 443 的 TCP 连接，建立好连接。
2. 客户端和服务器初始化 TLS 层，对加密参数进行沟通，并交换密钥 TLS 安全参数：
    1. 交换协议版本号
    2. 选择一个两端都了解的密码（客户端发送可供选择的密码并请求证书，服务器发送选中的密码和证书）
    3. 对两端身份进行认证（客户端发送保密信息；客户端和服务器生成密钥）
    4. 生成临时的会话密钥，以便加密信道
3. 客户端在 TLS 上发送 HTTP 请求/在 TCP 上发送已加密的请求
4. 服务端在 TLS 上发送 HTTP 响应/在 TCP 上发送已加密的响应
5. TLS 关闭通知
6. TCP 连接关闭

## Web 服务器证书有效性算法验证步骤：

1. 日期检测
2. 签名颁发者可信度检测
3. 签名检测
4. 站点身份检测

## 通过代理以隧道形式传输安全流量

当客户端使用服务器的公开密钥对数据进行加密时，代理无法读取 HTTP 首部，导致无法转发请求。常用的 解决办法是使用 HTTPS 隧道协议。

在加密前，客户端会先用 CONNECT 方法明文告诉代理它要连接的安全主机和端口。打开隧道后，就可以开始传输 TLS 数据了。


// TODO:  实现一个 HTTP 客户端

手写系列（感觉国人文章太少了）

- [https://www.zhihu.com/question/27896945](https://www.zhihu.com/question/27896945)
- [https://medium.com/from-the-scratch/http-server-what-do-you-need-to-know-to-build-a-simple-http-server-from-scratch-d1ef8945e4fa](https://medium.com/from-the-scratch/http-server-what-do-you-need-to-know-to-build-a-simple-http-server-from-scratch-d1ef8945e4fa)

- （科普文，攒着之后看）[http://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html](http://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html)
