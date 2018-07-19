**package.json 选项**

```json
“scripts”: {
  “prebuild”:….,   // npm run build 前执行
   “build”:….,
   “postbuild”:….  // npm run build 后执行
}
```

**npm script**

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



1.**moment** 时间格式化库

**demo**：

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



2.**rollup**   一个能把片段js代码整合成单个js文件的库,[地址](https://github.com/rollup/rollup#quick-start-guide)

**demo**: 

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