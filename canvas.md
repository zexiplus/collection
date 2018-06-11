### canvas web api

##### 绘制路径

```js
var ctx = canvas.getContext(‘2d’)   // 获取canvas2d上下文
canvas.getContext(‘webgl’)   //获取3d对象
ctx.beginPath()  //开始绘制路径   //ctx.closePath()   //连接起点终点
ctx.moveTo(x,y)  //设直线段的起点
ctx.lineTo(x,y)   //设置线段的终点
ctx.lineWidth = 1.0  //设直线段宽度
ctx.strokeStyle = ‘red’ //设直线段颜色
ctx.stroke() //绘制线段
```

##### 绘制矩形

```js
ctx.fillStyle = ‘yellow’   //填充色
ctx.fillRect(x,y,w,h)    //绘制实心矩形
ctx.strokeRect(x,y,w,h)   //绘制空心矩形
ctx.clearRect(x,y,w,h)    //清除矩形

```

##### 绘制文本

```js
ctx.font = ‘20px bold Arial’    //设置字体，大小
ctx.textAlign = ‘left’          //设置字体居中方式
ctx.fileText(text,x,y)				//绘制实心字体
ctx.strokeText(text,x,y)       //绘制空心字体

```

##### 绘制圆形和扇形

```js
//绘制扇形，anticlockwise 顺时针false，逆时针true，绘制扇形前需要调用beginPath方法
ctx.arc(x,y,r,startAngle,endAngle,anticlockwise)
```

##### 图像处理

```js
//绘制图像
ctx.drawImage(img,x,y[,width,height])      
ctx.getImageData(x,y,width,height)    

//获取图像信息imageData对象有一个data属性，它的值是一个一维数组。该数组的值，依次是每个像素的红、绿、蓝、alpha通道值，因此该数组的长度等于 图像的像素宽度 x 图像的像素高度 x 4，每个值的范围是0–255
ctx.putImageData(imgData,x,y)

```

##### 设置渐变色

```js
//x1,y1起点坐标 x2，y2 终点坐标
var gradient = ctx.createLinearGradient(x1, y1, x2, y2) 
gradient.addColorStop(0,color)
gradient.addColorStop(1,color)

 //创见圆形渐变
createRadialGradient(x1,y1,r1,x2,y2,r2)
```

