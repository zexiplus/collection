# Debug

> 总结了常用web前端调试方法和nodejs调试方法



### Catalogue

[TOC]

### webfont debug



#### console

* **console.log**

  ```js
  // 占位符 %o(对象), %d(数字),%s(字符串), %f(浮点数)
  console.log(‘需要打印的是%o’,obj)
  ```

* **console.group**

  > 打印组

  ```js
  console.group()
     console.log(1)
     console.log(2)
  console.groupEnd()
  ```

* **console.trace()**

  > 打印调用栈

  ```js
  function caller() {
      exec()
  }
  function exec() {
      console.trace()
  }
  
  caller()
  
  // exec	@	VM123:5
  // caller	@	VM123:2
  // (anonymous)	@	VM123:8
  
  ```

* **console.time**

  > 打印执行时间

  ```js
  console.time(timerName)		  		   
  fn（）    	  
  console.timeEnd(timerName)
  ```

* **console.dirxml**

  > 打印dom结构

  ```js
  console.dirxml(document.querySelector('body'))
  ```

* **console.dir**

  > 打印对象

  ```js
  console.dir(obj)
  ```


#### debugger

在js代码内打上debugger 关键字

当代码运行至 debugger 处，浏览器打开控制台scope窗会显示当前scope的变量

  

### Nodejs debug

> nodejs 程序调试工具， 方法， 技巧



#### node cli debug

> node 命令行窗口调试器

```shell
node inspect test.js
# 进入debug模式， 会停在第一行可执行脚本处

# 新版node debug 命令被弃用了， 用 inspect 代替
node debug test.js
```

进入node debuger后输入下面命令(括号内为简写）

* **cont(c)**

  > 执行剩余的所有代码

* **setBreakpoint(filename, line)**

  > 设置断点命令， 执行cont 会到此行暂定执行 ， 简称sb(filename, line), filename可省略，省略默认当前文件

* **clearBreakpoint(filename, line)**

  > 取消断点 简写**cb()**

* **next(n)**

  > 逐步执行

* **list(n)**

  > 列出当前点后n行的代码

* **scripts**

  > 列出当前文件所有引用的模块（不包括内置模块）

* **step(s)**

  > 进入函数内部

* **backtrace**

  > 查看当前函数在被调用函数的返回位置

* **out(o)**

  > 从函数中跳出至原执行流

* **watch('variable')**

  > 监听变量， 变量每次修改都会显示出来, 变量值要加**引号**

* **watchers**

  > 查看所有被监听的变量值

* **unwatch('variable')**

  > 取消监听变量

* **restart**

  > 从程序开始从新执行debug

* **repl**

  > 进入repl环境


#### node debug with chrome

* **调试服务程序**
  * 启动

    ```shell
    node --inspect app.js
    ```

  * 打开浏览器 输入 chrome://inspect， 点击target 

  * Source 面板 Add folder to Workspace 选择开发项目的目录

  * 在文件中添加响应断点 查看变量

* **调试非服务程序**

  * 启动

    > 在第一行就增加断点, 这样可以避免非服务脚本,运行太快而退出

    ```shell
    node --inspect-brk=9229 app.js
    ```

  * 之后类似

* **调试运行时脚本**

  * 启动程序

    ```shell
    node app.js
    ```

  * 按端口查看进程pid

    ```shell
    lsof -i :3000
    ```

  * 启动调试程序

    ```shell
    node -e 'process._debugProcess()'
    ```








