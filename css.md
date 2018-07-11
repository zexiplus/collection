## css

```css
// 单行超出显示省略号：
White-space:nowrap;
overflow:hidden;
text-overflow:ellipsis;

// 多行文本超出显示省略号

display:-webkit-box;
-webkit-line-cap:3;
-webkit-box-orient: vertical;
overflow:hidden;
text-overflow:ellipsis;

// 单行文字均匀对齐
text-align-last: justify;

// 不定宽元素居中
display: table;
margin: 0 auto;

// 只允许在空格处换行
word-wrap:break-word;


// 设置起始，结束位置元素样式
div:last-child{}
div:first-child{}

// 第n个p元素
p:nth-child(n)

// 缩放元素
transform: scale(0.6);

// 滚动条样式
1.	::-weskit-scrollbar 滚动条整体部分
2.	::-webkit-scrollbar-button 滚动条两端的按钮
3.	::-webkit-scrollbar-track 外层轨道
4.	::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去）
5.	::-webkit-scrollbar-thumb （滚动条里面可以拖动的那个）
6.	::-webkit-scrollbar-corner 边角
7.	::-webkit-resizer 定义右下角拖动块的样式

// 计算样式
width: calc(~"100% - 40px");

// :before , :after 伪类
.redStar:before {
  content: '*';
  color: red;
}
```

#### css selector

```css
p~ul  // 选择前面有 <p> 元素的每个 <ul> 元素。
```



#### BFC(block formating context)

```css
/* 创建bfc
	float: 值不为none;
	position: 值不为static或relative;
	overflow: 值不为visible;
	display: 值为 table-cell, table-caption, inline-block, flex, inline-flex
*/

/* 最常用方法 */
overflow: hidden;

```

#### BFC的应用

1.解决margin重叠（为每个带有margin的块包裹一个新的bfc）

2.解决浮动元素无法撑开父元素的高度（给包含浮动的元素设置为bfc）

3.解决文字环绕浮动元素（给包含文字的元素设置bfc）

#### media queries

```css
@media only screen and (min-width: 401px) and (max-width: 960px) {
    #main {
        ...
    }
}
```



#### FLEX布局（flexible弹性布局） 

[效果展示1](https://codepen.io/zexiplus/pen/ELppKb) [效果展示2](https://codepen.io/zexiplus/pen/wjxbEN)

```css
display: flex;
display: inline-flex;
display: -webkit-flex; /* Safari */
/* 注：容器设置为flex后，子元素自动成为容器成员， 子元素的float, clear, vertical-align 都将失效 */

/* 容器上的5个属性 */

/* 排列方向 */
flex-direction: row | row-reverse | column | column-reverse; 

/* 换行规则  */
flex-wrap: nowrap | wrap | wrap-reverse;
/* nowrap ： 不换行，所有item排列成一排，width ： 100% 被忽略 */
/* wrap ： 换行显示 */

/* 水平对齐规则 */ 
justify-content: flex-start | flex-end | center | space-between | space-around;
/* space-between: 等间隔排列 第一项前无间隔 */
/* space-around : 等间隔排列 第一项前有间隔 */

/* 垂直对齐规则 */
align-items: flex-start | flex-end | center | baseline | stretch;
/* stretch: flex-item 被纵向拉伸至flex-contanier 一样的高度 */

/* 多轴对其规则 */
align-content: flex-start | flex-end | center | space-between | space-around | stretch;





/* 子属性 */

/* 排列顺序 default 0 用此属性改变排列顺序 */
order: <integer>; 

/* 如果存在剩余空间， 放大的倍数 */
flex-grow: <number>; /* default 0 */

/* 如果空间不足， 缩小 */
flex-shrink: <number>; /* default 1 */

/* 属性定义了在分配多余空间之前，项目占据的主轴空间（main size） */
flex-basis: <length> | auto; /* default auto */

/* align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。 */
align-self: auto | flex-start | flex-end | center | baseline | stretch;
```



#### css3-3D

[demo-cube](https://codepen.io/zexiplus/pen/NMOVrX)

```css
/* 父元素设置之后，其后代元素便会有3d效果 */
.container {
    trnasform-style: preserve-3d; 
}

.container {
    perspective: 300px;  // 景深  
    perspective-origin: 0px 0px; // 视角起始点
}

.trans {
   transform: translate3d(10px, 10px, 10px) | translateX(30px) | translateY(30px) | translateZ(30px);
   transform: rotateX(90deg) | rotateY(90deg) | rotateZ(90deg);
   transform: scale3d(1, 1, 1) | scaleX(1.2) | scaleY(1.2) |  scaleZ(1.2);
}
```

![3d](./imgs/3d.jpg)





####css: filter滤镜(组合能达到神奇的效果)

```css
// 灰度
.grayscale {
    filter: grayscale(1) // 0~1 代表灰度
}

// 褐色
.sepia {
    filter: sepia(.5)
}

// 饱和度
.saturate {
    filter: saturate(5)
}

// 色相旋转 
.hue-rotate {
    filter: hue-rotate(90deg)
}

// 反色
.invert {
    filter: invert(1)
}

// 透明度
.opacity {
    filter: opacity(.5)
}

// 亮度
.brightness {
    filter: brightness(2)
}

// 对比度
.contrast {
    filter: contrast(.4)
}

// 模糊
.blur {
    filter: blur(10px)
}

// 阴影
.drop-shadow {
    filter: drop-shadow(5px 5px 10px #ccc)
}
```



#### css实现长宽比例一致div容器

```html
<div class=”container”>
  <div class=”dummy”></div>
  <div class=”content”></div>
</div>
```

```css
.container {
  position:relative;
  overflow: hidden;
  border: 1px solid red;
 }
.dummy {
  margin-top: 100%;
}
.content {
  position: absolute;
  top:0;
  left:0;
  right:0;
  bottom:0
}
```

```js
// 原理
/*----------
首先容器container块内包含了两个div，一个是dummy，这个纯粹是为了实现缩放效果加的，另一个content里面放的是我们真正想要展现的内容。其实原理也很简单，大家都知道div是块元素，它默认就是占一行，宽度本来就是自适应的，所以我们需要做的是让它的高度能随宽度改变。在不使用js的前提下，靠的就是前面提到的dummy那个块来实现，dummy只设置了一个css属性，margin-top:100%，相信大家都反应过来了。因为容器宽度已经在那儿了，通过dummy块的margin-top来把整个的高度撑得和宽度一样，当容器宽度改变时，dummy的位置也会改变，进而容器高度就跟着发生了变化。
为什么给父元素这样设置之后就能把父元素高度撑起来呢，准确的原理解释起来有点复杂。可以简单的理解为，当子元素脱离文档流时，父元素不知道子元素的存在，所以导致高度塌陷。当设置父元素为display:inline-block或者overflow:hidden时，迫使父元素去检查自己内部有哪些子元素，而这时候就发现了之前absolute定位的子元素，所以高度就撑开了

-----------*/
```



### less

```less
& // 指代自身
p {
  &:hover {
    ...
  }
}


```



