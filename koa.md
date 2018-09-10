# koa

> 官方doc  [link](https://koajs.com/)
>
> 笔记 [link](https://github.com/zexiplus/koa2-note)



#### catalogue

[TOC]

#### 1. koa app

> Index.js

```js
const Koa = require('koa')

const app = new Koa()

// compute response time
app.use(async (ctx, next) => {
	const startTime = new Date()
	await next()
	const endTime = new Date()
	const time = endTime - startTime
	
	ctx.set('X-Response-Time', `${time}ms`)
})


app.use(async (ctx) => {
	ctx.body = 'hello world'
})

app.listen(3000)
console.log('the server is listenning at 3000 port')
```

| app's attributes | using                             | explian                  |
| ---------------- | --------------------------------- | ------------------------ |
| app.listen       | app.listen(3000)                  | 监听端口                     |
| app.env          | app.env == 'development'          | app环境变量                  |
| app.use          | app.use(function (ctx, next) {})  | 使用中间件                    |
| app.keys         | app.keys = ['name', 'xiaoxixi']   | 设置cookie                 |
| app.context.db   | app.context.db = new Db()         | 全局context 设置             |
| app.slient       | app.slient = true                 | 关闭警告模式(error事件不会出发)      |
| app.on           | app.on('error \| ')               | 监听事件                     |
| app.proxy        | app.proxy = true                  | 设置代理                     |
| app.callback()   | http.createServer(app.callback()) | 返回可以被http/connect使用的回调函数 |




#### 2. middleware

###### define middleware

> middleware/log.js

```js
function log ( ctx ) {
    console.log(`request method is ${ctx.method}`)
    console.log(`request header is ${JSON.stringify(ctx.header)}`)
}

module.exports = function () {
    return async function ( ctx, next ) {
        log(ctx)
        await next()
    }
}
```

###### use middleware

> index.js

```js
const log = require('./middleware/log.js')
const app = new Koa()

app.use(log())
```



#### 3.router

```js
// 手动实现路由

app.use((ctx, next) => {
    let body
    switch (ctx.request.url) {
            case '/':
            body = 'hello one'
            break
            case '/home':
            body = 'hello home'
            break
    }
    ctx.body = body
})

// koa-router 中间件
const Router = require('koa-router')

// 定义2个子路由
const config = new Router()
const home = new Router()

home.get('/one', ctx => {
   ctx.body = 'home one' 
})

config.get('/user', ctx => {
    ctx.body = 'user'
}).get('/setting', ctx => {
    ctx.body = setting
})

// 创建父路由，装载子路由
const router = new Router()
router.use('/home', home.routes(), home.allowedMethods())
router.use('/config', config.routes(), config.allowedMethods())

// app对象使用父路由
app.use(router.routes()).use(router.allowedMethods())

```



#### 4.ctx

> **context(ctx)**  包含http-response 和 http-request 对象

| ctx's attributes |                                          | Explian      |
| ---------------- | ---------------------------------------- | ------------ |
| ctx.request      |                                          | koa 请求对象     |
| ctx.response     |                                          | koa 响应对象     |
| ctx.req          |                                          | node http 请求 |
| ctx.res          |                                          | node http 响应 |
| ctx.state        | ctx.state.user = db.find({name: 'xiaoxixi'}) | 推荐的命名空间      |
| ctx.app          |                                          | 对 app 引用     |
| ctx.cookies.set  | ctx.cookies.set('name', 'xiaoxix', { signed: true }) | 设置响应体cookie  |
| ctx.cookies.get  | ctx.cookies.get('name')                  | 获取cookie     |
| ctx.throw        | ctx.throw(400, 'not found')              | 手动抛出错误       |



#### 5. ctx.request

| ctx.request attribute           | alias                                    | Explian                        | demo                                     |
| ------------------------------- | ---------------------------------------- | ------------------------------ | ---------------------------------------- |
| ctx.request.header              | ctx.header, ctx.request.headers,  ctx.headers | 请求头                            |                                          |
| ctx.request.method              | ctx.method                               | 请求方法                           | ctx.method = 'post'                      |
| ctx.request.origin              | ctx.origin                               | 请求源地址(协议和地址)                   | =>  http://bing.com                      |
| ctx.request.length              |                                          | 请求长度  Content-Length           |                                          |
| ctx.request.url                 | ctx.url                                  | 请求url(不包括host)                 | /path?a=123&b=456                        |
| ctx.requset.path                | ctx.path                                 | 请求路径                           | /path                                    |
| ctx.request.href                | ctx.href                                 | 请求href(protocol, host and url) | (http://example.com/foo/bar?q=1)         |
| ctx.request.queryString         | ctx.queryString                          | 查询字符串                          | a=123&b=456                              |
| ctx.request.search              | ctx.search                               | 完整的查询字符串                       | ?a=123&b=456                             |
| ctx.request.host                | ctx.host                                 | 请求的主机                          |                                          |
| ctx.request.type                |                                          | 请求类型                           | image/png                                |
| ctx.request.charset             |                                          | 请求的字体类别                        | 'utf-8'                                  |
| ctx.request.query               | ctx.query                                | 请求的查询对象                        | ctx.request.query = {color: 'blue', age: 12} |
| ctx.request.fresh               | ctx.fresh     true 没有改变，   false改变了      | 请求体没有改变                        | If (ctx.fresh) {ctx.status = 304}        |
| ctx.request.stale               | ctx.stale                                | ctx.fresh的反义词                  |                                          |
| ctx.request.secure              | ctx.secure                               | 检查协议是否为https                   | ctx.protocol === 'https'                 |
| ctx.request.ip                  | ctx.ip                                   | 请求主机的ip                        |                                          |
| ctx.request.ips                 | ctx.ips                                  | 请求主机的ip数组                      |                                          |
| ctx.request.is(types)           | ctx.is(typs)                             | 判断请求的类型                        | // With Content-Type: text/html; charset=utf-8 ctx.is('html'); // => 'html' ctx.is('text/html'); // => 'text/html' ctx.is('text/*', 'text/html'); // => 'text/html'                                   ctx.is('text/json') // => false |
| ctx.request.accepts(types)      | ctx.accepts(types)                       | 判断请求的类型是否为可接受的                 | // Accept: text/html ctx.accepts('html'); // => "html" |
| ctx.request.acceptsEncoding()   | ctx.acceptsEncoding()                    | 判断可接受的编码类型                     | // Accept-Encoding: gzip ctx.acceptsEncodings('gzip', 'deflate', 'identity'); // => "gzip"  ctx.acceptsEncodings(['gzip', 'deflate', 'identity']); // => "gzip" |
| ctx.request.acceptsCharset()    | ctx.acceptsCharset()                     | 判断可接受的字符类型                     | // Accept-Charset: utf-8, iso-8859-1;q=0.2, utf-7;q=0.5 ctx.acceptsCharsets(); // => ["utf-8", "utf-7", "iso-8859-1"] |
| ctx.request.accerptsLanguages() | ctx.acceptsLanguages()                   | 判断可接受的语言类型                     | // Accept-Language: en;q=0.8, es, pt ctx.acceptsLanguages('es', 'en'); // => "es"  ctx.acceptsLanguages(['en', 'es']); // => "es" |
| ctx.request.socket              |                                          | 返回request的socket连接对象           |                                          |
| ctx.request.get(filed)          |                                          | 获得请求头字段                        | request.get(‘Content-Type‘)              |



#### 6.ctx.response

| ctx.response.attributes           | alias                    | Explian                    | Demo                                     |
| --------------------------------- | ------------------------ | -------------------------- | ---------------------------------------- |
| ctx.response.header               | ctx.response.headers     | 响应头对象                      |                                          |
| ctx.response.socket               |                          | 响应socket                   |                                          |
| ctx.response.status               | ctx.status               | 获得/设置响应的状态码                | ctx.status=200              ctx.status  // => 304 |
| ctx.response.message              | ctx.message              | 获得/设置响应的状态码信息              | ctx.message = 'ok'              ctx.message // => 'not found' |
| ctx.response.length               | ctx.length               | 获得/设置响应的长度                 | ctx.length = 199                          ctx.length // => 300 |
| ctx.response.body                 | ctx.body                 | 获得/设置响应体                   | ctx.body = 'hello world'              ctx.body // => '123' |
| ctx.response.get(field)           |                          | 获得相应头字段                    | ctx.response.get('Content-Type')         |
| ctx.response.append(field,val)    |                          | 添加响应头字段                    | ctx.response.append('Link', 'localhost') |
| ctx.response.set(object)          | ctx.set(object)          | 设置响应头信息                    | ctx.set({'Etag': '1234',   'Last-Modified': date }) |
| ctx.response.remove(field)        | ctx.remove(field)        | 删除响应头字段                    | ctx.remove('Content-Type')               |
| ctx.response.type                 |                          | 获取/设置响应体类型                 | ctx.response.type // => text/html                                 ctx.response.type = 'image/png' |
| ctx.response.is(types)            |                          | 判断响应体的类型                   | if(ctx.response.is('html')) return       |
| ctx.response.redirect(url)        | ctx.redirect(url)        | 响应跳转                       | ctx.redirect('back'); ctx.redirect('back', '/index.html'); ctx.redirect('/login');      ctx.redirect('http://google.com'); |
| ctx.response.attachment(filename) | ctx.attachment(filename) | 触发浏览器下载                    |                                          |
| ctx.response.headerSent           | ctx.headerSent           | 检查是否已经发送响应头                | if (ctx.headerSent) { // do}             |
| ctx.response.lastModified         | ctx.lastModified         | 返回/设置Last-Modified响应头的日期对象 | ctx.lastModified = new Date();           |
| ctx.response.etag                 | ctx.etag                 |                            | ctx.etag = crypto.createHash('md5').update(ctx.body).digest('hex'); |
| ctx.response.vary(field)          |                          |                            |                                          |
| ctx.response.flushHeaders()       |                          | 刷新响应头，并开始响应体               |                                          |






