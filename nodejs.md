## node knowladge points

### require

*  require过的文件会加载到缓存，所以多次 require 同一个文件（模块）不会重复加载

*  ( a->b,b->a)循环引用并不会报错，导致的结果是 require 的结果是空对象 {}，原因是 b require 了 a，a 又去 require 了 b，此时 b 还没初始化好，所以只能拿到初始值 {}



### module.exports

* module.exports 初始值为一个空对象 {}
* exports 是指向的 module.exports 的引用
* require() 返回的是 module.exports 而不是 exports



### develop

> 使用forever 保持node长时间运行&监听文件变化自动重启服务

```shell
# 全局下载 forever
sudo npm i -g forever

# 使用 forever 启动node进程
forever start server.js

# 查看 forever 正在跑的进程
forever list

# 结束 forever 的node进程
forever stop server.js

# 启动并监听 node 文件的变化（文件变化后会自动重启）

forever -w start server.js

# [node路径，js文件路径，命令行参数]
process.argv        

# 系统环境对象
process.env                 

# 设置环境变量  
windows => set NODE_ENV=test node test.js  
linux => NODE_ENV=test node test.js
跨平台 => npm i cross-env -g             cross-env NODE_ENV=test node test.js

# 查看npm 配置 (全局node_modules路径位置之类)
npm config list 

# 设置npm配置npm install 全局安装路径
npm config set prefix /usr/local/node_modules

# 检查node代码
node inspect myscript.js
```



### async operator control

```js
var fs = require('fs')
var Promise = require('bluebird')  //第三方promise模块，兼容
global.Promise = Promise           //全局promise

function readFile(filename) {
  return new Promise((resolve, reject) => {
     fs.readFile(filename,(err, file) => {
       if(err) {
          reject(err)
       }      
       else resolve(file)
     })
  })
}
readFile('./one.js').then(data => {
  	console.log(data)
})

//自己写的把普通函数转换为promise函数的函数
Function.prototype.convertPromise = function() {
  var fn = this
  return function() {
      var args = [].slice.call(arguments, 0)
      return new Promise((resolve, reject) => {
          args.push(function (err, file) {
            if(err) {
              reject(err)
            }
            else {
              resolve(file)
            }
          })
    	  fn.apply(null, args)
      })
  }
}

var readFile2 = fs.readFile.convertPromise()
readFile2('./one.js').then(data => {
  	console.log(data)
})

```





### 常用库

```js
var path = require('path')            //内置路径处理模块
path.join(__dirname,'www/')           //return 'currPath/www'
path.resolve('/usr','./local','bin')   //把路径解析为绝对路径的函数
```

### fs

> File system flags

| Flag | detail                      |
| ---- | --------------------------- |
| a    | 打开文件进行追加。如果文件不存在，则创建该文件     |
| a+   | 打开文件读取并追加。如果文件不存在，则创建该文件    |
| r    | 打开文件并读取。如果文件不存在，则发生异常       |
| r+   | 打开文件读取并写入。如果不存在，则发生异常       |
| w    | 打开文件并写入。如果文件不存在则创建，存在则覆盖    |
| w+   | 打开文件并读取和写入。如果文件不存在则创建，存在则覆盖 |



> Open file

```js
const fs = require('fs')
fs.open('test.txt', 'r+', (err, fd) => {
    if (err) throw err
    console.log(fd)
})
```

> Write file

```js
// fs.writeFile(path, data, options, cb)
fs.writeFile('text.txt', 'hello world', {encoding: 'utf8', flag: 'w'}, err => {
    if (err) console.error(err)
})

// fs.write(fd, string, [position], [encoding], cb)
fs.open('test.txt', 'a+', (err, fd) => {
    fs.write(fd, 'hello world', err => {
        if (err) return console.error(err)
    })
    fs.close(fd, err => {
        if (err) console.error(err)
    })
})
```

> Read file

```js
// fs.readFile(path, options, cb)
fs.readFile('test.txt', {encoding: null, flag: 'r'}, (err, data) => {
    if (err) console.error(err)
    console.log(data)
})

// fs.read(fd, buffer, offset, length, position, cb)
fs.open('test.txt', 'r', (err, fd) => {
    if (err) return console.error(err)
    let buffer = new Buffer(1024)
    fs.read(fd, buffer, 0, buffer.length, 0, (err, bytes) => {
        if (err) return console.error(err)
        if (bytes > 0) {
            console.log(buffer.slice(0, bytes).toString())
        }
    })
})
```

> close file

```js
// fs.close(fd, cb)
fs.open('test.txt', 'r+', (err, fd) => {
    if (err) throw err
    fs.close(fd, err => {
        if (err) throw err
        console.log('close file successfully')
    })
})
```

> watch file

```js
// 监听filePath文件变化， 一旦改变，则运行回调函数
fs.watchFile('filePath', (curr, prev) => {
    console.log(`current file state is ${curr.sate}`)
    cp.exec('mv index.js index-dep.js')
})
```



### http
>  index.js

```js
var http = require('http')
//最基本服务器
http.createServer((req,res) => {
  if(req.url == '/hello') {
    res.end('hello')
  }
  if(req.url == '/world') {
    res.end('world')
  }
}).listen(6666)

```



> http **method&properties**

