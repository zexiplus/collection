# node package library



### catalogue

[TOC]



### package.json scripts

```js
{
    "scripts": {
         "prebuild":….,   // npm run build 前执行
         "build":….,
         "postbuild":….  // npm run build 后执行
    },
    "bin": {
		"command": "./bin/script.js"
    }
}
```



### package structor

| Folder  | Use                |
| ------- | ------------------ |
| bin     | 命令行脚本         |
| docs    | 文档               |
| example | 例子               |
| lib     | 程序的核心功能     |
| test    | 测试脚本及相关资源 |





### npm script

* **初始化仓库**

  ```shell
  npm init
  ```

* **登录 npm 账号**

  ```shell
  npm login
  ```

* **发布包**

  ```shell
  npm publish
  ```

* **全剧链接**

  > 可以让包在机器上全局使用 bin 命令

  ```shell
  sudo npm link
  
  sudo npm unlink
  ```

  > bin 文件

  ```bash
  #!/usr/bin/env node
  console.log('hello world')
  ```

* **自定义参数**

  > 自定义参数 -- 会把之后的所有参数存入process.argv

  ```shell
  npm run command -- --someArg 
  npm run -- --test
  
  # index.js
  process.argv.includes('--test') // true
  ```

* **锁定版本**

  ```shell
  npm shrinkwrap
  ```

* **设置镜像**

  ```shell
  npm config set registry http://registry.npmjs.org
  ```




### node packages

#### moment 

> 时间格式化库

> https://github.com/moment/moment

```shell
npm i moment
```

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



#### rollup  

> 整合多个js文件到一个js文件

> https://github.com/rollup/rollup#quick-start-guide

```shell
npm i rollup
```

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



#### busboy 

> 文件上传

> https://github.com/mscdex/busboy

```shell
npm i busboy
```

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



#### opn  

> 预览文件插件 浏览器/图片

> https://github.com/sindresorhus/opn

```shell
npm i opn 
```

```js
var opn = require('opn')   
open('http://localhost:8080')

opn('unicorn.png').then(() => {
    // 预览图片
});

opn('http://sindresorhus.com', {app: 'firefox'});

```



#### supervisor

> 检测node.js代码变化， 自动重启脚本 

> https://github.com/petruisfan/node-supervisor

```shell
# download
$ sudo npm i -g supervisor

# use 
supervisor test.js
```



#### mocha 

> 用于node测试代码 

> https://mochajs.org/

```shell
# download
$ sudo npm i -g mocha

# use
mocha test.js
```

> test.js

```js
var assert = require('assert');
describe('Array', function() {
  describe('#indexOf()', function() {
    it('should return -1 when the value is not present', function() {
      assert.equal([1,2,3].indexOf(4), -1);
    });
  });
});
```



#### should.js

> BDD 风格（behavior driven development) 测试库，是assert模块的扩展

> https://github.com/shouldjs/should.js

```shell
npm i should -D
```

```js
var should = require('should')

var user = {
    name: 'tj'
  , pets: ['tobi', 'loki', 'jane', 'bandit']
};

user.should.have.property('name', 'tj');
user.should.have.property('pets').with.lengthOf(4);

// If the object was created with Object.create(null)
// then it doesn't inherit `Object.prototype`, so it will not have `.should` getter
// so you can do:
should(user).have.property('name', 'tj');

// also you can test in that way for null's
should(null).not.be.ok();

someAsyncTask(foo, function(err, result){
  should.not.exist(err);
  should.exist(result);
  result.bar.should.equal(foo);
});
```



#### uglifyjs

>  压缩混淆代码工具



#### node-notifier

> 跨平台的系统通知插件

> https://github.com/mikaelbr/node-notifier

```shell
npm install node-notifier
```

```js
const notifier = require('node-notifier')
notifier.notify('message')
notifier.nogiry({
    title: 'My title',
    message: 'hello world'
})
```



#### ora

>  界面友好的交互式输出工具 spinner

> https://github.com/sindresorhus/ora

```shell
npm i ora
const spinner = ora('Loading unicorns').start();

setTimeout(() => {
	spinner.color = 'yellow';
	spinner.text = 'Loading rainbows';
}, 1000);
```



#### ansi.js

> 彩色化命令行 文字, 前景色, 背景色 

> https://github.com/TooTallNate/ansi.js

```bash
npm install ansi
```

```js
const ansi = require('ansi'), 
      cursor = ansi(process.stdout)

// You can chain your calls forever:
cursor
    .red()                 // Set font color to red
    .bg.grey()             // Set background color to grey
    .write('Hello World!') // Write 'Hello World!' to stdout
    .bg.reset()            // Reset the bgcolor before writing the trailing \n,
//      to avoid Terminal glitches
    .write('\n')           // And a final \n to wrap things up
```



