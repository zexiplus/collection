# Nodejs

nodejs 参考指南



## Catalague

[TOC]



## Module

*  require过的文件会加载到缓存，所以多次 require 同一个文件（模块）不会重复加载
*  ( a->b,b->a )循环引用并不会报错，导致的结果是 require 的结果是空对象 {}，原因是 b require 了 a，a 又去 require 了 b，此时 b 还没初始化好，所以只能拿到初始值 {}
*  module.exports 初始值为一个空对象 {}
*  exports 是指向的 module.exports 的引用
*  require() 返回的是 module.exports 而不是 exports
*  **require.cache**  保存了当前模块引用的所有模块



## Node REPL 

* .break 退出noderepl
* .help 查看所有node repl命令



## Develop Cli

> 常用node开发调试 cli命令

```shell
# 查看npm 配置 (全局node_modules路径位置之类)
npm config list 

# 设置npm配置npm install 全局安装路径
npm config set prefix /usr/local/node_modules

# 检查node代码
node inspect myscript.js

# 根据进程端口查看进程信息
lsof -i :3000

# 根据pid 杀死进程
kill -9 pid
```



## Built-in module



#### os

> **获取系统信息**

> http://nodejs.cn/api/os.html#os_os_platform

* **os.platform()**

  > 显示系统平台， 等价于 process.platform

  > darwin - mac系统， win32 - window系统， linux- linux系统

* **os.arch()**

  > 系统架构 例如 'x64'

* **os.cpus()**

  > 返回系统的cpus数组

* **os.userInfo()**

  > 返回用户信息

  ```js
  { uid: -1,
    gid: -1,
    username: 'Administrator',
    homedir: 'C:\\Users\\Administrator',
    shell: null 
  }
  ```

* **os.hostname()**

  > 返回计算机主机名 例如 ipanel-pc

* **os.homedir()**

  > 返回当前用户home目录路径 例如 'C:\\Users\\Administrator'

* **os.freemem()**

  > 返回空闲系统内存字节数 例如 4228706304

* **os.totalmem()**

  > 返回系统的总内存数

* **os.networkInterfaces()**

  > 返回网络信息对象 ，如下

  ```JS
  { '本地连接 3':
     [ { address: 'fe80::94e1:8930:9f9c:b6c1',
         netmask: 'ffff:ffff:ffff:ffff::',
         family: 'IPv6',
         mac: '6c:4b:90:0d:74:d3',
        ...
  }
  ```


#### cluster

> 集群

> http://nodejs.cn/api/cluster.html

* **cluster.isMaster**

  > 一个node程序只有一个主进程, 此方法返回当前程序是否在主进程

* **cluster.fork()**

  > 只在**主进程**中可用, 复制出一个工作进程, 工作进程可以共享任意tcp连接

* **cluster.workers**

  > 返回所有进程的对象, 以进程id为键, 的哈希表

* **cluster.woker**

  > 当前工作对象的引用

* **cluster.on('message', function (worker, message, handle) {})**

  > 当**主进程**收到工作进程的任意消息时触发

* **process.on('message', function () {})**

  > 当**工作进程**收到主进程的消息时触发

* **worker.send(message, [handle])**

  > 发送一个消息给**工作进程或主进程**，也可以附带发送一个handle

* **process.send({msg: 'hello'})**

  > 从工作进程中发送消息给主进程

* **cluster.js demo**

  ```js
  const cluster = require('cluster')
  const http = require('http')
  
  let cpuNum = require('os').cpus().length
  let workers = []
  if (cluster.isMaster) {
      for(let i = 0; i < cpuNum; i ++) {
          workers.push(cluster.fork())
          // 向工作进程发送消息
          workers[i].send('hello child cluster')
      }
      cluster.on('exit', worker => {
          console.log(`${worker.process.pid} 工作进程已退出`)
      })
      // 监听工作进程的消息事件
      cluster.on('message', function (worker, msg) {
          console.log(`from worker message is${msg}`)
      })
  } else {
      // 监听来自主进程的消息事件
      process.on('message', msg => {
          console.log(`receive ${msg}`)
      })
      
      http.createServer((req, res) => {
          res.end('hello world')
          // 向主进程发送消息
          process.send('the same to next')
          cluster.worker.send('i am a server')
      }).listen(3000) // 共享http端口
  }
  ```

#### stream

> 流， 分为可读，可写， 可读写 都是EventEmitter的实例

> http://nodejs.cn/api/stream.html

* **事件**

  * **data** - 当有数据可读时触发。

  - **end** - 没有更多的数据可读时触发。
  - **error** - 在接收和写入过程中发生错误时触发。
  - **finish** - 所有数据已被写入到底层系统时触发。

