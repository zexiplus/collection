# koa

> 官方doc  [链接](https://koajs.com/)

#### 1.最小demo

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



#### 2. middleware

###### 定义中间件

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

###### 使用中间件

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
const user = new Router()
user.get('/', ctx => {
    
})
```

#### 4.ctx

```js

```



