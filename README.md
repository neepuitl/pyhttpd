# pyhttpd
pyhttpd是一个基于python的http网络服务软件，该项目参考了[tinyhppd](https://github.com/EZLippi/Tinyhttpd)并力求以最少的代码复现http协议。

## 1. 环境
- 编译器: `Python3`

## 2. 使用
> 1) git clone https://github.com/neepuitl/pyhttpd.git
> 2) cd pyhttpd
> 3) python3 httpd.py

## 3. 原理
HTTP协议（HyperText Transfer Protocol，超文本传输协议）是因特网上应用最为广泛的一种网络传输协议，所有的WWW文件都必须遵守这个标准。

HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

### 3.1. HTTP工作原理

HTTP协议工作于客户端-服务端架构上。浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。

Web服务器有：Apache服务器，IIS服务器（Internet Information Services）等。

Web服务器根据接收到的请求后，向客户端发送响应信息。

HTTP默认端口号为80，但是你也可以改为8080或者其他端口。

HTTP三点注意事项：

- HTTP是无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
- HTTP是媒体独立的：这意味着，只要客户端和服务器知道如何处理的数据内容，任何类型的数据都可以通过HTTP发送。客户端以及服务器指定使用适合的MIME-type内容类型。
- HTTP是无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

以下图表展示了HTTP协议通信流程：

![HTTP协议通信流程](docs/images/1.gif)

### 3.2. HTTP消息结构

HTTP是基于客户端/服务端（C/S）的架构模型，通过一个可靠的链接来交换信息，是一个无状态的请求/响应协议。

一个HTTP"客户端"是一个应用程序（Web浏览器或其他任何客户端），通过连接到服务器达到向服务器发送一个或多个HTTP的请求的目的。

一个HTTP"服务器"同样也是一个应用程序（通常是一个Web服务，如Apache Web服务器或IIS服务器等），通过接收客户端的请求并向客户端发送HTTP响应数据。

HTTP使用统一资源标识符（Uniform Resource Identifiers, URI）来传输数据和建立连接。

一旦建立连接后，数据消息就通过类似Internet邮件所使用的格式[RFC5322]和多用途Internet邮件扩展（MIME）[RFC2045]来传送。

客户端发送一个HTTP请求到服务器的请求消息包括以下格式：请求行（request line）、请求头部（header）、空行和请求数据四个部分组成，下图给出了请求报文的一般格式。

![请求报文的一般格式](docs/images/2.png)

HTTP响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文。

![](/image/http/http03.png)

实例

```http
// 客户端请求
GET /hello.txt HTTP/1.1
User-Agent: curl/7.16.3 libcurl/7.16.3 OpenSSL/0.9.7l zlib/1.2.3
Host: www.example.com
Accept-Language: en, mi
// 服务端响应
HTTP/1.1 200 OK
Date: Mon, 27 Jul 2009 12:28:53 GMT
Server: Apache
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
ETag: "34aa387-d-1568eb00"
Accept-Ranges: bytes
Content-Length: 51
Vary: Accept-Encoding
Content-Type: text/plain
// 输出
Hello World! My payload includes a trailing CRLF.
```

### 3.3. HTTP请求方法

|           |                                                                                                                          |
| --------- | ------------------------------------------------------------------------------------------------------------------------ |
|GET        |请求指定的页面信息，并返回实体主体。                                                                                          |
|HEAD       |类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头                                                                   |
|POST       |向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。|
|PUT        |从客户端向服务器传送的数据取代指定的文档的内容。                                                                               |
|DELETE     |请求服务器删除指定的页面。                                                                                                   |
|CONNECT    |HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。                                                                        |
|OPTIONS    |允许客户端查看服务器的性能。                                                                                                  |
|TRACE      |回显服务器收到的请求，主要用于测试或诊断。                                                                                     |