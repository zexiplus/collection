## css

```css
//文字强制换行
word-wrap: break-word;

//设置起始，结束位置元素样式
div:last-child{}
div:first-child{}

//缩放元素
transform: scale(0.6);

//滚动条样式
1.	::-weskit-scrollbar 滚动条整体部分
2.	::-webkit-scrollbar-button 滚动条两端的按钮
3.	::-webkit-scrollbar-track 外层轨道
4.	::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去）
5.	::-webkit-scrollbar-thumb （滚动条里面可以拖动的那个）
6.	::-webkit-scrollbar-corner 边角
7.	::-webkit-resizer 定义右下角拖动块的样式


```

#### css实现长宽比例一致

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

