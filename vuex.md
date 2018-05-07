**index.js**

```js
import Vue form 'vue'
import Vuex form 'vuex'

import module1 from './modules/module1'
import module2 from './modules/module2'

const debug = process.env.NODE_ENV !== 'production'
Vue.use(Vuex)

const state = {}
const getters = {}
const mutations = {}
const actions = {}

export default new Vuex.Store({
  state,
  getters,
  mutaions,
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
}

// 定义state属性映射
const getters = {
  // 参数1：state对象， 参数2：自身getters，参数3：根state
  getAll: state => state.productions.filter(item => item.price > 5),
  calculateAll(state, getters, rootState) {
    return state.selectedProductions.reduce((total, item) => total + item.price * item.num, 0)
  }
}

// 定义commit修改
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
import { mapGetters,mapState } from 'vuex'

new Vue({
  computed: {
    ...mapGetters({
      getAllProducts: 'getAll'
    }),
    ...mapState(['productions', 'selectedProductions']),
    moduleState() {
    	return this.$store.state.moduleState.a
  }
  },
  methods: {
    buyAll() {
      this.$store.dispatch('buyProductions')
    }
  }
})
```

