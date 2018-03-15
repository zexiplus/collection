# why

> /\d{6}/.test(‘440303008000’)    //true

 要只匹配6个数字  /^\d{6}$/.test('1234566565') //false 

   

> Unexpected side effect in "sortArr" computed property

```js
//vue 计算属性 ， this    es-lint报错
sortArr() {
  return this.rankData.sort((prev, next) => next.value - prev.value);
}
```

> 原因 side effect 修改了原数据（data或prop）
>
> 正确做法

```js
sortArr() {
  return this.rankData.slice(0).sort((prev, next) => next.value - prev.value);
}
```

> process变量在vue的静态环境和开发环境是否存在

```js
// 开发环境，无论是<script>标签还是mounted均存在
// 静态环境（node http-server服务器和apache服务器均可）  无论是<script>标签还是mounted均存在
```

npm install 时 报错, 解决：删除package-lock.json继续下载

```js
Unexpected token < in JSON at position 25997
```

