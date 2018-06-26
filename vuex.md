### intro 

[doc链接](https://vuex.vuejs.org/zh/guide/state.html)

* **state** 全局单一状态树存储对象，遵循响应式原则

  ```js
  const state = {
      num: 5,
      user: {
          name: 'xiaoxixi',
          age: 21,
      },
      list: [
          {id: '1', price: 5, name: 'banana', saled: false},
          {id: '2', price: 10, name: 'watermelon', saled: true},
      ]
  }
  ```

  访问state

  ```js
  // js
  this.$store.state.user
  ```

  ```html
  <!-- template -->
  <span>{{$store.state.user}}</span>
  ```
  快速访问state (**mapState**)

  ```js
  import { mapState } from 'vuex'
  const vm = {
      computed: {
          ...mapState({
      		count: state => state.count,
     			countAlias: 'count',
    		})
      }
  }
  ```

  

* **getters** (从state派生出一些属性，比如过滤，统计)

  ```js
  const getters = {
      listNum(state, getters) { // 参数1为state对象,参数2为其他getters对象
          return state.list.length
      },
      saledLength(state) {
          return state.list.filter(item => item.saled)
      },
      findByPrice(state) { // 返回函数，之后通过函数调用
          return price => {
          	return state.list.find(item => item.price === price)
          }
      }
  }
  
  ```
  访问getters

  ```js
  this.$store.getters.listNum // 属性访问
  this.$store.getters.findByPrice(5) // 方法访问
  ```

  快速访问getters (**mapGetters**)

  ```js
  computed: {
     	...mapGetters(['listNum', 'saledLength', 'findByPrice'])
  }
  ```

  

* **mutations** (更改state的唯一方式)

  每个mutations都有一个字符串的事件类型和一个callback(state, payload) 参数一为state, 参数2为提交的载荷

  ```js
  const mutations = {
      reduce (state, n) {
          this.num -= n
      }
  }
  // in component use
  this.$store.commit('reduce', 5)
  ```

  mutations 修改state的对象属性 必须遵守响应式规则, 必须是同步函数

  ```js
  addProps (state, prop) {
      state.props = { ...state.props, prop}
      Vue.set(state.props, 'props', prop)
  }
  ```

  快速访问mutations(**mapMutations**)

  ```js
  methods: {
      ...mapMutations(['reduce'])
  }
  ```

* **actions**(通过actions提交mutations，可以包含异步操作)

  ```js
  const store = {
      actions: {
          incrementAsync (context) {
              window.setTimeout(() => {
                  context.commit('addProps')
                  context.commit('lalal')
              }, 1000)
          }
      }
  }
  
  this.$store.dispatch('incrementAsync')
  ```

  快速访问actions (**mapActions**)

  ```js
  export default {
      methods: {
          ...mapActions(['incrementAsync'])
      }
  }
  ```

  异步操作

  ```js
  actions: {
    actionA ({ commit }) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          commit('someMutation')
          resolve()
        }, 1000)
      })
    }
  }
  
  // use
  this.$store.dispatch('actionA').then(() => {
      ...
  })
  ```

* plugins

  ```js
  const myPlugin = store => {
      store.subscribe((mutation, state) => {
          // 每次mutations之后调用
      })
  }
  
  const store = new Vuex.Store({
      plugins: [myPlugin]
  })
  ```

  

  

### code

------

**index.js**

```js
import Vue form 'vue'
import Vuex form 'vuex'

// 辅助函数
import {mapState, mapGetters}

import module1 from './modules/module1'
import module2 from './modules/module2'

const debug = process.env.NODE_ENV !== 'production'
Vue.use(Vuex)

const state = {
    todos: [
        {name: 'football', done: false},
        {name: 'basketball', done: true},
    ]
}
const getters = {
    getDoneTodos(state) {
        return state.todos.filter(item => item.done)
    },
    doneNums(state) {
        return state.todos.filter(item => item.done).length
    },
    // 返回一个函数
    getByPrice(state) {
        return price => state.todos.filter(item => itme.price > price).length
    }
}
const mutations = {}
const actions = {}

export default new Vuex.Store({
  state,
  getters,
  mutaions,
  actions,
  modules: {
    module1,
    module2
  },
  strict: debug,   //设置运行模式
  plugin: debug ? [createLogger()] : [],   // 开发模式加入日志插件
}) 
```

**module1.js**

```js
// 服务层逻辑，封装ajax调用返回promise对象
import service from '../api/service/curl'

const state = {
	productions: [
      { name: 'shootGun', price:3 },
      { name: 'dog', price:10 },
      { name: 'ball', price: 6},
    ],
  	selectedProductions: [],
    user: 
}

// 定义state属性映射
const getters = {
  // 参数1：state对象， 参数2：自身getters，参数3：根state
  getAll: state => state.productions.filter(item => item.price > 5),
  calculateAll(state, getters, rootState) {
    return state.selectedProductions.reduce((total, item) => total + item.price * item.num, 0)
  }
}

// 定义commit修改, mutations必须是同步函数, 
const mutations = {
  // 参数1：当前state对象, 参数2：commit后提交的载荷
  addDiscountProduction(state, { name, num, price}) {
    state.selectedProduction.push({
      name,
      num,
      price * 0.9
    })
  },
  buyAll(state, {buyAll}) {
    state.selectedProduction = []
    state.isClear = buyAll
  },
    changeUser(state, payload) {
        state.user = 
    }
}

// 定义异步操作，内部触发mutations
const actions = {
  // 参数1：当前store对象 有state,commit,rootState等属性。 参数2：载荷对象
  buyProductions(context, productions) {
    service.buy(productions).then(res => {
      context.commit('buyAll', res.body.success)
    })
  }
}

export default {
  namespaced: true,   // 是否启用命名空间，默认false,如果启用则数据不挂在到全局跟store上
  state,
  getters,
  actions,
  mutations
}
```



**component use** 

```js
import { mapGetters, mapState, mapMutations, mapActions } from 'vuex'

new Vue({
  computed: {
    ...mapGetters({
      getAllProducts: 'getAll' // 把getter的getAll作为名为getAllProducts的计算属性
    }),
      
    // 把state的productions作为同名计算属性
    ...mapState(['productions', 'selectedProductions']),
    moduleState() {
    	return this.$store.state.moduleState.a
  }
  },
  methods: {
    ...mapMutations(['addDevice']), // 把store的addDevice作为同名方法
    ...mapActions([]), //
    buyAll() {
      this.$store.dispatch('buyProductions')
    }
  }
})
```

