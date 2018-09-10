# Vue Plugins

vue 插件收录, 用法



### Table of content

[TOC]



### prerender-spa-plugin

>预渲染插件， 提升首屏徐然速度， seo优化 

> https://github.com/chrisvfritz/prerender-spa-plugin





### vue-server-renderer

> 服务端渲染插件



### Vue-axios

> 客户端ajax 请求插件

>  [git repo](https://github.com/axios/axios)

* **创建ajax实例对象 axios.create(options)**

  ```js
  axios.create({
      baseURL: 'http://localhost:3000',
      timeout: 1000,
      headers: {'X-Custom-Header': 'football'},
      withCredentials: true,
  })
  ```

* **全局ajax请求拦截**

  ```js
  //挂载请求拦截器
  var interceptor = axios.interceptors.request.use(function (config) {
      // 对请求进行设置（格式。。。）返回config
     	if (config.method === 'post') {
        config.headers['Content-Type'] = 'application/x-www-form-urlencoded';
      }
      return config;
  }, function (error) {
      // 失败操作
    	alert('操作失败')
      return Promise.reject(error);
  });
  
  //取消拦截器
  axios.interceptors.request.eject(interceptor)
  
  /* -------------------响应拦截器，可处理失败响应等等 --------------------- */
  axios.interceptors.response.use(function (response) {
      //根据响应状态吗判断
    	if (response.status !== 200) {
        alert('服务器异常');
        return Promise.reject(response);
      } 
    	//根据响应返回信息判断
    	else if (response.data.ret !== 0) {
        alert(ERR_CODE[response.data.ret] || '操作失败');
        //返回失败态promise
        return Promise.reject(response.data.ret_msg);
      }
      return response;
  }, function (error) {
      // 错误处理
      return Promise.reject(error);
  });
  
  /* ------------------ Promise.all应用(多项操作，但接口不支持多个) --------------------- */
  
  {
    arr: [1,2,4,5]
    getById(id) {
      // service接口请求，返回promise实例
      return this.service.getById(id)
    }
    getByIds() {
      new Promise.all(this.arr.map(item => this.getById(item))).then(results => {
        this.getList()
      }).catch((err) => {
        alert('请求出错')
      })
    }
  }
  ```

* **分离服务 service**

  * ```service/Index.js```

    ```js
    import api from './api'
    import axios from 'axios'
    
    const ajax = axios.create({
        timeout: 3000
    })
    
    const service = {
        device: {
            getList(params) {
                return ajax.get(api.device.getList, params)
            }
        },
        user: {
            saveInfo(params) {
                return ajax.post(api.user.saveInfo, params)
            }
        }
    }
    
    export default {
        install(V, options) {
            V.prototype.$service = service
        }
    }
    ```

  * ```main.js```

    ```js
    import service from './service'
    Vue.use(service)
    ```

  * ```Component.vue```

    ```js
    export default {
        methods: {
            getList() {
                this.$service.getList().then(res => {
                    // to do something
                })
            }
        }
    }
    ```




### vue router

> vue 路由

> [参考链接](https://router.vuejs.org/installation.html)

* **this.\$route**

  > 代表当前页面的url封装对象,url不同对应不同的route对象

  ```js
  {name: "mainFrame", meta: {…}, path: "/mainFrame", hash: "", query: {…}, …}
  
  // this.$route.matched 表示vue-router 实例 匹配的模式
  {
      name: 'page2',
      path: 'page2', // 对应的浏览器url为 localhost:8080/mainFrame/page2
      component: page2
  },
      
  // this.$route.query  /path?name=xiaoxiix&id=123
  {
      name: 'xiaoxixi',
      id: '123'
  }
  ```


* **this.$router**

  > router 代表vue-router全局对象，全局唯一

  ```js
  {app: Vue, apps: Array(1), options: {…}, beforeHooks: Array(0), resolveHooks: Array(0), …}
  
  
  // 浏览器会生成一条历史记录，后退键可用
  this.$router.push('home') 
  // 浏览器地址变为 localhost/dynamicCom/123
  this.$router.push({name: 'dynamicCom', { params: { id: '123' } } })
  this.$router.push({name: 'dynamicCom', {query: {name: 'xiaoxixi'}}})
  
  // router.replace() 浏览器不会生成历史记录，用法和push一致
  this.$router.replace()
  
  // router.go(n)
  this.$router.go(-1) // 浏览器后退一步
  this.$router.go(2)  // 前进两步
  ```



* **using**

  * ```index.html```

    ```html
    <template>
    	<div>
            <router-view/>
            <router-view name="a"></router-view> <!-- 命名路由视图 -->
            <router-link to="/user">user</router-link>
           	<router-link :to="{ name: 'setting' }">main</router-link>
            <router-link :to="{ name: 'page1' }">link1</router-link>
          	<router-link :to="{ name: 'page2' }">link2</router-link>
        </div>
    </template>
    ```

  * ```main.js```

    ```js
    import router from './router'
    new Vue({
        el: '#app',
        router,
    })
    ```

  * ```router/Index.js```

    ```js
    import Vue from 'vue'
    import Router from 'vue-router'
    
    // 引入组件
    import mainFrame from '@/components/mainFrame'
    import dynamicCom from '@/components/dynamicCom'
    
    const page1 = () => import('@/components/page1')
    const page2 = () => import('@/components/page2')
    
    Vue.use(Router)
    
    const router = new Router({
      // 控制滚动行为，如果有上次访问同一页面的位置，则返回相同的位置，否则跳转到 x: 300, y: 800 位置
      scrollBehavior (to, from, savedPosition) {
        if (savedPosition) {
          return savedPosition
        } else {
          return { x: 300, y: 800 }
        }
      },  
      routes: [
        {
          path: '/mainFrame',
          name: 'mainFrame',
          component: mainFrame,
          // 嵌套子路由
          children: [
              {
                  name: 'page1',
                  path: '/page1',  // 对应的浏览器url为 localhost:8080/page1
                  component: page1,
                  meta: {
                      needAuth: true,
                  }
              },
              {
                  name: 'page2',
                  path: 'page2', // 对应的浏览器url为 localhost:8080/mainFrame/page2
                  component: page2
              },
              {
                  name: 'page3',
                  path: 'page3',
                  redirect: { name: 'page2' }, // 重定向路由，重定向至路由name: 'page2'
              },
              {
                  path: '/page2',
                  alias: '/page4',   // 别名路由，访问localhost:8080/page4 和 访问 ../page2 一致
                  component: page2,
              },
              {
                  name: 'nameRouter',
                  path: 'nameRouter',
                  components: {
                      default: nameView,
                      a: viewA
                  }
              },
              // 属性路由,组件可以接受路由中的参数作为props
              {
                  name: 'propsRouter',
                  path: 'props/:name',    //  访问../props/xiaoxixi 组件name属性为'xiaoxixi' 
                  component: propsRouter,
                  props: true,
              },
              {
                  name: 'propsRouter',
                  path: 'props/:name',   // 访问../props/xiaoxixi 组件name属性为'daxixi' 
                  component: propsRouter,
                  props: {name: 'daxixi'},
              },
          ]
            
        },
    	/* 	动态路由 在dynamicCom组建内可通过 this.$route.params.id 
    		拿到浏览器地址栏 /dynamicRouter/12 的参数12
    	*/
        {
          path: '/dynamicRouter/:id',
          name: 'dynamicRouter',
          component: dynamicCom
        },
        {
          path: '*', // 除此之外所有路由均不匹配时会匹配此项
          name: 'notFound',
          component: notFoundPage,
        },
      ]
    })
    // 全局路由钩子
    router.beforeEach((to, form, next) => {
        // 进入默认主页
        if（to.fullPath === '/') {
            next({name: 'home'})
        } else if (to.matched.some(item => item.needAuth)) {
            next({name: 'needAuth'})
        } else {
            next() // 通过路由
        } 
        next({path: 'route1'})
        next(new Error()) // 传递错误
    })
    
    router.onError(err => {
        // 处理错误
    })
    
    export default router
    ```


* **动态路由**

  ```vue
  <script>
      export default {
          watch: {
              '$route' (to, from) {
                  // 监听地址栏动态参数变化
                  console.log(to, from)
              },
          },
          // route hook in-component
          beforeRouteEnter (to, from, next) {
              next(vm => {
                  
              })
          },
          beforeRouteUpdate (to, from, next) {
              if (to.params.id == '123') {
                  next() // call next() to change route
              }
          },
          afterRouteLeave (to, from, next) {
              next()
          }
      }
  </script>
  ```


### vuex

> vue 状态管理

> [doc链接](https://vuex.vuejs.org/zh/guide/state.html)

- **state**

  > 全局单一状态树存储对象，遵循响应式原则

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

  * **访问state**

    ```js
    this.$store.state.user
    ```

    ```html
    <span>{{$store.state.user}}</span>
    ```


  * **快速访问state** (**mapState**)

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



- **getters**

  > 从state派生出一些属性，比如过滤，统计

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

  * **访问getters**

  ```js
  this.$store.getters.listNum // 属性访问
  this.$store.getters.findByPrice(5) // 方法访问
  ```

  * **快速访问getters** (**mapGetters**)

  ```js
  computed: {
     	...mapGetters(['listNum', 'saledLength', 'findByPrice'])
  }
  ```




- **mutations** 

  > 更改state的唯一方式

  >  每个mutations都有一个字符串的事件类型和一个callback(state, payload) 参数一为state, 参数2为提交的载荷

  ```js
  const mutations = {
      reduce (state, n) {
          this.num -= n
      }
  }
  // in component use
  this.$store.commit('reduce', 5)
  ```

  * mutations 修改state的对象属性 必须遵守响应式规则, 必须是同步函数

  ```js
  addProps (state, prop) {
      state.props = { ...state.props, prop}
      Vue.set(state.props, 'props', prop)
  }
  ```

  * **快速访问mutations(mapMutations)**

  ```js
  methods: {
      ...mapMutations(['reduce'])
  }
  ```

- **actions**

  > 通过actions提交mutations，可以包含异步操作

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

  * 快速访问actions (**mapActions**)

  ```js
  export default {
      methods: {
          ...mapActions(['incrementAsync'])
      }
  }
  ```



* **异步操作**

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

* **plugins**

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

* **vuex module**

  * ```index.js```

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

  * ```module.js```

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

  * component use 

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


