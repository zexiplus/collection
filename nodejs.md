# Nodejs



### Catalague





### Module

*  require过的文件会加载到缓存，所以多次 require 同一个文件（模块）不会重复加载

*  ( a->b,b->a)循环引用并不会报错，导致的结果是 require 的结果是空对象 {}，原因是 b require 了 a，a 又去 require 了 b，此时 b 还没初始化好，所以只能拿到初始值 {}

* module.exports 初始值为一个空对象 {}
* exports 是指向的 module.exports 的引用
* require() 返回的是 module.exports 而不是 exports
* process.argv    [node路径，js文件路径，命令行参数]
* process.env     系统环境对象



### Develop Cli

> 常用node开发调试 cli命令

```shell
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

# 根据进程端口查看进程信息
lsof -i :3000

# 根据pid 杀死进程
kill -9 pid
```






### Async operator control

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





### Built-in module

* **fs**

  > File system flags

  | Flag | detail                                                 |
  | ---- | ------------------------------------------------------ |
  | a    | 打开文件进行追加。如果文件不存在，则创建该文件         |
  | a+   | 打开文件读取并追加。如果文件不存在，则创建该文件       |
  | r    | 打开文件并读取。如果文件不存在，则发生异常             |
  | r+   | 打开文件读取并写入。如果不存在，则发生异常             |
  | w    | 打开文件并写入。如果文件不存在则创建，存在则覆盖       |
  | w+   | 打开文件并读取和写入。如果文件不存在则创建，存在则覆盖 |

* **fs.readDir**

  > 打开文件夹并遍历文件拷贝到另一目录

  ```js
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

  

### http
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
//内置路径处理模块
var path = require('path')  

//return 'currPath/www'
path.join(__dirname,'www/')  

//把路径解析为绝对路径的函数
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



### process

```shell
# 环境变量   系统上得键值对对象
# process.env  

DEBUG=1 node one.js   
# process.env.DEBUG == 1

# 运行node时指定得参数[node路径，文件路径，指定参数]
# process.argv
node one.js hello world 
# process.argv = [‘/usr/bin/node’,/home/code/one.js’,’hello’,’world’]

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



### Package.json

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



