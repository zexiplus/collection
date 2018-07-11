## vue router

> [参考链接](https://router.vuejs.org/installation.html)

>**this.\$route**  (代表当前页面的url封装对象,url不同对应不同的route对象)

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

> **this.$router**  (router 代表vue-router全局对象，全局唯一)
>
> $go, push, replace 是对 window.history.pushState, window.history.replaceState and window.history.go 方法的模拟$

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



> mainFrame.vue

```vue
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



> Index.js

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



> dynamicCom.vue(动态路由组件页)

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



> main.js

```js
import router from './router'
new Vue({
    el: '#app',
    router,
})
```

