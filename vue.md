# vue

vue 使用指南

[TOC]



### vue remind

* **使用html字符串**

  ```html
  <script>
      new Vue({
          el: '#app',
          data() {
              return {
                  htmlTemplate: '<p>这里是一段html</p>'
              }
          }
      })
  </script>
  <div v-html="htmlTemplate">
      
  </div>
  ```


**vue cli**

```js
vue list // 列出所有可用脚手架
vue init webpack demo // 创建以webpack为脚手架名为demo的项目
```



**main.js**

```js
/* ----------- main.js  ------------------ */
import App from './App'
new Vue({
  el: '#app',
  router,
  store,
  components: { App },
  template: '<App/>'
})


// 与上面写法等效 (使用render函数)
new Vue({
  el: '#app',
  router,
  store,
  render: h => h(App)
})


```



**Vue config**

```bash
# 在vue中使用less于编译器
npm i less less-loader -S
# webpack.base.config.js
modules.exports.module.rules: [{test: /\.less$/, loader: 'style-loader!css-loader!less-loader'}]
```



**vue plugins**

> Vue.js 的插件应当有一个公开方法 `install` 。这个方法的第一个参数是 `Vue` 构造器，第二个参数是一个可选的选项对象：

```js
MyPlugin.install = function (Vue, options) {
  // 1. 添加全局方法或属性
  Vue.myGlobalMethod = function () {
    // 逻辑...
  }

  // 2. 添加全局资源
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 逻辑...
    }
    ...
  })

  // 3. 注入组件
  Vue.mixin({
    created: function () {
      // 逻辑...
    }
    ...
  })

  // 4. 添加实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // 逻辑...
  }
}
```



**vue directives**

> 注册指令

```js
//全局注册
Vue.directive('directiveName',{
  	bind: function (el, binding) {
    	//binding.value 指绑定的值
  	},
  	inserted: function () {},
  	update: function () {},
  	componentUpdated: function () {},
 	unbind: function () {}
});
//组件内注册
export default {
  directives: {
    directiveName: {
      bind() {} .....//一系列钩子函数
    }
  }
}

```



**vue style**

```html
<!-- 样式引入-->
<style lang="less" scoped>
	@import './demo.less'
</style>

<div :class="{classOne: true, classTwo: true}"
     :style="{color: 'red', fontSize: '12px', 'background-color': 'red'}">
</div>

```

```js
/* --------------header 高度为100,动态设置main容器高度 --------- */
let timer;
export default {
  data() {
    return { mainHeight: 0 }
  },
  created() {
    window.addEventListener('resize', () => {
      if (timer) {
        cleartTimeout(timer)
      }
      else {
        timer = setTimeout(() => {
       		this.mainHeight = window.innerHeight - 100;   
        })
      }
    })
  },
}
```



**vue load on demand**

```js
/* ---------------异步引入模块 ------------ */
// 写法一
const component = () => import('componentName')  

// 写法二
const component = resolve => require(['componentName'],resolve)
```



**vue render（createElement function h）**

```html
<custom-component>
  <p slot="header">
    这里是头部内容
  </p>
  <p>
    这里是默认slot内容
  </p>
  <p slot="footer">
    这里是底部内容
  </p>
</custom-component>
```

```js
// render 函数
Vue.component('customComponent', {
  render(h) {
    let hearder = this.$slot.header;
    let main = this.$slot.default;
    let footer = this.$slot.footer;
    return h('div', [
      h('header', header),
      h('main', main),
      h('footer', footer),
    ])
  }
})
```



**vue render（with jsx）**

- {value} 单花括号变量名

```jsx
new Vue({
  el: '#demo',
  props: ['name','imgSrc'],
  methods: {
    handleImgClick() {
      console.log('awesome picture')
    }
  },
  render(h) {  
    return (
    	<div 
          level={1} 
          name={this.name} 
          class={{ foo: true, bar: false }}
      	  style={{ color: 'red', fontSize: '14px' }}>
        	<img src={this.imgSrc} onClick={this.handleImgClick}/>
      	</div>
    )
  }
})
```



**Vue options**

```js
// watch 深度观察, *注: 观察数组时不需要deep，但是arr[1] = 1,赋值操作不会触发观察,方法操作才会触发例如arr.splice(0, 1, 1)
watch: {
  c: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true
    }
}
```



**service**  

>  分离请求(太多ajax太丑陋，封装成单个js暴露出去，每个方法均返回promise实例)

```js
// index.js
import Vue from 'vue'
import VueResource from 'vue-resource'
Vue.use(VueResource)

// 小 service模块
import login from './login' 
import cart from '.cart'

export default {
  login,
  cart,
}

// cart.js
import Vue from 'vue'
export default {
  getProductsById(id) {
    return Vue.http.get('url', {params: {id}})
  }
}
```



**vue others**

```js
// Vue.extend(component) 创建并返回一个子类，可用于构造新组件用于测试
import app from './app'
const App = Vue.extend(app)
new App().$mount('#id')  // 创建并挂载组件
const vm = new App().$mount() // 创建组件并挂载（也可以不挂载）
expect(vm.$el.querySelector('h1').textContent).toEqual('title') // 判断组件内部选择器h1内容

// 路由跳转
this.$router.push({name: 'pathName'})

/* --------------------返回当前route路径 ------------------ */
computed() { 
  return this.$route.path
}


/* ----------单独引用element 组件使用方法------- */
import { MessageBox } from 'element-ui'
//单独调用
MessageBox.alert(msg, title, {type:'error'});
//挂载后调用
this.$alert(msg, title, {type:'error'});


/*---------------动态组建 (可用于一个页面有多个弹窗)----------------------*/
v-bind:is=”componentName”
<component :is=”currentView”></component>

```





