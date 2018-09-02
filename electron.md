# Electron 

使用javascript, html, css 构建跨平台桌面应用

> https://electronjs.org/

```bash
npm i electron@latest
```



### 目录

[TOC]

### 封装/打包 & 脚手架模版

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

#### app

> 主线程可用

> https://electronjs.org/docs/api/app

> 监听应用程序的生命周期, 控制窗口操作

```js
const { app } = require('electron')
app.on('window-all-closed', () => {
    app.quit()
})

// 使程序获得焦点
app.focus() 

// 退出程序
app.quit()

// 隐藏程序
app.hide()
```



#### BrowserWindow

> https://electronjs.org/docs/api/browser-window

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



#### dialog 对话框

> https://electronjs.org/docs/api/dialog

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



#### IpcRenderer

> 从渲染进程到主进程的异步通讯

> https://electronjs.org/docs/api/ipc-renderer

```js
// 绑定信道事件
ipcRenderer.on(channel, function (event, arg) {})

// 向主进程发送事件
ipcRenderer.send(channel, args)
```



#### ipcMain

> 从主进程到渲染进程通讯

> https://electronjs.org/docs/api/ipc-main

```js
ipcMain.on(channel, function (event, arg) {})
```



#### remote

> 渲染进程可用

> https://electronjs.org/docs/api/remote

> 在渲染进程中使用主进程模块, 方法

```js
const { BrowserWindow } = require('electron').remote
let win = new BrowserWindow({
    width: 700,
    height: 700,
})
win.loadURL('http://www.github.com')
```



#### globalShortcut

> 快捷键监听

> https://electronjs.org/docs/api/accelerator

```js
const { app, globalShortcut } = require('electron')
app.on('ready', () => {
    globalShortcut.register('CommandOrControl+Y', () => {
        // do something 
    })
})
```



#### clipboard

> 主线程, 渲染进程可用 

> https://electronjs.org/docs/api/clipboard

> 剪贴板

```js
const { clipboard } = require('electron')

// 复制纯文本
clipboard.writeText('some string')

// 读取剪贴板中的内容
let clipText = clipboard.readText()

// 带有格式的文本
clipboard.writeHTML('<p>this is a p</p>')

// 读取带有格式的剪贴板内容
let clipHtml = clipboard.readHTML()

// 复制图片到剪贴板
clipboard.writeImage(image)

// 读取剪贴板的图片
let img = clipboard.readImage()
```



#### shell

> 主进程, 渲染进程可用

> https://electronjs.org/docs/api/shell

> 系统级别调用命令

```js
const { shell } = require('electron')

// 使用默认浏览器打开网址
shell.openExternal('https://www.github.com') 

// 在finder中显示文件 返回Boolean值, 成功为true, 失败为false
shell.showItemInFolder(fullPath)

// 以默认应用打开文件 返回Boolean值, 成功为true, 失败为false
shell.openItem(fullPath) 

// 删除文件 返回Boolean值, 成功为true, 失败为false
shell.moveItemToTrash(fullPath)

// 放出系统提示音
shell.beep()

```



#### systemPreferences

> 主进程可用

> https://electronjs.org/docs/api/system-preferences

> 获取 系统偏好设置

```js
const sys = require('electron').systemPreferences

// 是否是黑暗模式 mac os有效
sys.isDarkMode()

```



#### tray

> 主进程可用

> https://electronjs.org/docs/api/tray

> 系统托盘

```js
const { app, Menu, Tray } = require('electron')
let tray = null
app.on('ready', () => {
    tray = new Tray('/path/to/myicon')
    const contextMenu.buildFromTemplate([
        {label: 'Item1', type: 'radio'},
        {label: 'Item2', type: 'radio', checked: true}
    ])
    // 设置鼠标提示
    tray.setToolTip('hello my guest')
    // 设置菜单内容
    tray.setContextMenu(contextMenu)
    // 销毁托盘
    tray.destory()
    // 设置图标
    tray.setImage(image)
    
    tray.on('click', (event, bounds, position) => {
        // event 事件对象, bounds 系统托盘图标的边界, position 事件的位置信息
    }) 
})
```



#### screen

> 主进程, 渲染进程可用, 在 app 模块ready事件之后可用

> https://electronjs.org/docs/api/screen

> 检测屏幕, 光标位置信息

```js
const electron = require('electron')
const { app, BrowserWindow } = electron

app.on('ready', () => {
    const { screen } = electron
    
    // 获得显示器长宽
    const { width, height } = screen.getPrimaryDisplay().workAreaSize
    
    // 获得所有显示器列表数组
    const displays = screen.getAllDisplays()
    
    // 获取鼠标绝对位置
    const cursor = screen.getCursorScreenPoint()
   	
    // 返回菜单的高度
    const menu = screen.getMenuBarHeight()
})
```



