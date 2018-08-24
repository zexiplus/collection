# Electron 

使用javascript, html, css 构建跨平台桌面应用

> https://electronjs.org/

```bash
npm i electron@latest
```



### 目录



### 封装与打包

* **electron-vue**

  > https://github.com/SimulatedGREG/electron-vue

  > vue 在 electron环境下的脚手架

  ```bash
  # 初始化项目结构
  vue init simulatedgreg/electron-vue my-project
  
  cd my-project
  yarn 
  # 开发模式
  yarn run dev
  
  # 编译生成二进制文件
  yarn run build
  ```

* **electron-forge**

  > 可打包成二进制文件

  > https://github.com/electron-userland/electron-forge

  ```bash
  npm install -g @electron-forge/cli@beta
  electron-forge init my-new-app
  cd my-new-app
  
  # 开发模式下打开应用
  npm start
  
  # 生成二进制文件
  npm run package
  
  # 生成二进制文件的压缩包
  npm run make 
  
  
  ```

  > 使用说明, 两种方法

  * 使用electron-forge init projectname 来初始化项目, 然后在此项目内开发
  * 基于已有的项目, 把package.json文件内的 script , config, dependencies 和 devDependdencies 复制进来

  

* **electron-packager**

  > https://github.com/electron-userland/electron-packager

  ```bash
  electron-packager <sourcedir> <appname> --platform=<platform> --arch=<arch> [optional flags...]
  ```

* **electron-builder**

  > https://github.com/electron-userland/electron-builder

  ```bash
  # download 
  yarn add electron-builder --dev
  ```






### API



* **BrowserWindow**

  > 创建或销毁窗口 

  ```js
  const { BowserWindow } = require('electron').remote
  
  let win = new BowserWindow({
      width: 400,
      height: 400,
      minWidth: 400,
      minHeight: 400,
      resizable: true,
      frame: false, // 无边框
      show: true, // 显示或隐藏
  })
  
  win.loadURL(path.join(`file://${__dirname}/app/index.html`))
  
  // 监听页面 window.close() 事件
  win.on('close', () => {
      app = null
  })
  
  // 监听页面的crash事件并给出提示框
  win.on('crash', () => {
      const option = {
        type: 'info',
        message: 'This process has crashed.',
      }
      electron.dialog.showMessageDialog(option, fn)
  })
  
  win.on('move', function () {
      app.getSize() // 获得尺寸
      app.getPosition() // 获得位置
  })
  win.on('resize', fn)
  win.show()
  
  ```

  > app/index.html

  ```html
  <a href="javascript:window.close()">点击关闭窗口</a>
  <a href="javascript:process.crash()">点击触发crash</a>
  ```

  



* **dialog 对话框** 

  > 打开文件夹

  ```js
  const { dialog } = require('electron').remote
  
  dialog.showOpenDialog({
      properties: ['openDirectory', 'openFile', 'multiSelections'],//允许选择目录,文件,多选 
      defaultPath: '', // 默认打开路径
  }, function (filePaths) {
      // filePaths 为路径数组
  })
  ```

  > 保存文件操作

  ```js
  dialog.showSaveDialog({
      defaultPath: '',
  }, function (filename) {
      
  })
  ```

  > 系统通知对话框

  ```js
  dialog.showMessageDialog({
      type: 'info',
      title: 'Renderer Process Crashed',
      message: 'This process has crashed.',
      buttons: ['Reload', 'Close']
  })
  ```

  

* **IpcRenderer**

  > 从渲染进程到主进程的异步通讯

  ```js
  // 绑定信道事件
  ipcRenderer.on(channel, listener)
  
  // 向主进程发送事件
  ipcRenderer.send(channel, args)
  ```

  

