# javascript

> javascript 常用知识点总结



### catalague



- [javascript](#javascript)
        - [catalague](#catalague)
        - [Skill](#skill)
        - [Class](#class)
        - [ES6](#es6)
        - [new webApi](#new-webapi)
        - [others](#others)



### Skill

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


### Class

* **Array** 

  ```js
  //合并复制数组
  var a = [{today: '1',lunch: 'food'}]
  var b = [{tomorrow: '2'}]
  var c = [...a,...b] 
  
  // 把Array转化为iterator对象
  arr[Symbol.iterator]()
  
  //类数组转化为数组 Array.from 方法
  const foo = document.querySelectorAll('.foo');
  const nodes = Array.from(foo);
  
  // Array.of 将一组值转换为数组
  Array.of(1,2,3) // [1,2,3]
  Array.of(3) // [3]
  Array(1,2,3) // [1,2,3]
  Array(3) // [,,,] 
  
  // 数组查询 find() 找到并返回第一个为true的元素
  Array.prototype.find((n) => n > 10)
  
  // 数组排序
  arr.sort(function(prev, next) {
    return prev - next
  })
  
  // 数组去重
  […new Set(arr)]				//es6 Set 不允许有重复值
  Array.from(new Set(arr))  
  // 双重循环
  // 先排序，再循环
  
  // 数组的解构赋值
  let [a, b, c] = [1, 2, 3]
  a // 1
  b // 2
  c // 3
  
  // iterator 接口解构赋值
  function* itera() {
    let a = 0,
        b = 1;
    while (a < 1000) {
      yield a;
      [a, b] = [b, a + b];
    }
  }
  let [a, b, c, d, e, f] = itera();
  
  // 交换变量值
  let x = 1,
      y = 2;
  [x, y] = [y, x]
  
  // 遍历数组键值对
  for (let [index, val] of arr.entries()) {
      console.log(index, val)
  }
  ```


* **Object**

  ```js
  // 对象解构赋值
  let {a, b, c} = {a: 1, b: 2, c: 3}
  a // 1
  
  // 对象结构并重命名
  let {a: animal} = {a: 1, b: 2, c: 3}
  animal // 1
  
  // 函数参数默认值
  let ajax = function(url, option = { crossDomain : false, transformJson : true}) {
    console.log(option.crossDomain,option.transformJson)
  }
  let ajax2 = function(url, { crossDomain = false, transformJson = true}) {
    console.log(crossDomain, transformJson)
  }
  
  // 删除Object.assign 复制的原对象属性，会改变复制后的对象
  let obj = {a: 1, b: 2, c: 3}
  let extendObj = Object.assign(obj, {d: 4}) // {a: 1, b: 2, c: 3, d: 4}
  delete obj.a
  extendObj // {b: 2, c: 3, d: 4}
  
  delete extendObj.b
  obj // {c: 3, d: 4}
  ```


* **JSON**

  ```js
  // obj 为需要格式的对象， filter为函数或数组， space为空格缩进
  JSON.stringify(obj, filter, space)
  
  // demo
  let obj = {a: 1, b: 2, c: 3}
  let str1 = JSON.stringify(obj, ['a', 'b'], 4)
  /* 
  	str => "{
      	"a": 1,
      	"b": 2
  	}"
  */
  
  // 函数过滤器, 
  let str2 = JSON.stringify(obj, function (key, val) {
      switch (key) {
          case 'a':
              return '123';
          case 'b':
              return '456';
          case 'c':
              return undefined; // 返回undefined去除c属性和值
          default:
              return val
      }
  })
  
  /* 注意 函数遍历器接收的第一个key是空字符串, val是传入的obj这个对象. 第二次key才是属性a
  ```

* **Number**

  ```js
  // 固定小数位数
  6.12333.toFixed(2) // return 6.12
  
  // 字符串转数字（'5%' -> 5）
  parseInt(num, 10)
  Number('10%') // return NaN
  ```


* **String**


  * **match方法**

    > 把字符串中匹配模式的的字符串组成数组并返回 相当于 regexp的exec方法

    ```js
    let arr = 'dog yellow green bat cat dat'.match(/.at/)
    // arr => ['bat', 'cat', 'dat']
    ```


  ```js
  // 返回是否包含字符串
  String.prototype.includes(searchString[, position])
  
  // 是否在开头或者结尾
  'hello world'.startsWith('hel') // true
  'hello world'.endsWith('orld') // true
  
  // 截除首字符
  str.substring(1)
  
  // 字符串遍历
  for (let letter of 'abc') {
    console.log(letter)
  }
  // 'a' 'b' 'c'
  ```

* **Function**

  ```js
  // 函数默认值与解构赋值 es6函数默认值是定义函数时指定的，不能再函数运行时指定
  function demo({x = 666, y} = {}) {
    return console.log(console.log(x, y));
  }
  demo({}) == demo() // 666, undefined
  demo({x: 7, y: 8}) // 7, 8
  
  // 传undefined 会触发默认效果， null则不会
  demo({x: undefined, y: 8}) // 666, 8
  demo({x: null, y: 8}) // null, 8
  
  //rest 剩余参数
  function add(...values) {
    let sum = 0;
    for (var val of values) { //
      sum += val;
    }
    return sum;
  }
  
  add(2, 5, 3) // 10
  add(...[2, 5, 3]) // 10
  
  // generator 生成器函数
  function* sayHello() {
    yield 'hello1';
    yield 'hello2';
    return 'end'
  }
  var helloGenerator = sayHello();
  helloGenerator.next() // returns {value: 'hello1', done: false};
  helloGenerator.next() // returns {value: 'hello2', done: false};
  helloGenerator.next() // returns {value: 'end', done: true};
  
  //Iterator 迭代器
  var person = {name: 'xiaoxixi', age: 24};
  var iterator = new Iterator(person);
  var arr = [1,2,3];
  iterator.next() // returns ['name', 'xiaoxixi'];
  
  // trampoline 蹦床函数（把递归函数转化为循环的函数，防止调用栈过多造成 maximum call exceed 错误）
  const trampoline = (f) => {
      while (f && f instanceof Function) {
          f = f()
      }
      return f
  }
  ```



### ES6

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


### new webApi



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



### others

* **module**

  ```js
  /* 同一个js文件可以同时export {} 和 export default {}，但不建议
    如果模块只有一个输出值，就使用export default，如果模块有多个输出值，就使用export，export default与   普通的export不要同时使用
    如果模块默认输出一个函数，函数名的首字母应该小写。
    如果模块默认输出一个对象，对象名的首字母应该大写。
  */
  
  // commonJs模块加载
  let { stat, exists, readFile } = require('fs');
  // 等同于
  let _fs = require('fs');
  let stat = _fs.stat;
  let exists = _fs.exists;
  let readfile = _fs.readfile;
  
  //上面代码的实质是整体加载fs模块（即加载fs的所有方法），生成一个对象（_fs），然后再从这个对象上面读取 3 个方法，这种加载称为“运行时加载”，导致完全没办法在编译时做“静态优化”
  
  //es6模块加载
  import { stat, exists, readFile } from 'fs';
  //上面代码的实质是从fs模块加载 3 个方法，其他方法不加载
  
  //需要特别注意的是，export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。
  
  // 报错
  export 1;
  
  // 报错
  var m = 1;
  export m;
  // 报错
  function f() {}
  export f;
  
  // 正确
  export function f() {};
  
  // 正确
  function f() {}
  export {f};
  
  // 写法一
  export var m = 1;
  
  // 写法二
  var m = 1;
  export {m};
  
  // 写法三
  var n = 1;
  export {n as m};
  
  // 另外，export语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。
  //import命令输入的变量都是只读的，因为它的本质是输入接口。也就是说，不允许在加载模块的脚本里面，改写接口。
  import {a} from './xxx.js'
  a = {}; // Syntax Error : 'a' is read-only;
  
  //import命令具有提升效果，会提升到整个模块的头部，首先执行
  foo();
  import { foo } from 'my_module';
  ```


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