# Javascript

> javascript 常用知识点总结



## Catalogue

[TOC]

## Skill

* **求值表达式 (0, vald)**

  ```js
  let obj = {a: 1, b: function() {console.log(this.a)}}
  obj.b() // returns 1
  (0, obj.b)() // returns undefined
  (obj.b)() // returns 1
  ```

* **返回值函数 bind**

  ```js
  // bad 
  return function() {
    return sum(5, 1)
  }
  // good
  return sum.bind(5, 1)
  ```

* **arguments 与箭头函数**

  ```js
  // arguments 不可以在箭头函数中使用，不存在
  (...rest) => {
      console.log(arguments) // undefined
      console.log(rest)      // 使用rest数组代替arguments
  }
  ```

* **Object.isExtensible() & Object.preventExtensions()**

  ```js
  let a = {}
  Object.isExtensible(a) // expected : true
  Object.preventExtensions(a)
  Object.isExtensible(a) // expected: false
  ```

* **Object.getOwnPropertyDescriptor(obj, prop)**

  ```js
  let obj = {get a() { return 1 }}
  Object.getOwnPropertyDescriptor(obj, 'a')
  // return {get: ƒ, set: undefined, enumerable: true, configurable: true}
  ```

* **蹦床函数**

  > trampoline 蹦床函数（把递归函数转化为循环的函数，防止调用栈过多造成 maximum call exceed 错误）

  ```js
  const trampoline = (f) => {
      while (f && f instanceof Function) {
          f = f()
      }
      return f
  }
  ```


## Class



### Number

* **toFixed**

  > 固定小数位数

  ```js
  6.12333.toFixed(2) // 6.12
  ```

* **parseInt, Number**

  > 转化字符串为数字

  ```js
  let num = '123%'
  parseInt(num, 10) // 123
  Number(num)       // NaN
  ```


### String

- **match方法**

  > 把字符串中匹配模式的的字符串组成数组并返回 相当于 regexp的exec方法

  ```js
  let arr = 'dog yellow green bat cat dat'.match(/.at/)
  // arr => ['bat', 'cat', 'dat']
  ```

- **遍历字符串**

  ```js
  for (let letter of 'abc') {
      console.log(letter)
  }
  // 'a' 'b' 'c'
  ```

- **includes 方法**

  > 判断给定字符串是否包含字符

  ```js
  String.prototype.includes(searchString[, position])
  // true or false
  ```

- **substring 方法**

  > 截取字符串

  ```js
  // 截除首字符
  str.substring(1)
  ```

- **startsWith, endWith**

  > 判断字符串开头, 结尾

  ```js
  'hello world'.startsWith('hel') // true
  'hello world'.endsWith('orld') // true
  ```



### Array

* **Array.from**

  > 把类数组转化为数组

  ```js
  const foo = document.querySelectorAll('.foo');
  const nodes = Array.from(foo);
  
  // 去重
  Array.from(new Set(array))
  [... new Set(array)]
  ```

* **…** 

  > 合并数组

  ```js
  var a = [1,3,3]
  var b = [4]
  var c = [...a,...b] 
  ```

* **变量交换**

  ```js
  let a = 1, b = 2;
  [a, b] = [b, a]
  ```

* **find**

  > 找到并返回第一个true的值

  ```js
  [1,2,3,4].find(item => item > 2)
  // 3
  ```


### Object

* **{a}**

  > 解构赋值

  ```js
  let {a, b, c} = {a: 1, b: 2, c: 3}
  console.log(a)      // 1
  
  let {a: animal} = {a: 1, b: 2, c: 3}
  console.log(animal) // 1
  ```

* **Object.assign**

  > 对象浅拷贝, 只修改了对象的原型链, 并没有真正复制属性值

  ```js
  let obj = {a: 1, b: 2, c: 3}
  let extendObj = Object.assign(obj, {d: 4}) // {a: 1, b: 2, c: 3, d: 4}
  delete obj.a
  extendObj // {b: 2, c: 3, d: 4}
  ```



### Function

* **rest**

  > 剩余参数

  ```js
  function add(...rest) {
    let sum = 0;
    for (var val of rest) {
      sum += val;
    }
    return sum;
  }
  
  add(2, 5, 3) // 10
  ```

* **解构赋值&默认参数**

  ```js
  function demo({x = 666, y} = {}) {
    return console.log(console.log(x, y));
  }
  
  // 传undefined 会触发默认效果， null则不会
  demo({}) == demo() // 666, undefined
  demo({x: 7, y: 8}) // 7, 8
  demo({x: undefined, y: 8}) // 666, 8
  demo({x: null, y: 8}) // null, 8
  
  let ajax = function(url, option = { crossDomain : false, transformJson : true}) {
    console.log(option.crossDomain,option.transformJson)
  }
  let ajax2 = function(url, { crossDomain = false, transformJson = true}) {
    console.log(crossDomain, transformJson)
  }
  ```

