# Electron 学习笔记

## 什么是Electron

- 定义：Electron 是一个框架，使开发者能够为 macOS、Windows和Linux 构建跨平台桌面应用通过将 Web 技术(HTML、CSS、JavaScript)与 Node.js 和原生代码相结合。
- 学习中文官网：<https://electron.nodejs.cn/>
- 维护者：Electron 由多家公司共同维护：Microsoft、Slack/Salesforce、Notion等；
- 开发语言：Electron用C++ 和 Objective-C编写；

## 构建你的第一个应用

```javascript
// 创建项目文件夹
mkdir my-electron-app && cd my-electron-app
// 初始化 package.json 文件
npm init
// 下载 Electron 模块
// 注意：Electron 作为开发依赖，是因为它只在开发和构建阶段需要，最终分发的应用包中不包含 npm 上的 electron 模块。
// 而是包含特定平台的 Electron 运行时二进制文件。
npm install electron --save-dev
```

运行第一个 Electron 应用

```javascript
// 在 package.json 中添加启动脚本
"scripts": {
  "start": "electron ."
}
// 根目录创建 index.js 文件 代码如下  
console.log('Hello from Electron 👋')
// 运行应用  那么在终端就会输出 Hello from Electron 👋
npm start
```

将网页加载到 BrowserWindow 中
根目录新建 index.html 文件 代码如下

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <!-- https://web.nodejs.cn/en-US/docs/Web/HTTP/CSP -->
    <meta
      http-equiv="Content-Security-Policy"
      content="default-src 'self'; script-src 'self'"
    />
    <meta
      http-equiv="X-Content-Security-Policy"
      content="default-src 'self'; script-src 'self'"
    />
    <title>Hello from Electron renderer!</title>
  </head>
  <body>
    <h1>Hello from Electron renderer!</h1>
    <p>👋</p>
  </body>
</html>
```

根目录的入口文件 index.js 修改如下：

```javascript
/**
 *  app 控制应用的事件生命周期
 *  BrowserWindow 创建和管理应用窗口：每个窗口都是一个独立的渲染进程，类似浏览器的主程序
 */
const { app, BrowserWindow } = require('electron')

const createWindow = () => {
  const win = new BrowserWindow({
    width: 800,  // 宽度 800 像素
    height: 600, // 高度 600 像素
  })

  // 加载 index.html 文件
  win.loadFile('index.html')
}

/**
 * 等待初始化完成
 * app. whenReady() 返回一个 Promise, 在Electron 完全初始化后解析
 */
app.whenReady().then(() => {
  createWindow();
  /**
   * 针对 macOS 平台的特殊处理
   * 当应用激活时(例如点击 Dock 图标或重新打开已关闭的窗口)
   * 如果没有窗口存在, 则创建一个新窗口
   */
  app.on("activate", () => {
    if (BrowserWindow.getAllWindows().length === 0) {
      createWindow();
    }
  });
});

/**
 * 当所有窗口被关闭时触发
 * process.platform !== "darwin" 表示非 macOS 平台
 * 关键点：在 Windows 和 Linux 平台上, 当所有窗口被关闭时, 应用会继续运行, 直到用户手动退出。
 * 而在 macOS 平台上, 当所有窗口被关闭时, 应用也会继续运行(这是 macOS 的标准设计)
 */
app.on("window-all-closed", () => {
  if (process.platform !== "darwin") {
    app.quit();
  }
});
```

## 使用预加载脚本

定义：Electron的主进程是一个具有完全操作系统访问权限的 Node.js 环境。除了 Electron 模块之外，你还可以访问 Node.js内置组件以及通过 npm 安装的任何软件包。另一方面，出于安全原因，渲染器进程默认运行网页并且不运行Node.js。为了将 Electron 的不同进程类型桥接在一起，我们需要使用一个成为预加载的特殊脚本。

使用预加载脚本增强渲染器
BrowserWindow 的预加载脚本可以访问 HTML DOM 以及 Node.js 和 Electron APU 的有限子集的上下文中运行。

在根目录添加一个新的 preload.js 脚本文件，该脚本将Electron的process.versions 对象的选定属性公开给 versions 全局变量中的渲染器进程。
代码如下：

```javascript
// preload.js
const { contextBridge } = require('electron')

/**
 *  语法：contextBridge.exposeInMainWorld('apiKey', apiObject)
 *  作用：将对象安全地暴露给渲染进程的window对象，参数：
 *  @param {string} apiKey - 渲染进程中访问 API 的键名
 *  @param {object} apiObject - 包含要暴露的函数或属性的对象
 */
contextBridge.exposeInMainWorld('versions', {
  node: () => process.versions.node,
  chrome: () => process.versions.chrome,
  electron: () => process.versions.electron
  // we can also expose variables, not just functions
})
```

要将次脚本附加到渲染器进程，要将上面的文件路径传递给 BrowserWindow 构造函数的 webPreferences.preload 选项。
代码如下： 这样就可以在全局访问  versions 全局变量了。

```javascript
// index.js
const { app, BrowserWindow } = require('electron')

const createWindow = () => {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js'),
    },
  })

  win.loadFile('index.html')
}
```

进程间通信
使用 Electron 的 icpMain 和 icpRenderer 模块进行进程间通信(IPC)。要将信息从网页发送到主进程，你可以使用 ipcMain.handle 设置主进程处理程序，然后公开一个
调用 ipcRenderer.invoke 的函数以在预加载脚本中触发该处理程序。
首先，在预加载脚本中设置 invoke 调用：

```javascript
// preload.js
const { contextBridge, ipcRenderer } = require('electron')

contextBridge.exposeInMainWorld('versions', {
  node: () => process.versions.node,
  chrome: () => process.versions.chrome,
  electron: () => process.versions.electron
  ping: () => ipcRenderer.invoke('ping'),
  // we can also expose variables, not just functions
})
```

然后，在主进程设置 handle 监听器：

```javascript
// index.js
const { app, BrowserWindow, ipcMain } = require('electron/main')

const path = require('node:path')

const createWindow = () => {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js')
    }
  })
  win.loadFile('index.html')
}
app.whenReady().then(() => {
  ipcMain.handle('ping', () => 'pong')
  createWindow()
})
```

设置好发送方和接收方后，就可以在渲染器进程中调用 ping 函数了：

```javascript
// index.html
const { versions } = window.electronAPI
versions.ping().then(response => console.log(response)) // 输出: 'pong'
```

### 打包你的应用

将现有项目迁移到 Electron Forge 构建工具链的标准步骤：

```javascript
// 作用：在你的项目中安装 Electron Forge 的CLI(命令行界面)工具，这样你就可以使用它来构建、打包和发布你的 Electron 应用。
npm install --save-dev @electron-forge/cli
/*
 迁移项目的核心步骤
 - 扫描你的项目(读取 package.json 等配置文件)
 - 自动配置 Electron Forge  所需的所有文件和设置
 - 更新 package.json,添加 Forge 相关的脚本和配置
 - 创建必要的配置寄文件(如 forge.config.js)
*/
npx electron-forge import
```

运行打包命令后会在根目录生成一个 out 文件夹：
如果要分发给用户，使用 out/make/squirrel.windows/x64/ 中的 Setup.exe

| 文件夹 | 类型 | 用途 | 适用场景 |
| --- | --- | --- | --- |
| my-electron-app-win32-x64 | 应用程序文件夹 | 直接运行的应用 | 内部测试、开发者使用 |
| make | 安装程序 | 分发给用户的安装包 | 正式发布、最终用户使用 |
