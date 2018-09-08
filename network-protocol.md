# 网络协议原理

收录了一些网络协议, 网络状态的总结



### 目录

[TOC]





### 浏览器输入url后发生了什么？

1.DNS域名解析；
2.建立TCP连接；
3.发送HTTP请求；
4.服务器处理请求；
5.返回响应结果；

6.关闭TCP连接；

7.浏览器解析HTML；
8.浏览器布局渲染；

#### 步骤详解

* **建立tcp连接**

![tcp三次握手](./imgs/tcp.jpg)

​	客户端：“你好，在家不，有你快递。”

​	服务端：“在的，送来就行。”

​	客户端：“好嘞。”

* **发送http请求**

![http-request](./imgs/http_request.jpg)

* **关闭tcp连接**

![closeLink](./imgs/closeLink.jpg)

	客户端：“兄弟，我这边没数据要传了，咱关闭连接吧。”

	服务端：“收到，我看看我这边有木有数据了。”

	服务端：“兄弟，我这边也没数据要传你了，咱可以关闭连接了。”

	客户端：“好嘞。”

* **浏览器解析html**

  浏览器需要加载解析的不仅仅是HTML，还包括CSS、JS。以及还要加载图片、视频等其他媒体资源。

  浏览器通过解析HTML，生成DOM树**(dom tree)**，解析CSS，生成CSS规则树，然后通过DOM树和CSS规则树生成渲染树。渲染树与DOM树不同，渲染树中并没有head、display为none等不必显示的节点。

  要注意的是，浏览器的解析过程并非是串连进行的，比如在解析CSS的同时，可以继续加载解析HTML，但在解析执行JS脚本时，会停止解析后续HTML，这就会出现阻塞问题。

* **浏览器布局渲染**

  根据渲染树**(render tree)**布局，计算CSS样式，即每个节点在页面中的大小和位置等几何信息。HTML默认是流式布局的，CSS和js会打破这种布局，改变DOM的外观样式以及大小和位置。这时就要提到两个重要概念：replaint和reflow。



  **replaint(重绘)**：屏幕的一部分重画，不影响整体布局，比如某个CSS的背景色变了，但元素的几何尺寸和位置不			   变。
    **reflow(重构)**： 意味着元件的几何尺寸变了，我们需要重新验证并计算渲染树。是渲染树的一部分或全部发生了变化。这就是Reflow，或是Layout。


	所以我们应该尽量减少reflow和replaint，我想这也是为什么现在很少有用table布局的原因之一。

	最后浏览器绘制各个节点，将页面展示给用户。



### http头信息

* **请求头对象例子**

  ```js
  {
      host: "localhost:3001",
      referer: "localhost:3001/request",
      connection: "keep-alive",
      upgrade-insecure-requests: "1",
      user-agent: "AppleWebKit/537.36 Chrome/67.0.3396.87 Safari/537.36",
      accept: "text/html,application/xhtml+xml,image/webp,image/apng,*/*;q=0.8",
      accept-encoding: "gzip, deflate, br",
      accept-language: "zh-CN,zh;q=0.9,en;q=0.8,zh-HK;q=0.7",
  }
  ```


* **请求头信息**

  | 字段            | 含义                                                         |
  | --------------- | ------------------------------------------------------------ |
  | Accept          | 浏览器能够处理的内容类型                                     |
  | Accept-Charset  | 浏览器能够显示的字符集                                       |
  | Accept-Encoding | 浏览器能够处理的压缩编码                                     |
  | Accept-Language | 浏览器当前设置的语言                                         |
  | Host            | 发出请求的页面所在域                                         |
  | Referer         | 发送请求页面的url                                            |
  | Cookie          | 当前页面设置的任何cookie                                     |
  | Connection      | 浏览器与服务器之间的连接类型,Connection : Keep-Alive 功能避免了建立或者重新建立连接, 超过5秒建立新的连接， 最大请求100次, connection: 'keep-alive, timeout = 5,max = 100' |
  | User-Agent      | 浏览器类型字符串                                             |

