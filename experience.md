# experience

> 工作踩坑总结

1. vue 的 router-link 绑定click事件绑定不上，需要在原生元素上绑定.    **@click.native**

   ```html
   <router-link @click.native="handleClick" ></router-link>
   ```

2. 在vue中绑定html 字符串, 使用 **v-html** 指令

   ```html
   <div v-html="htmlTemplate"></div>
   ```

3. 

