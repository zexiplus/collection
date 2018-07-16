### dom operate

```js
// 获得所有class为epic的节点并遍历
let node = document.querySelectorAll('epic').forEach()

// 获得节点的class属性
node.getAttribute('class') // => epic

// 设置节点的class属性

node.setAttribute('class', 'epic2')
```



### html translation

| 显示 | 说明           | 实体名称 | 实体编号 |
| ---- | -------------- | -------- | -------- |
|      | 半方大的空白   | & ensp; | &#8194;  |
|      | 全方大的空白   | & emsp; | &#8195;  |
|      | 不断行的空白格 | & nbsp; | &#160;   |
| <    | 小于           | & lt ; | &#60;    |
| >    | 大于           | & gt ;   | &#62;    |
| &    | &符号          | & amp ;  | &#38;    |
| "    | 双引号         | & quot;  | &#34;    |
| ©    | 版权           | & copy;  | &#169;   |
| ®    | 已注册商标     | & reg;   | &#174;   |
| ™    | 商标（美国）   | ™        | &#8482;  |
| ×    | 乘号           | & times; | &#215;   |
| ÷    | 除号           | & divide; | &#247;   |

### size

1. window.innerHeight     // 浏览器视窗内高度（当前页面 ，不包括外层iframe）
2. window.innerWidth      // 浏览器内宽度
3. div.offsetHeight            // 元素在垂直方向占用的空间大小（单位-像素）
4. div.offsetWidth             // 元素在水平方向占用空间大小（单位-像素）
5. div.offsetLeft                 // 元素的左外边框至包含元素的左内边框之间的像素距离\
6. div.scrollLeft                  // 既可以确定元素当前滚动状态，又可以设置元素的**滚动位置**



![domOffset](./imgs/DOMOffset.gif)