* **generator**

  > 生成器函数

  ```js
  function* sayHello() {
    yield 'hello1';
    yield 'hello2';
    return 'end'
  }
  var helloGenerator = sayHello();
  helloGenerator.next() // returns {value: 'hello1', done: false};
  helloGenerator.next() // returns {value: 'hello2', done: false};
  helloGenerator.next() // returns {value: 'end', done: true};
  ```

* **Iterator**

  > 迭代器函数

  ```js
  var person = {name: 'xiaoxixi', age: 24};
  var iterator = new Iterator(person);
  iterator.next() // returns ['name', 'xiaoxixi'];
  ```





### JSON

* **JSON.stringify(obj,  filter,  space)**

  >   obj 为需要格式的对象， filter为函数或数组， space为空格缩进

  ```js
  let obj = {a: 1, b: 2, c: 3}
  let str1 = JSON.stringify(obj, ['a', 'b'], 4)
  // str1 => {a: 1, b: 2}
  ```



## ES6

* **Decorator**

  ```js
  // 装饰器修饰类 
  @decorator
  class MyClass() {
      
  }
  function decorator(target) {
      target.hasDecorator = true;
      target.prototype._number = 0;
  }
  MyClass.hasDecorator // true
  new MyClass()._number === 0 // true
  
  // 装饰器修饰类的属性
  class MyClass2() {
      @readonly
      name() { return 'xiaoxixi' }
  }
  function readonly(target, name, descriptor) {
      descriptor.writable = false;
      return descriptor
  }
  ```



* ##### Promise

  ```js
  // 手动将promise转化为失败态 reject()
  Promise.reject("Testing static reject").then(function(reason) {
    // 未被调用
  }, function(reason) {
    console.log(reason); // "Testing static reject"
  });
  
  ```



## new webApi



* **requestAnimationFrame**

  ```js
  /************ js动画 以60hz调用函数得函数 requestAnimationFrame **********/
  requestAnimationFrame(function (timeStack) {
      // 接受一个函数作为参数，这个函数得参数为一个 date 对象，代表当前函数运行时的时间戳
  })
  somethingAnimation() {
    el.style.width = `${parseInt(el.style.width, 10) + 1}%`; // 每一帧的操作
    if(something) { // 根据判断条件调用下一帧动画
      requestAnimationFrame(somethingAnimation)
    }
  }
  ```

* **FileReader**

  ```js
  /************* 文件读取构造函数 构造函数  FileReader *************************/
  <input type="file" id="fileUp">
  document.addEventListener(document.querySelector('#fileUp'), 'change', function(e) {
    var reader = new FileReader();
    reader.readAsDataURL(e.target.files[0]);
    reader.onload = function() {
      someDom.innerHTML = `<img src="${reader.result}" />`
    }
  })
  ```

* **RAM URL**

  ```js
  /********* 对象URL  window.URL.createObjectURL() *******************/
  var objUrl = window.URL.createObjectURL(e.target.files[0]);  //指向一块内存地址
  someDom.innerHTML = `<img src="${objUrl}" />`
  
  /*********** xhr 上传文件 构造函数 FormData ******************/
  var data = new FormData();
  data.append('file',e.target.files[0]);
  xhr.send(data)
  ```

* **Web worker**

  ```js
  /*********** Web Worker 多线程对象 ***********/
  var worker = new Worker('sort.js')
  var arr = [68,66,89,32,18]
  worder.onmessage = function(event) {
    var data = event.data;
    console.log('sorted array is', data)
  }
  worker.postMessage(arr)
  
  // sort.js 文件
  self.onmessage = function(event) {
    var data = event.data;
    data.sort(function(a, b) {
      return a - b
    })
    self.postMessage(data)
  }
  ```

* **Html5 notification API**

  ```js
  var myNotify = new Notification('标题', {
      body: '这是正文内容'
  })
  
  myNotify.onclick = function () {
      console.log('消息被点击')
  }
  ```



## others


* **comment specification**

  ```js
  // 函数（sublime快捷键 /** + 回车）
  	/**
      * 数据格式化
      * @param src {Array}        长度自由的一维数组，子元素为json对象
      * @param data {Object}      参考数据
      * @ignore created           2013-10-11
      * @return result {Array}    返回格式化后与src类型相同的数组
      */
  ```