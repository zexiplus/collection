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

   

