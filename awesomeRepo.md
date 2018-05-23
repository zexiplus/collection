### Awesome Repository

1.**rollup**   一个能把片段js代码整合成单个js文件的库,[地址](https://github.com/rollup/rollup#quick-start-guide)

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

