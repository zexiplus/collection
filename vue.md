## vue record

#### vue

```js
/* ---------------异步引入模块 ------------ */
// 写法一
const component = () => import('componentName')  

// 写法二
const component = resolve => require(['componentName'],resolve)



/* --------------header 高度为100,动态设置main容器高度 --------- */
export default {
  data() {
    return { mainHeight: 0 }
  },
  created() {
    window.addEventListener('resize', () => {
      this.mainHeight = window.innerHeight - 100;         
    })
  },
}



/* --------------------返回当前route路径 ------------------ */
computed() { 
  return this.$route.path
}



/* ----------单独引用element 组件使用方法------- */
import { MessageBox } from 'element-ui'
//单独调用
MessageBox.alert(msg,title,{type:'error'});
//挂载后调用
this.$alert(msg,title,{type:'error'});



/* ---------------------------------样式绑定--------------------- */
:class="{'number-board': true}"



/* --------------------------------注册指令-------------------- */
//全局注册
Vue.directive(‘directiveName’,{
  	bind: function (el,binding) {
    	//binding.value 指绑定的值
  	},
  	inserted: function () {},
  	update: function () {},
  	componentUpdated: function () {},
 	unbind: function () {}

});
//组建内注册
export default {
  directives: {
    directiveName: {
      bind() {} .....//一系列钩子函数
    }
  }
}

/*---------------动态组建 (可用于一个页面有多个弹窗)----------------------*/
v-bind:is=”componentName”
<component :is=”currentView”></component>

```

#### axios

```js
//挂载请求拦截器
var ajax = axios.interceptors.request.use(function (config) {
    // 对请求进行设置（格式。。。）返回config
   	if (config.method === 'post') {
      newConf.headers['Content-Type'] = 'application/x-www-form-urlencoded';
    }
    return config;
}, function (error) {
    // 失败操作
  	alert('操作失败')
    return Promise.reject(error);
});

//取消拦截器
axios.interceptors.request.eject(interceptor)

/* ------------------- 相应拦截器，可处理失败响应等等 --------------------- */
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


```



#### vuex

```js
// store.js
import Vue form 'vue'
import Vuex form 'vuex'

const debug = process.env.NODE_ENV !== 'production'
Vue.use(Vuex)

export default new Vuex.Store({
  strict: debug,   //设置运行模式
  plugin: debug ? [createLogger()] : [],   // 开发模式加入日志插件
}) 
```