* **stream.Writable(obj)**

  > 创建可写流 , obj 必须要有_write方法

  ```js
  const { Writable } = require('stream')
  const writable = new Writable({
      _write(chunk, encoding, callback) {
          // do something
          callback()
      } 
  })
  ```




#### buffer

>node二进制数据

* **buf.toString(encoding)**

  ```js
  buf.toString('utf8')
  ```




#### process

> process 对象是全局对象， 不需要手动require引入

> http://nodejs.cn/api/process.html

* **process.env**

  > 系统环境变量

  ```shell
  # 运行程序时设置环境变量 
  # windows 
  set NODE_ENV=test node test.js  
  # uinx
  NODE_ENV=test node test.js
  # 跨平台
  npm i cross-env -g             
  cross-env NODE_ENV=test node test.js
  
  # node 程序中设置
  process.env.NODE_ENV = 'test'
  ```

* **process.exit(code)**

  > 指定当前进程立即退出

  ```js
  // 以 success 方式退出node进程
  process.exit(0)
  
  // 以 fail 方式退出node进程
  process.exit(1)
  ```

* **process.argv**

  > 运行node程序的参数数组

  ```shell
  node one.js hello world 
  # process.argv => ['/usr/bin/node', '/home/code/one.js', 'hello', 'world']
  ```

* **process.cwd()**

  > process.cwd() 代表 **node 进程**当前工作的目录,  **__dirname**只返回当前文件的路径

  ```js
  // /usr/lib/other.js
  console.log(`process.cwd is ${process.cwd()}`) // /usr/lib
  console.log(`__dirname is ${__dirname}`)       // /usr/lib
  
  // /usr/main.js
  require('./lib/other')
  // /usr
  // /usr/lib
  ```

* **process.nextTick(cb)**

  > 一旦当前事件轮询队列的任务全部完成 , 所有cb就会依次调用

* **process.stdout**

  > 输出流， 是一个可写流， console.log 也属于process.stdout

  * **process.stdout.write()**

    ```js
    // 把用户输入输出到终端
    process.stdin.pipe(process.stdout)
    ```

* **process.stdin**

  > 输入流， 可读流

  ```js
  process.stdin.setEncoding('utf8')
  process.stdin.on('readable', () => {
      let chunk = process.stdin.read()
      process.stdout.write(`receive data is ${chunk}`)
  })
  process.stdin.on('end', () => {
      process.stdout.write('end')
  })
  ```

* **process.on('exit', cb)**

  > 进程退出事件

  ```js
  process.on('exit', code => {
      console.log(`退出码${code}`)
  })
  ```

* **process.kill(pid[,signal])**

  > 结束进程

  ```js
  process.kill(123)
  ```


#### child_process

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



#### path

```js
//内置路径处理模块
var path = require('path')  

//return 'currPath/www'
path.join(__dirname,'www/')  

//把路径解析为绝对路径的函数 返回 
path.resolve('/usr','./local','bin')   

// Returns: '/foo/bar/baz/asdf'
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..');


// Returns:
// { root: 'C:\\',//   dir: 'C:\\path\\dir',
//   base: 'file.txt',//   ext: '.txt',//   name: 'file' }
path.parse('C:\\path\\dir\\file.txt');
┌─────────────────────┬────────────┐
│          dir        │    base    │
├──────┬              ├──────┬─────┤
│ root │              │ name │ ext │"  /    home/user/dir / file  .txt "
└──────┴──────────────┴──────┴─────┘
// The path.resolve() method resolves a sequence of paths or path 
// segments into an absolute path.

// Returns: '/foo/bar/baz'
path.resolve('/foo/bar', './baz');

// Returns: '/tmp/file'
path.resolve('/foo/bar', '/tmp/file/');

// if the current working directory is /home/myself/node,
// this returns '/home/myself/node/wwwroot/static_files/gif/image.gif'
path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif');
```



#### file-system

* **File system flags**

  | Flag | detail                                                 |
  | ---- | ------------------------------------------------------ |
  | a    | 打开文件进行追加。如果文件不存在，则创建该文件         |
  | a+   | 打开文件读取并追加。如果文件不存在，则创建该文件       |
  | r    | 打开文件并读取。如果文件不存在，则发生异常             |
  | r+   | 打开文件读取并写入。如果不存在，则发生异常             |
  | w    | 打开文件并写入。如果文件不存在则创建，存在则覆盖       |
  | w+   | 打开文件并读取和写入。如果文件不存在则创建，存在则覆盖 |

