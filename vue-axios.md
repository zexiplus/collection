**Vue-axios**	

[Click](https://github.com/axios/axios)

> 1.ajax实例对象 axios.create(options)
>

```js
axios.create({
    baseURL: 'http://localhost:3000',
    timeout: 1000,
    headers: {'X-Custom-Header': 'football'},
    withCredentials: true,
})
```

> 2.全局ajax请求拦截


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

> 3.分离服务 service

> service/Index.js

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

> main.js

```js
import service from './service'
Vue.use(service)
```

> Component.vue

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

