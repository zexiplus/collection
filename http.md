

###1.浏览器输入url后发生了什么？

1.DNS域名解析；
2.建立TCP连接；
3.发送HTTP请求；
4.服务器处理请求；
5.返回响应结果；
6.关闭TCP连接；
7.浏览器解析HTML；
8.浏览器布局渲染；

#### 步骤详解

**建立tcp连接**

![tcp三次握手](./imgs/tcp.jpg)

​	客户端：“你好，在家不，有你快递。”

​	服务端：“在的，送来就行。”

​	客户端：“好嘞。”

**发送http请求**

![http-request](./imgs/http_request.jpg)

**关闭tcp连接**

![closeLink](./imgs/closeLink.jpg)

客户端：“兄弟，我这边没数据要传了，咱关闭连接吧。”

服务端：“收到，我看看我这边有木有数据了。”

服务端：“兄弟，我这边也没数据要传你了，咱可以关闭连接了。”

客户端：“好嘞。”

**浏览器解析html**

浏览器需要加载解析的不仅仅是HTML，还包括CSS、JS。以及还要加载图片、视频等其他媒体资源。

浏览器通过解析HTML，生成DOM树，解析CSS，生成CSS规则树，然后通过DOM树和CSS规则树生成渲染树。渲染树与DOM树不同，渲染树中并没有head、display为none等不必显示的节点。

要注意的是，浏览器的解析过程并非是串连进行的，比如在解析CSS的同时，可以继续加载解析HTML，但在解析执行JS脚本时，会停止解析后续HTML，这就会出现阻塞问题。

**浏览器布局渲染**

根据渲染树布局，计算CSS样式，即每个节点在页面中的大小和位置等几何信息。HTML默认是流式布局的，CSS和js会打破这种布局，改变DOM的外观样式以及大小和位置。这时就要提到两个重要概念：replaint和reflow。

> replaint：屏幕的一部分重画，不影响整体布局，比如某个CSS的背景色变了，但元素的几何尺寸和位置不变。
> reflow： 意味着元件的几何尺寸变了，我们需要重新验证并计算渲染树。是渲染树的一部分或全部发生了变化。这就是Reflow，或是Layout。

所以我们应该尽量减少reflow和replaint，我想这也是为什么现在很少有用table布局的原因之一。

最后浏览器绘制各个节点，将页面展示给用户。

### 2.http头信息

> 请求头信息例子

```js
// http header json object
{
    host: "localhost:3001",
    referer: "localhost:3001/request",
    connection: "keep-alive",
    upgrade-insecure-requests: "1",
    user-agent: "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.87 Safari/537.36",
    accept: "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8",
    accept-encoding: "gzip, deflate, br",
    accept-language: "zh-CN,zh;q=0.9,en;q=0.8,zh-HK;q=0.7",
}
```

> 默认请求头含义

* Accept: 浏览器能够处理的内容类型

* Accept-Charset: 浏览器能够显示的字符集

* Accept-Encoding: 浏览器能够处理的压缩编码

* Accept-Language: 浏览器当前设置的语言

* Host: 发出请求的页面所在域

* Referer: 发送请求页面的url

* Cookie: 当前页面设置的任何cookie

* Connection: 浏览器与服务器之间的连接类型

  ```js
  // Connection : Keep-Alive 功能避免了建立或者重新建立连接
  connection: 'keep-alive',
  timeout = 5,max = 100 // 超过5秒建立新的连接， 最大请求100次
  
  // http/1.0
  // Keep-Alive，当服务器收到附带有Connection: Keep-Alive的请求时，它也会在响应头中添加一个同样的字段来使用Keep-Alive。这样一来，客户端和服务器之间的HTTP连接就会被保持，不会断开（超过Keep-Alive规定的时间，意外断电等情况除外），当客户端发送另外一个请求时，就使用这条已经建立的连接
  
  // http/1.1
  // 在HTTP/1.1版本中，官方规定的Keep-Alive使用标准和在HTTP/1.0版本中有些不同，默认情况下所在HTTP1.1中所有连接都被保持，除非在请求头或响应头中指明要关闭：Connection: Close  ，这也就是为什么Connection: Keep-Alive字段再没有意义的原因
  
  
  ```

  

* User-Agent: 浏览器类型字符串

> 响应头信息