#### chalk

> 彩色化输出

> https://github.com/chalk/chalk

```shell
npm i chalk
```

```js
const chalk = require('chalk')
console.log(chalk.red('hello'))

// 自定义
const error = chalk.bold.red
const info = chalk.keyword('origin')
console.log(error('error occured'))
console.log(info('be careful origin'))
```



#### commander.js

> 创建命令行工具的库

> https://github.com/tj/commander.js

```shell
npm install commander --save
```

* **基本用法**

  ```js
  #!/usr/bin/env node
  const programe = require('commander')
  
  programe.version(require('../package.json').version)
  
  programe
  	.command('init [env]')  // 命令名称
  	.alias('i')       // 简称
  	.description('init a project')  // 描述
  	.option('-s --setup_mode [mode]', 'which setup mode to use')
  	.action((env, options) => {                 // 执行命令的回调函数
          let mode = options.setup_mode
          env = env || 'dev'
  	})
  
  programe.parse(process.argv)
  ```

* **API**

  * **version 版本**

    ```js
    programe.version('1.2.3', '-v --version')
    ```

    ```shell
    programe --version
    programe -v
    # returns 1.2.3
    ```

  * **option 选项**

    > 与参数不同的是 option 不区分顺序

    ```js
    programe
    	.option('-p --peppers', 'Add peppers')
    	.parse(process.argv)
    
    console.log(programe.peppers) // true
    ```

    ```shell
    programe -p
    # true
    ```

  * **command 命令**

    ```js
    programe
    	.command('list <num>')
        .action((num) => {
        	conosle.log(num)
    	})
    ```

    ```shell
    programe list 10
    # log 10
    ```

  * **命令必选参数 \<param>**

    ```js
    programe
    	.command('rm <dir>')
    	.option('-r --recursive')
        .action((dir, cmd) => {
            console.log(dir)
            console.log(cmd.recursive)
    	})
    ```

    ```shell
    programe rm /bin
    # /bin
    # false
    ```

  * **命令可选参数 [param]**

    ```js
    programe
    	.command('rm <dir> [otherdirs...]')
        .action((dir, otherdirs) => {
        	console.log(dir)
        	if (otherdirs) {
            	othersdirs.forEach((item) => { console.log(item) })
        	}
    	})
    ```

    * **arguments 声明参数**

      ```js
      programe
      	.arguments('<dir> [env]')
          .action((dir, env) => {
          
      	})
      ```

  * **正则参数**

    ```js
    programe
    	.option('-s --size <size>', 'dick size', /^(large|medium|small)$/i, 'medium')
        .action(cmd => {
        	console.log(`dick size is  ${cmd.size}`)
    	})
    ```

    ```shell
    programe -s 18
    # log medium
    ```

  * **usage 使用提示**

    ```js
    programe.usage('command [option] <param>')
    ```

    ```shell
    programe
    # returns Usage: programe command [option] <param>
    ```

  * **参数列表**

    ```js
    programe
    	
    ```




#### chromix-too, chromix

>控制浏览器刷新, 关闭, 重启, 不支持windows系统

> **npm** https://www.npmjs.com/package/chromix-too
>
> **chrome插件** chromix-too

```shell
# 下载
npm i -g chromix-too

# 启动服务
chromix-too-server

# 查看浏览器列表
chromix-too ls
# 输出如下
17 https://blog.zfanw.com/webpack-tutorial/ webpack 4 教程
373 https://www.npmjs.com/package/chromix-too chromix-too - npm

# focus a tab
chromix focus https://blog.zfanw.com/webpack-tutorial

# 刷新某页面
chromix reload https://blog.zfanw.com/webpack-tutorial

# 打开新的页面
chromix open https://blog.zfanw.com/webpack-tutorial

# 通过id 关闭页面
chromix rm 17 
```

```js
// module use
const chromix = require('chromix-too')().chromix
```



#### shields

> git repository 图标生成工具

> https://github.com/badges/shields

```shell
npm install -g gh-badges
badge build passed :green .png > mybadge.png
```



#### Inquirer.js

> 命令行用户交互界面库

> https://github.com/sboudrias/Inquirer.js

```shell
npm i inquirer
```

* 单个询问

  ```js
  const inquirer = require('inquirer')
  
  let prompt = inquirer.createPromptModule()
  
  let options = {
      type: 'list | rawlist | expand | checkbox | confirm | input | password | editor',
      name: 'question name',
      choices: ['1', '2', '3 hello']
  }
  
  prompt(options).then(cb)
  ```

* 多个询问

  ```js
  const inquirer = require('inquirer')
  inquirer
      .prompt([
      	option1, 
      	option2,
      	option3,
  	])
      .then(anwser => {
      	
  	})
  ```



#### prerender.io

> 服务端渲染

> https://prerender.io/
>