#### session

> 主进程可用

> https://electronjs.org/docs/api/session

> 管理浏览器会话, cookie, 缓存

```js
const { BrowserWindow } = require('electron')
let win = new BrowserWindow({
    width: 800,
    height: 600
})

win.loadURL('http://www.github.com')
const session = win.webContents.session
console.log(session.getUserAgent())
```



#### Menu

> 主进程可用

> https://electronjs.org/docs/api/menu

> 创建原生应用菜单

```js
const { app, Menu } = require('electron')

const template = [
    {
        label: 'Edit',
        submenu: [
            {role: 'undo'},
            {role: 'redo'},
            {role: 'delete'},
        ]
    }
]

const menu = Menu.buildFromTemplate(template)
Menu.setApplicationMenu(menu)
```



#### Notification

>  主进程可用

> https://electronjs.org/docs/api/notification

> 系统通知栏

```js
import Notification from 'electron'

Notification.isSuported() // 返回系统是否支持通知

const opt = {
    title: '通知',
    body: '这是一条通知',
    icon: '/assets/img/nogify.png',
    sound: '/assets/sound/notify.mp3',
}
const notify = new Notification(opt)
// 显示这条通知
notify.show()
```



#### webContents

> 主进程可用

> https://electronjs.org/docs/api/web-contents

> 渲染和控制web页面

```js
const { BrowserWindow } = require('electron')
let win = new BrowserWindow({
    width: 800,
    height: 600
})

let contents = win.webContents

// 获取app所有的web 页内容, 是一个数组
contents.getAllWebContents()

// 获取当前焦点的web 内容页
contents.getFocusedWebContents()

// 主进程向渲染进程发消息
contents.send(channel, function (args) {
    // do someting 
})


```



#### webFrame

> 渲染进程可用

> https://electronjs.org/docs/api/web-frame

> 自定义渲染页面

```js
const { webFrame } = require('electron')

// 页面放大2倍
webFrame.setZoomFactor(2)

// 获取当前页面的缩放倍数
webFrame.getZoomFactor()

// 插入text到焦点元素
webFrame.insertText(text)

// 释放不在使用的内存
webFrame.clearCache()
```



#### webView

> 渲染进程可用

> https://electronjs.org/docs/api/webview-tag

> 类似于iframe 在进程中显示独立的网页

```html
<webview id="view" 
         src="https://www.github.com" 
         plugins                // 可以访问插件
         preload="./test.js"    // 指定事先加载脚本
         
         ></webview>
```



#### powerMonitor

> 主进程可用

> https://electronjs.org/docs/api/power-monitor

> 监控系统级事件

```js
const electron = require('electron')
const { app } = electron

// 必须在程序启动后监听系统电源事件
app.on('ready', () => {
    electron.powerMonitor.on('suspend', () => {
        console.log('the system is going to sleep')
    })
})
```



#### powerSaveBlocker

> 主进程可用

> https://electronjs.org/docs/api/power-save-blocker

> 组织系统进入休眠状态

```js
const { powerSaveBlocker } = require('electron')

// 阻止系统休眠
const id = powerSaveBlocker.start('prevent-display-sleep')

// 取消
powerSaveBlocker.stop(id)
```



#### cookies

> 主线程可用

> https://electronjs.org/docs/api/cookies

> 查询和修改一个会话的cookies

```js
const { session } = require('electron')

// 设置cookie
const cookie = {
    url: 'http:// www.github.com',
    name: 'dummy_name',
    value: 'dummy_value',
}
session.defaultSession.cookies.set(cookie, err => {
    if (err) {
        console.log(err)
    }  
})

// 读取cookie
session.defaultSession.cookies.get({url: 'http://www.github.com'}, (err, cookies) => {
    console.log(err, cookies)
})
```



#### DesktopCapturer

> 渲染进程可用

> https://electronjs.org/docs/api/desktop-capturer

> 录屏, 截屏



#### net

> 主进程可用

> https://electronjs.org/docs/api/net

> 使用Chromium的原生网络库发出HTTP / HTTPS请求

```js
const { net } = require('electron')
const request = net.request('https://www.github.com')
request.on('response', res => {
    console.log(res)
})
```



#### client-request

> 主线程可用

> https://electronjs.org/docs/api/client-request

> 发起http/https请求

```js
const { ClientRequest } = require('http')

const request = new ClientRequest({
    method: 'GET',
    protocol: 'https:',
    hostname: 'github.com',
    port: 443,
    path: '/'
})

request.on('login', (authInfo) => {})
request.on('response', res => {
    console.log(res)
})
```