1. http.get(options, callback)

    ```js
      http.get('http://nodejs.org/dist/index.json', (res) => {
     const { statusCode } = res;
     const contentType = res.headers['content-type'];
      
     res.setEncoding('utf8');
     let rawData = '';
     res.on('data', (chunk) => { rawData += chunk; });
     res.on('end', () => {
       try {
         const parsedData = JSON.parse(rawData);
         console.log(parsedData);
       } catch (e) {
         console.error(e.message);
       }
     });
      }).on('error', (e) => {
     console.error(`错误: ${e.message}`);
      });
    ```



2. http.request(options,[callback])


   ```js
   const postData = querystring.stringify({
     'msg' : 'Hello World!'
   });
   
   const options = {
     hostname: 'www.google.com',
     port: 80,
     path: '/upload',
     method: 'POST',
     headers: {
       'Content-Type': 'application/x-www-form-urlencoded',
       'Content-Length': Buffer.byteLength(postData)
     }
   };
   
   const req = http.request(options, (res) => {
     res.setEncoding('utf8');
     res.on('data', (chunk) => {
       console.log(`响应主体: ${chunk}`);
     });
     res.on('end', () => {
       console.log('响应中已无数据。');
     });
   });
   
   req.on('error', (e) => {
     console.error(`请求遇到问题: ${e.message}`);
   });
   
   // 写入数据到请求主体
   req.write(postData);
   req.end();
   ```

   

   ​                         


> response  **methods&properties**
>

1. res.**setEncoding**('utf-8')                                      设置编码格式

2. res.**setHeader**('Content-type', 'text/html')      设置响应头

3. res.**headers**['content-type']                               获取content-type响应头

4. res.**resume**()                                                        消耗res, 释放内存空间

5. res.**writeHead**(statusCode, [headers])     设置响应头和响应码，会和setHeader合并，setHeader优先级高

   ```js
      res.writeHead(200, {
          'Content-Type': 'text/plain', 
          'Content-Length': Buffer.length(body)
      })
   ```


> request **methods&properties**


1. req.**headers**                                                        响应头对象,格式如下

   ```js
   { host: 'localhost:3001',
     connection: 'keep-alive',
     'cache-control': 'max-age=0',
     'upgrade-insecure-requests': '1',
     'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_4) ...',
     accept: 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
     'accept-encoding': 'gzip, deflate, br',
     'accept-language': 'zh-CN,zh;q=0.9,en;q=0.8,zh-HK;q=0.7',
      cookie: 'name=float'          // cookie会随请求头一起发送至服务器
   }
   ```



### path

```js
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..');// Returns: '/foo/bar/baz/asdf'

path.parse('C:\\path\\dir\\file.txt');// Returns:// { root: 'C:\\',//   dir: 'C:\\path\\dir',//   base: 'file.txt',//   ext: '.txt',//   name: 'file' }
┌─────────────────────┬────────────┐
│          dir        │    base    │
├──────┬              ├──────┬─────┤
│ root │              │ name │ ext │"  /    home/user/dir / file  .txt "
└──────┴──────────────┴──────┴─────┘
The path.resolve() method resolves a sequence of paths or path segments into an absolute path.
path.resolve('/foo/bar', './baz');// Returns: '/foo/bar/baz'

path.resolve('/foo/bar', '/tmp/file/');// Returns: '/tmp/file'

path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif');// if the current working directory is /home/myself/node,// this returns '/home/myself/node/wwwroot/static_files/gif/image.gif'

```



### process

```js
process.env           // 环境变量   系统上得键值对对象
DEBUG=1 node one.js   // process.env.DEBUG == 1

process.argv          // 运行node时指定得参数[node路径，文件路径，指定参数]
node one.js hello world // process.argv = [‘/usr/bin/node’,/home/code/one.js’,’hello’,’world’]

```

### dns

```js
const dns = require('dns')
// dns.lookup 提供域名，解析出ip地址
dns.lookup('hostname', (err, ipAddress, ipVersion) => {
    
})
// dns.resolve4 与 dns.lookup作用相同，实现不同
// dns.reverse 提供ip地址 反向解析出域名

dns.resolve4('hostname', (err, ipAddress) => {
    dns.reverse(ipAddress, (err, hostname) => {
        console.log(hostname)
    })
})

// dns.Resolver 使用特定的值解析
const server = new dns.Resolver([192.168.17.108])
server.resolve4('hello.com', (err, ipAddress) => {
    console.log(ipAddress) // 192.168.17.108
})

// 取消解析
server.cancel() 

// 设置和返回当前dns解析的ip数组
server.setServers(['123.23.22.22'])
server.getServers()
```



### child_process

```js
const cp = require('child_process')

// spawn 第一个参数命令，第二个参数 命令的参数
let ls = cp.spawn('ls', ['-lh', '/usr'])
ls.stdout.on('data', (data) => {
    console.log(data)
})
ls.stdin.on('data', data => {
    console.log(data)
})
ls.stderr.on('data', err => {
    console.err(err)
})
ls.on('close', code => {
    console.log(`退出码${code}`)
})
```



### cluster

```js

```





### connect

```js
var connect = require('connect')
var http = require('http')
var app = connect()
app.use((req,res,next)=> {
  res.send('middleware')
  next()          // 全局中间件要执行next才能执行其它中间件
})
app.use('/hello'(req,res) => {   
  res.end('hello')
})
http.createServer(app).listen(555)
```



### express

```js

```

