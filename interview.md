## interview



### keep in mind

- mistake can clear your eyes
- convert failure to expreience
- Fun & Enjoy

###awesome interview collection

- https://blog.csdn.net/kongjiea/article/details/46341575
- https://segmentfault.com/a/1190000000465431
- http://www.cnblogs.com/syfwhu/p/4434132.html

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

