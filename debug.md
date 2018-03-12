### 调试方法1   console

```js
@1  占位符  console.log(‘需要打印的是%o’,obj)   // %o  对象占位符 %d //数字  %s //字符串   %f //浮点数  

@2  打印组   
console.group()
   console.log(1)
   console.log(2)
console.groupEnd()

@3  调用栈 
console.trace();

@4  运行时间   
console.time(timerName)		  		   
	fn（）    	  
console.timeEnd(timerName)

@5  dom结构  
console.dirxml()

```



### 调试方法2   debugger

在js代码内打上debugger 关键字

当代码运行至 debugger 处，浏览器打开控制台scope窗会显示当前scope的变量

  





