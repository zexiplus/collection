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
  yarn run dev
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

