# express

> 介绍了express 框架的搭建， 配置， 简单使用和扩展用法



* **快速使用**

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

  