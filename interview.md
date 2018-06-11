## interview



### keep in mind

- mistake can clear your eyes
- convert failure to expreience
- Fun & Enjoy

###awesome interview collection

- https://blog.csdn.net/kongjiea/article/details/46341575

- https://segmentfault.com/a/1190000000465431

- http://www.cnblogs.com/syfwhu/p/4434132.html

### Terminology

**1.xss 和 csrf区别**

- csrf (Cross-site request forgery) 

  跨站请求伪造， 攻击者盗用了你的身份，以你的名义发送恶意请求 。

  条件

  1.登录受信任网站A，并在本地生成Cookie。

  2.在不登出A的情况下，访问危险网站B。

  ![csfr](./imgs/csrf.jpg)

- xss （Cross-site scripting）

  分类

  1.持久性（永久上传某段代码，之后的所有用户都会被攻击）

  2.非持久性（让某个用户点击恶意代码片段连接，只对这个用户攻击）

  恶意代码注入，将恶意代码片段通过任何途径上传到服务器端，当下次用户进行访问时就会执行恶意代码

   防御

  1.过滤用户输入，谨慎存取
  2.对用户输入进行转码

  
**2.http 和 https 的区别**

http默认端口80，https默认端口443

https采用ssl加密，http无加密

https的web服务器启用ssl需要获得一个服务器证书，并将该证书与要使用ssl的服务器绑定



**3.懒加载（load on demand）**

懒加载或者按需加载，是一种很好的优化网页或应用的方式。这种方式实际上是先把你的代码在一些逻辑断点处分离开，然后在一些代码块中完成某些操作后，立即引用或即将引用另外一些新的代码块

```js
// 1.webpack 使用promise和async函数实现 懒加载

// 2.vue 使用 import 函数
Vue.component('AsyncCmp', () => import('./AsyncCmp'))

// 3.react 与 vue 类似
const LoadableComponent = Loadable({
  loader: () => import('./Dashboard'),
  loading: Loading,
})

export default class LoadableDashboard extends React.Component {
  render() {
    return <LoadableComponent />;
  }
}

// 图片懒加载 <img> 标签不要设置src属性，放在自定义属性中，根据window.scrollTop判断图片是否出现在用户视野中，如果出现 将自定义属性中的 url 放入src属性中
```





### advanced question

1. 分析双向数据绑定的原理，并用简单的代码实现

   ```js
   
   function Vue(options) {
     this.$init(options);
   }
   Vue.prototype.$init = function(options = {}) {
     let data = this._data = options.data || {}
     let self = this
     let dep = new Dep()
     // 属性代理 vm.a === vm.data.a
     Object.keys(data).forEach((key, index) => {
       Object.defineProperty(self, key, {
         configurable: true,
         enumerable: true,
         get(){
           return data[key]
           dep.depend()
         },
         set(val) {
           data[key] = val
           dep.notice() 
         }
       })
     })
   }
   
   function Dep() {
     // 
   }
   Dep.prototype.notice = function() {
     // 触发页面更新
   }
   Dep.prototype.depend = function() {
     // 关联当前所有数据和watcher的关系
   }
   
   let vm = new Vue({data: {a: 1, b: 2}})
   
   
   console.log(vm.a === vm._data.a) // 实例访问数据
   vm.a = 3
   console.log(vm._data.a)
   ```

   

2. 手动封装一个promise库，能实现基本的promise api。

3. js实现快速排序（前端常见算法举例说明）

4. 实现深度拷贝

   ```js
   function clone(obj) {
       if (obj instanceof Array) {
           let ret = []
           obj.forEach((item, index) => {
               ret[index] = clone(item)
           })
           return ret
       } else if (obj intanceof Object) {
           let ret = {}
           Object.keys(ret).forEach((item, index) => {
               ret[item] = clone(obj[item])
           })
           return ret
       } else {
           return obj
       }
   }
   ```

   