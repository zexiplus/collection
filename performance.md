### bowser render block optmize 

------

#### 什么是阻塞？

在页面中我们通常会引用外部文件，而浏览器在解析HTML页面是**从上到下依次解析**、渲染，如果<head>中引用了一个a.js文件，而这个文件很大或者有问题，需要2秒加载，那么浏览器会停止渲染页面（此时是白屏显示），2秒后加载完成才会继续渲染，这个就是阻塞。



#### 为什么会阻塞？

因为浏览器不知道a.js中执行了哪些脚本，会对页面造成什么影响，所以浏览器会等**js文件下载并执行完成后**才继续渲染，如果这个时间过长，会白屏。

 CSS文件也一样，因为CSS文件会对DOM的样式，布局，色彩，效果产生影响，所以浏览器会等**CSS文件下载并执行完成后**继续。



#### css 阻塞

css 的加载**不会**阻塞dom的解析(DOM tree), 但**会**阻塞dom 树的渲染(render tree). **会**阻塞之后js代码的执行.

为了避免dom树重新渲染回溯影响性能浪费,  所以css代码的加载会阻塞dom树渲染.



#### 加载事件

> load 事件和 DOMContentLoaded 事件

**load事件**: 页面中的所有静态资源加载完成时触发包括样式表, 图像, 脚本

**DOMContentLoaded事件**: 当初始的 **HTML** 文档被完全加载和解析完成之后，**DOMContentLoaded** 事件被触发，而无需等待样式表、图像和子框架的完成加载. **注意**：**DOMContentLoaded** 事件必须等待其所属script**之前**的样式表加载解析完成才会触发



#### 解决阻塞

1. **延迟加载 defer**

   把script标签放到 body 的最后一行,  或者在script标签加入 defer属性

   **这两种的不同点**:  

   defer 会 **立即下载**,但到 浏览器解析至html标签时才**顺序执行**.而放在body后的script代码会在遇到这个标签时才下载,下载完成后执行.

   

   ```html
   <head>
       <script	src="js/defer.js" defer></script>
       <script	src="js/defer2.js" defer></script>
   </head>
   
   <!-- other -->
   <html>   
   	<body>
   	</body>
   	<script src="js/defer.js"></script>
   </html>
   ```

   

2. **异步加载 async**

   告知浏览器可以边下载边渲染而不用等到js下载再执行后才渲染, 使用了 async 属性的脚本不能保证执行的先后顺序, 异步脚本一定会在页面**load事件前执行**(所有资源都下载完), 但可能会在**DOMContentLoaded 事件前或后**执行

   ```html
   <head>
       <script src="js/async.js" async></script>
       <script src="js/async2.js" async></script>
   </head>
   ```

   

3. **动态加载 createElement('script')**

   当有需要时,再加载脚本

   ```js
   function createScript(src) {
       var script = document.createElement('script')
       script.type = 'text/javascript'
       script.src = src
       document.head.appendChild(script)
   }
   document.querySelector('button').onclick = createScript
   ```

4. **load 事件之后加载**

   Onload 事件为页面中所有的图片, 视频,js文件,css文件 等资源都加载完才触发

   ```js
   window.onload = function () {
       createScript('js/onload.js')
   }
   ```

5. **DOMContentLoaded 事件之后加载**

   DOMContentLoaded 事件为 形成完整的DOM树后就会触发, 不会理会图像, javascript文件, css文件等其他资源时候下载完毕

   ```js
   window.addEventListener('DOMContentLoaded', function () {
       createScript('js/onDOM.js')
   })
   ```

   





### page optmize proposal

---

1. **Minimize HTTP Requests**
2. **Use a Content Delivery Network**
3. **Add an Expires or a Cache-Control Header**
4. **Gzip Components**
5. **Put Stylesheets at the Top**
6. **Put Scripts at the Bottom**
7. **Avoid CSS Expressions**
8. **Make JavaScript and CSS External**
9. **Reduce DNS Lookups**
10. **Minify JavaScript and CSS**
11. **Avoid Redirects**
12. **Remove Duplicate Scripts**
13. **Configure ETags**
14. **Make Ajax Cacheable**
15. **Flush the Buffer Early**
16. **Use GET for AJAX Requests**
17. **Post-load Components**
18. **Preload Components**
19. **Reduce the Number of DOM Elements**
20. **Split Components Across Domains**
21. **Minimize the Number of iframes**
22. **No 404s**
23. **Reduce Cookie Size**
24. **Use Cookie-free Domains for Components**
25. **Minimize DOM Access**
26. **Develop Smart Event Handlers**
27. **Choose <link> over @import**
28. **Optimize Images**
29. **Optimize CSS Sprites**
30. **Don't Scale Images in HTML**
31. **Make favicon.ico Small and Cacheable**
32. **Keep Components under 25K**
33. **Pack Components into a Multipart Document**
34. **Avoid Empty Image src**