* **fs.readDir**

  > 打开文件夹

  ```js
  // 把一个文件夹下的文件拷贝到另一文件夹
  fs.readDir(path, (err, files) => {
      files.forEach(item => {
          fs.writeFileSync(__dirname + '/copy', fs.readFileSync(__dirname + '/origin'))
      })
  })
  ```

* **fs.mkdir**

  > 新建目录

  ```js
  // 若没有重名目录 则新建目录
  fs.existsSync(logFold) || fs.mkdirSync(logFold)
  ```

* **fs.open**

  > Open file

  ```js
  const fs = require('fs')
  fs.open('test.txt', 'r+', (err, fd) => {
      if (err) throw err
      console.log(fd)
  })
  ```

* **fs.writeFile**

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

* **fs.readFile**

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

* **fs.close**

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

* **fs.watch**

  > 监听文件/目录变化

  ```js
  // 监听filePath文件变化， 一旦改变，则运行回调函数
  const options = {
  	recursive: true, // 如果是目录, 是否递归监听子目录, 默认false
      encoding: 'utf8' // 监听文件名的字符编码
  }
  
  fs.watch('filePath', options,  (eventType, filename) => {
      console.log(`current file state is ${curr.sate}`)
      cp.exec('mv index.js index-dep.js')
  })
  ```

* **fs.createWriteStream**

  > 创建可写流

  ```js
  let writableStream = fs.createWriteStream(path.join(__dirname, '../readme.md'))
  let chunk = 'good good study day day up'
  // 向可写流中写入数据
  writableStream.write(chunk)
  ```


#### http

* **server.js**

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

* **http method&properties**

  * **http.get(options, callback)**

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

  * **http.request(options,[callback])**

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

* **response  methods&properties**                          

  * res.**setEncoding**('utf-8')                                      设置编码格式
  * res.**setHeader**('Content-type', 'text/html')      设置响应头
  * res.**headers**['content-type']                               获取content-type响应头
  * res.**resume**()                                                        消耗res, 释放内存空间

  * res.**writeHead**(statusCode, [headers])     设置响应头和响应码，会和setHeader合并，setHeader优先级高

  ```js
  res.writeHead(200, {
      'Content-Type': 'text/plain', 
      'Content-Length': Buffer.length(body)
  })
  ```

* **request methods&properties**

  * req.**headers**

  ```js
  { 'host': 'localhost:3001',
    'connection': 'keep-alive',
    'cache-control': 'max-age=0',
    'upgrade-insecure-requests': '1',
    'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_4) ...',
    'accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,',
    'accept-encoding': 'gzip, deflate, br',
    'accept-language': 'zh-CN,zh;q=0.9,en;q=0.8,zh-HK;q=0.7',
     'cookie': 'name=float'          // cookie会随请求头一起发送至服务器
  }
  ```


#### dns

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



## Async operator control

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



## Package.json

- **dependency**

  > 主版本号.次版本号.修正号

  > - 主版本号：当你做了不兼容的API 修改，
  > - 次版本号：当你做了向下兼容的功能性新增，
  > - 修订号：当你做了向下兼容的问题修正

  | 表达式                                | 版本范围             | 说明                                                         |
  | ------------------------------------- | -------------------- | ------------------------------------------------------------ |
  | 1.2.1                                 | 1.2.1                | 匹配指定版本，这里是匹配1.2.1。                              |
  | ^1.0.0                                | >=1.0.0 且 <2.0.0    | `^`表示与指定的版本兼容，左边第一个非0字段不可变，后面的可变 |
  | ^5.x                                  | >=5.0.0 且 <6.0.0    | 同上                                                         |
  | ~0.1.1                                | >=0.1.1 且 <0.2.0    | `~`表示约等于版本，如果存在次版本号，则允许修订号为最高的，否则允许次版本为最高，如 ~1匹配>=1.0.0 且 <2.0.0 |
  | *                                     | 匹配 >=0.0.0         | 通配符                                                       |
  | >=3.0.0                               | >=3.0.0              | 其他符号还有<,<=,>,>=,=.字面意思。可使用空格表示AND，双竖线表示OR. |
  | 1.30.2 - 2.30.2                       | >=1.30.2 且 <=2.30.2 |                                                              |
  | git://github.com/user/some.git#commit | Git URL形式的依赖    | 还支持URL、GitHub URL、本地 URL                              |
  | latest                                | 当前发布的版本       |                                                              |

  ```json
  {
      "dependencies": {
          "compression": "^1.2.3",
          "axios": "~1.2.3"
      }
  }
  ```



