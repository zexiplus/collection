# requirejs

> RequireJS是一个工具库，主要用于客户端的模块管理。它可以让客户端的代码分成一个个模块，实现异步或动态加载



### 基本使用

* 加载使用

  > data-main 属性不可忽虑, 指定主代码所属的脚本文件

  ```html
  <head>
      <script src="js/require.js" data-main="js/entry.js"></script>
  </head>
  ```

* 配置项 **config**

  * **paths**

    > 用于指定模块的位置, 属性值可以是字符串/数组, 若是数组, 从第一个地址加载, 加载失败则用之后的路径加载

    ```js
    // paths 指定模块的链接地址
    require.config({
        paths: {
            'vue': './lib/vue.min',
            'vueRouter': [
                'www.cdnjs.com/js/vueRouter.js',
                './lib/vue-router.min'
            ]
        }
    })
    ```

    

  * **baseUrl**

    > 用于指定本地模块的基准目录, 该属性通常由require.js加载时的data-main属性指定

    ```js
    require.config({
        paths: ...,
        baseUrl: 'js/entry.js'
    })
    ```

    

  * **shim**

    > 有些库不是amd规范库, 需要该属性帮助加载非amd兼容的库

    ```js
    require.config({
        paths: {
            'underscore': 'lib/underscore',
        },
        shim: {
            'underscore': [
                exports: '_'
            ]
        }
    })
    ```

    

  

* 定义模块 **define**

  > entry.js 

  ```js
  // 定义命名模块
  define('myModule', function () {
      return {}
  })
  
  // 定义无依赖模块, define 接受的函数只能是对象或者函数
  define({
      method1: function () {},
      method2: function () {}
  })
  
  define(function () {
      var module = []
      return module
  })
  
  
  // 定义有依赖的模块
  define(['js/module1', 'js/module2'], function (module1, module2) {
      // 使用module1, module2
      module1.method1();
  })
  
  // 定义多个模块, 避免数组太长,参数太长书写不便
  define(function (require) {
      var module1 = require('js/module1');
      var module2 = require('js/module2');
      var moduel3 = require('js/module3');
  })
  ```




* 调用模块 **require**

  ```js
  require(['js/module1', 'js/module2'], function (module1, module2) {
      module1.methods();
      module2.methods();
  })
  ```



### 例子

* **vue & vueRouter & element with requirejs**

  > Index.html

  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Page Title</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <link href="css/elementUi.css" />
      <script data-main="entry.js" src="lib/require.js"></script>
  </head>
  <body>
      <div id="app">
          <el-button>test</el-button>
      </div>
  </body>
  </html>
  ```

  

  > entry.js

  ```js
  require.config({
      paths: {
          'vue': 'lib/vue.min',
          'vueRouter': 'lib/vue-router.min',
          'ELEMENT': 'lib/elementUI.js',
      },
      baseUrl: 'lib'
  })
  
  define('entry', function (require, exports, module) {
      var ELEMENT = require('ELEMENT');
      var Vue = require('vue');
      var router = require('script/router')
      Vue.use(ELEMENT);
      new Vue({
          el: '#app',
          router: router;
          data: function () {
              return {
                  text: 'hello world'
              }
          }
      })
  })
  ```

  > router.js

  ```js
  define('router', function (require, exports, module) {
      var Vue = require('vue');
      var VueRouter = require('vueRouter');
      var component1 = require('component1');
      
      Vue.use(VueRouter)
      return new VueRouter({
          routes: [
              {
                  path: '/',
                  component: component1,
              }
          ]
      })
  })
  ```

  

