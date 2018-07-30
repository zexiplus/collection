# experience

> 工作踩坑总结

1. vue 的 router-link 绑定click事件绑定不上，需要在原生元素上绑定.    **@click.native**

   ```html
   <router-link @click.native="handleClick" ></router-link>
   ```

2. ​