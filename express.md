# express

> 介绍了express 框架的搭建， 配置， 简单使用和扩展用法



#### 快速使用

```js
var express = require('express')
var path = require('path')
var app = express()

// 应用中间件
app.use(express.static(path.join(__dirname, 'www/')))
app.use((req,res,next) => {
  
})
app.get('/',(req,res) => {
  res.end('hello')
})

// 挂载多个中间件函数
app.get('/',fn1,fn2,fn3) 
app.listen(3000)

// 定义路由
var users = express.Router()
users.get('/',fn)
users.get('/home',fn)

// 自定义路径
router.get('/:name', function (req, res) {     
  res.send('hello, ' + req.params.name)
})
app.use(users)
```



###API

* #### express

  * **express()**

    > 返回一个服务器对象实例

    ```js
    const app = express()
    ```

    

  * **express.static(dirname)**

    > 设置express服务器静态目录

    ```js
    app.use(express.static(`${__dirname}/static`))
    ```

  * **http.createServer(app)**

    > http 代理express服务器

    ```js
    http.createServer(app).listen(3001)
    ```

    

* #### app

  * **app.use(path, fn)**

    > 使用中间件

    ```js
    // 全局使用
    app.use((req, res, next) => {
        // 设置状态码和响应头
        res.writeHead(200, {'Content-Type': 'text/plain'})
        
        // 根据不同的url 设置不同的响应体
        switch (req.url) {
        	case: '/':
            res.end('hello world');
            break;
            case: 'doc':
            res.end('welcome to doc page')
            break;
            default:
            res.end('where are you')
        }
        
        // 调用下一个中间件函数, 不调用next request对象就不再向后传递了
        next(); 
    })
    
    // 匹配 '/path'路由调用中间件
    app.use('/path', (req, res, next) => {})
    ```

    

  * **app.all('*', fn)**

    > 所有请求都经过此方法

    ```js
    app.all('*', (req, res, next) => {
        res.writeHead(200, {
            'Content-Type': 'text/plain'
        })
        next()
    })
    ```

    

  * **app.get(path, fn),   app.post(),    app.put(),    app.delete ...**

    > 响应不同请求方法

    ```js
    app.get('/home', (req, res) => {})
    app.post('/login', (req, res) => {})
    
    // put 请求用于上传资源
    app.put('/logout', (req, res) => {})
    
    // get 动态匹配
    app.get('/path/:id', (req, res) => {
        res.end(`请求参数是${req.params.id}`)
    })
    
    // 参数后加问号表示可选参数
    app.get('/path/:name?', (req, res) => {
        if (req.params.name) { // do something }
    })
    
    // 匹配多个参数 
    app.get('/path/:where/who/:name', (req, res) => {
        res.end(`path is ${req.params.where}, name is ${req.params.name}`)
    })
        
    // 匹配正则表达式
    app.get(/^\/path\/(\w+)(\.\.(\w+))?/, (req, res) => {
    
    })
    ```

  

  - **app.set(key, val)**

    > 设置express实例变量

    ```js
    // 设置模版引擎和模版目录
    app.set('view engine', 'pug')
    app.set('views', __dirname + '/views')
    ```

    

  - **app.listen(port, [callback])**

    > 监听端口

    

  - **app.engine(ext, callback)**

    > 设置模版引擎

    ```js
    // 模版引擎设置为pug
    app.engine('pug', require('pug').__express)
    ```

  

  - **app.path()**

    > 返回express实例所对应的路径

    ```js
    const app = express()
    const doc = express()
    const blog = express()
    app.use('/doc', doc)
    doc.use('/blog', blob)
    
    // returns /doc/blog
    console.log(blog.path())
    ```

* #### request

  * **req.ip**

    > 请求的IP地址

    

  * **req.files**

    > 请求上传的文件

    

  * **req.method**

    > 请求的方法

    

  * **req.params**

    > 请求的参数对象

    ```js
    let url = 'http://www.test.com/params/123/params2/345';
    req.params // returns {params: 123, params2: 345}
    ```

  * **req.url**

    > 请求的路径

    

  * **req.query**

    > 请求的查询参数

    ```js
    let url = 'http://www.test.com/search?q=123&s=456'
    req.query.q // returns 123
    req.query // {q: 123, s: 456}
    ```

    



* #### response

  * **res.writeHead(statusCode, options)**

    > 设置http状态吗和响应头信息

  * ```js
    res.writeHead(200, {'Content-Type': 'text/plain', 'Content-Length': 1234})
    ```

  * **res.setHeader(key, value)**

    > 设置响应头

    ```js
    res.setHeader('cache-control': 'max-age=315360000, public, immutable')
    ```

    

  * **res.redirect([statusCode], url)**

    >重定向

    ```js
    res.redirect(301, 'http://www.bing.com')
    res.redirect('http://www.bing.com')
    ```
    

  * **res.sendFile(filePath)**

    >发送文件

    ```js
    res.sendFile('/path/to/play.mp4');
    ```
    

  * **res.render(templateName, message)**

    > 用模版渲染页面

    ```js
    // 在模版目录里面找到 index 前缀文件, 
    res.render('index', {message: 'hello world'})
    ```

    



### express middleware

[http://www.expressjs.com.cn/resources/middleware.html](http://www.expressjs.com.cn/resources/middleware.html)

* #### 内置中间件

  * **express.static()**

    > 设置静态目录, 存放静态资源

    ```js
    app.use(express.static(__dirname + '/static'))
    ```

    

* 第三方中间件

  * **cookie-parser**

    > 用于解析cookie到, cookie-parser不会对出站cookie做设置, 需要手动调用res.setHeader('Set-Cookie': '...')

    > signedCookie : cookie 在出站时生成一**段防篡改验证码**

    ```js
    res.setHeader('Set-Cookie', `name=${xiaoxixi}|${md5(123454)}`)
    ```

    ```js
    const cookieParser = require('cookie-parser')
    app.use(cookieParser())
    app.get('/', (req, res) => {
        // Cookie: name=xiaoxixi, age=24
        console.log(req.cookies)  // {name: 'xiaoxixi', age: '24'}
        
        // Cookie: user=tobi.CP7AWaXDfAKIRfH49dQzKJx7sKzzSoPq7/AcBBRVwlI3
        console.log(req.signedCookies) // tobi
    })
    
    ```

  * **body-parser**

    > 处理post请求体

    ```js
    const bodyParser = require('body-parser')
    
    // parse application/x-www-form-urlencoded
    app.use(bodyParser.urlencoded({ extend: false}))
    
    // parse application/json
    app.use(bodyParser.json())
    ```

  * **cookie-session**

    > session 作为存储在服务器端的标识符, 常常存在于散列中, 通过客户端发送的sessionid来判断会话属于哪个用户

    ```js
    // cookie-session 设置出站cookie 
    // 例如 Set-Cookie: session=eyJ2aWV3cyI6NH0=; path=/; httponly
    
    const cookieSession = require('cookie-session')
    app.use(cookieSession({
        name: 'session',
        keys: ['key1', 'key2'],
        maxAge: 24 * 60 * 60 * 1000 // one day
    }))
    
    app.get('/', function (req, res, next) {
      // Update views
      req.session.views = (req.session.views || 0) + 1
    
      // Write response
      res.end(req.session.views + ' views')
    })
    ```

    