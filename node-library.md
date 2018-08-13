### package.json options

```js
“scripts”: {
  “prebuild”:….,   // npm run build 前执行
   “build”:….,
   “postbuild”:….  // npm run build 后执行
}
```



### npm script

```shell
# npm 参数 --argname
npm run --test 
process.argv.includes('--test') // false . node 会把所有--参数当成node参数而不是运行参数

# 自定义参数 -- 会把之后的所有参数存入process.argv
npm run command -- --someArg 
npm run -- --test 
process.argv.includes('--test') // true

# 锁定当前npm版本
npm shrinkwrap 
```



### javascript library



#### utils

1.**moment** 时间格式化库

```js
//es6 import引入
import moment from 'moment' 
//nodejs 引入
var moment = require('moment')

//把时间对象格式化为文本 2018-01-09 12:14:27
moment(new Date).format('YYYY-MM-DD hh:mm:ss')

//把文本解析为date对象   
moment('2018-01-09 12:14:27')
```



2.**rollup**   整合多个js文件到一个js文件

> doc [link](https://github.com/rollup/rollup#quick-start-guide)

```js
const path = require('path')
const rollup = require('rollup').rollup

rollup({
	input: path.resolve(__dirname, '../src/vue.js')
}).then((bundle) => {
	bundle.write({
		file: path.resolve(__dirname, '../dist/vue.js'),
		format: 'umd',
		name: 'Vue'
	})
}).catch((e) => {
	console.error(e)
})
```



3**.busboy**  文件上传

> doc [link](https://github.com/mscdex/busboy)

> download: `npm i busboy`

```js
const Busboy = require('busboy')
const http = require('http')

http.createServer((req, res) => {
   var busboy = new Busboy({ headers: req.headers });
    busboy.on('file', function(fieldname, file, filename, encoding, mimetype) {
      var saveTo = path.join(os.tmpDir(), path.basename(fieldname));
      file.pipe(fs.createWriteStream(saveTo));
    });
    busboy.on('finish', function() {
      res.writeHead(200, { 'Connection': 'close' });
      res.end("That's all folks!");
    });
    return req.pipe(busboy);
  }
  res.writeHead(404);
  res.end();
}).listen(3000)
```



4 **.opn**    操作浏览器

> [git link](https://github.com/sindresorhus/opn)

```js
var opn = require('opn')            //自动打开浏览器函数
open('http://localhost:8080')

opn('unicorn.png').then(() => {
	// image viewer closed
});

opn('http://sindresorhus.com', {app: 'firefox'});


```



5.. **data-fns** 时间处理库 

> 

6.**weex** 用vue做native app

> [official website](http://weex.apache.org/cn/guide/)

> download `npm install weex-toolkit -g `

> 初始化项目 `weex create app`

**module**

```js
// 引入模块
weex.requireModule('stream')
```