* **响应头信息**

  > expires 和 cache-control : max-age = 1000 与缓存有关， expires 是一个过期日期， max-age是时长， 二者可以相互推算

  | 响应头                   | 解释                                                         |
  | ------------------------ | ------------------------------------------------------------ |
  | Allow                    | 服务器支持哪些请求方法（如GET、POST等）                      |
  | Content-Encoding         | 文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。 |
  | Content-Length           | 表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据。 |
  | Content-Type             | 表示后面的文档属于什么MIME类型。Servlet默认为text/plain，但通常需要显式地指定为text/html。 |
  | Date                     | 当前的GMT时间。你可以用setDateHeader来设置这个头以避免转换时间格式的麻烦。 |
  | Expires                  | 应该在什么时候认为文档已经过期，从而不再缓存它               |
  | Last-Modified            | 文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。Last-Modified也可用setDateHeader方法来设置。 |
  | Location                 | 表示客户应当到哪里去提取文档。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302。 |
  | Refresh                  | 表示浏览器应该在多少时间之后刷新文档，以秒计。除了刷新当前文档之外，你还可以通过setHeader("Refresh", "5; URL=http://host/path")让浏览器读取指定的页面。 |
  | Server                   | 服务器名字。Servlet一般不设置这个值，而是由Web服务器自己设置。 |
  | Set-Cookie               | 设置和页面关联的Cookie。                                     |
  | WWW-Authenticate         | 客户应该在Authorization头中提供什么类型的授权信息？在包含401（Unauthorized）状态行的应答中这个头是必需的。例如，response.setHeader("WWW-Authenticate", "BASIC realm=＼"executives＼"")。  。 |
  | Etag                     | 是资源的特定版本识别符, 可以让缓存更高效,如果没有改变, web服务器不需要发送完整的响应. 例如  ETag  :  "33a64df551425fcc55e4d42a148795d9f25f89d4" |
  | Cache-Control : no-cache | 缓存设置时间                                                 |

   




### http 状态

* **常用状态码**

  | status code | information                                    |
  | ----------- | ---------------------------------------------- |
  | 200         | 请求成功                                       |
  | 201         | 已创建, 请求成功并且服务器创建了新资源         |
  | 202         | 已接受, 服务器已接受请求, 但尚未处理           |
  | 304         | 未修改                                         |
  | 301         | 永久移动                                       |
  | 302         | 临时移动                                       |
  | 400         | 参数错误                                       |
  | 401         | 请求未授权                                     |
  | 403         | 拒绝访问                                       |
  | 404         | 地址不存在                                     |
  | 405         | 客户端请求中的方法被禁止（一般是请求方式错误） |
  | 500         | 服务器报错                                     |
  | 501         | 尚未实施, 服务器还不存在请求功能               |
  | 502         | 请求超时，无效网关                             |
  | 503         | 服务器超载或者维护，无法响应                   |

* **全部请求头**

  | status code | information                     |
  | ----------- | ------------------------------- |
  | 100         | continue                        |
  | 101         | switching protocols             |
  | 102         | processing                      |
  | 200         | ok                              |
  | 201         | created                         |
  | 202         | accepted                        |
  | 203         | non-authoritative information   |
  | 204         | no content                      |
  | 205         | reset content                   |
  | 206         | partial content                 |
  | 207         | multi-status                    |
  | 208         | already reported                |
  | 226         | im used                         |
  | 300         | multiple choices                |
  | 302         | found                           |
  | 303         | see other                       |
  | 304         | not modified                    |
  | 305         | use proxy                       |
  | 307         | temporary redirect              |
  | 308         | permanent redirect              |
  | 400         | bad request                     |
  | 401         | unauthorized                    |
  | 402         | payment required                |
  | 403         | forbidden                       |
  | 404         | not found                       |
  | 405         | method not allowed              |
  | 406         | not acceptable                  |
  | 407         | proxy authentication required   |
  | 408         | request timeout                 |
  | 409         | conflict                        |
  | 410         | gone                            |
  | 411         | length required                 |
  | 412         | precondition failed             |
  | 413         | payload too large               |
  | 414         | uri too long                    |
  | 415         | unsupported media type          |
  | 416         | range not satisfiable           |
  | 417         | expectation failed              |
  | 418         | I'm a teapot                    |
  | 422         | unprocessable entity            |
  | 423         | locked                          |
  | 424         | failed dependency               |
  | 426         | upgrade required                |
  | 428         | precondition required           |
  | 429         | too many requests               |
  | 431         | request header fields too large |
  | 500         | internal server error           |
  | 501         | not implemented                 |
  | 502         | bad gateway                     |
  | 503         | service unavailable             |
  | 504         | gateway timeout                 |
  | 505         | http version not supported      |
  | 506         | variant also negotiates         |
  | 507         | insufficient storage            |
  | 508         | loop detected                   |
  | 510         | not extended                    |
  | 511         | network authentication required |


 