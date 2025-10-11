# 从0到1开发一款vscode插件
插件名称：[vs-quick-operation](https://marketplace.visualstudio.com/search?term=vs-quick-operation&target=VSCode&category=All%20categories&sortBy=Relevance)</br>
使用快捷键：选中需要粘贴的内容，按快捷键： ctrl+shift+D</br>
工具要求：使用vscode，注意(使用trae/cursor等ai工具可能调试模式下会有问题)
## 功能预览：vscode插件实现快速粘贴复制
![快速粘贴复制](https://cdn.nlark.com/yuque/0/2025/gif/2488285/1753830001145-e0a13906-6a87-4b81-8597-6069842881d3.gif)
## 准备环境和初始化项目
1. 安装脚手架：
```javascript
// yo(Yeoman): 一个强大的脚手架工具，用于快速生成项目模板
// generator-code: 专门用于生成 VS Code 扩展
 npm i  -g yo generator-code 
```
2. 使用脚手架快速创建vscode插件开发模板代码
```javascript
yo code
```
## 开发vsCode插件功能关键文件和代码：
1. package.json
```javascript
{
  "name": "vs-quick-operation",
  "displayName": "vs-quick-operation",
  "description": "exCode extension",
  "publisher": "发布者名称",
  "version": "0.0.1",
  "icon": "image/quickly.png",
  "engines": {
    "vscode": "^1.60.0"
  },
  "categories": [
    "Other"
  ],
  "activationEvents": [
    "onCommand:extension.duplicateLine"
  ],
  "main": "./extension.js",
  "contributes": {
    "commands": [
      {
        "command": "extension.duplicateLine",
        "title": "Duplicate Line"
      }
    ],
    "keybindings": [
      {
        "command": "extension.duplicateLine",
        "key": "ctrl+shift+d",
        "when": "editorTextFocus && !editorHasMultipleSelections"
      }
    ]
  },
  "scripts": {
    "lint": "eslint .",
    "pretest": "npm run lint",
    "test": "node ./test/runTest.js"
  },
  "devDependencies": {
    "@types/vscode": "^1.60.0",
    "@types/glob": "^7.1.3",
    "@types/mocha": "^8.2.3",
    "@types/node": "14.x",
    "eslint": "^7.32.0",
    "glob": "^7.1.7",
    "mocha": "^8.4.0",
    "typescript": "^4.3.5",
    "vscode-test": "^1.5.2"
  }
}
```

## 代码实现
1. main/copyAndPast.js 主要逻辑代码如下：

需要注意的是，vscode插件开发推荐使用 Commonjs 语法，原因：
- 兼容性更好- VS Code 扩展环境对 CommonJS 支持更成熟
- 配置简单- 不需要额外的配置
- 社区标准 -大部分 VS Code 扩展都是用 CommonJS
- Node.js 默认，-Node.js 默认支持CommonJS

```javascript
const vscode = require("vscode");
/** 复制和快速黏贴 */
const copyAndPast = function () {
  /**
   * 获取当前活动编辑器
   * 获取当前焦点所在的文本编辑器
   * 如果没有打开的编辑器或着焦点不在编辑器上，则返回 undefined
   */
  const editor = vscode.window.activeTextEditor;
  if (!editor) {
    return;
  }

  /**
   * 获取当前活动编辑器的文档
   */
  const document = editor.document;
  /**
   * 获取当前活动编辑器的选区
   */
  const selection = editor.selection;

  // 获取选中的文本
  const selectedText = document.getText(selection);

  if (selectedText) {
    // 如果有选中的文本，复制到下一行

    /** 获取选择区域的结束位置对象，包含  line(行号) 和 character(列号) */
    const endPosition = selection.end;
    /**创建新的位置对象  endPosition.line + 1 表示在结束位置的下一行，0 表示在下一行的第0列 */
    const insertPosition = new vscode.Position(endPosition.line + 1, 0);

    /**开始一个编辑操作，所有的修改都在这个回调中进行 */
    editor.edit((editBuilder) => {
      /**在指定位置插入文本  
       * selectedText + "\n" 表示插入选中的文本并在末尾添加换行符
      */
      editBuilder.insert(insertPosition, selectedText + "\n");
    });
  } else {
    // 如果没有选中文本，复制当前行到下一行

    /** 获取光标当前所在的行号 */
    const currentLine = selection.active.line;
    /**获取当前行号所在的行文本 */
    const lineText = document.lineAt(currentLine).text;
    /**创建新的位置对象  currentLine + 1 表示在当前行的下一行，0 表示在下一行的第0列 */
    const insertPosition = new vscode.Position(currentLine + 1, 0);

    editor.edit((editBuilder) => {
      /**在指定位置插入文本  
       * lineText + "\n" 表示插入选中的文本并在末尾添加换行符
      */
      editBuilder.insert(insertPosition, lineText + "\n");
    });
  }
}

module.exports = copyAndPast;

```
2. extension.js（代码执行入口）
```javascript
const vscode = require("vscode");
const copyAndPast = require("./main/copyAndPast");

  /**
   * 激活插件时调用
   * @param {vscode.ExtensionContext} context
   */
function activate(context) {
  console.log('===== VS Quick Operation 扩展已启动 =====');
  console.log('扩展名称:', 'vs-quick-operation');
  console.log('版本:', '0.0.1');
  console.log('启动时间:', new Date().toLocaleString());
  console.log('=========================================');
    // 注册命令
    let disposable = vscode.commands.registerCommand(
      /**注册了一个名为extension.duplicateLine的命令 
       *  这个命令对应 package.json 中的 "extension.duplicateLine"
      */
      "extension.duplicateLine",
      copyAndPast
    );

    /** 将 disposable 添加到上下文订阅中，确保插件在停止时被正确卸载 */
    context.subscriptions.push(disposable);
  }

  /**
   * 插件停用时调用
   */
  function deactivate() {}

  // 导出
  module.exports = {
    activate,
    deactivate,
  };

```
## 本地运行和代码调试
直接运行 Run and Debug 就会起一个调试的 vscode 窗口，如图所示：

![run and debug](https://cdn.nlark.com/yuque/0/2025/png/2488285/1753828588876-4f3a376b-badb-4431-bfa0-0bb9818cee81.png?x-oss-process=image%2Fformat%2Cwebp)
## 打包插件
1. 全局安装打包插件
```javascript
// 安装插件
npm install vsce -g
// 查看安装成功与否
打包的 .vsix 文件可以直接安装到 vscode 当中，直接将打包的插件拖拽安装即可。vsce --version
```
1. 调用命令就能完成打包，打包之后的文件格式 .vsix
```javascript
vsce package
console.log(vsce package)
vsce package
```
1. 安装本地插件：
   

## 发布插件到应用市场
1. 注册microsoft账号，官网地址：https://marketplace.visualstudio.com/
2. 在开发者平台新建项目 https://dev.azure.com/   这里主要是生成token,用于发布插件
   注意：需要将生成的token保存起来，不然每次都要重新生成token（token过期了就没有办法）:
   token: [你的Azure DevOps Personal Access Token]
   这个是获取token的直接地址：https://dev.azure.com/18819270610/_usersSettings/tokens
   token过期或者忘记了记得重新生成
   如下图片所示：
   ![token获取](https://cdn.nlark.com/yuque/0/2025/png/2488285/1758968946187-1e740f67-d1c0-4da1-bc6e-eb7a20f93797.png?x-oss-process=image%2Fformat%2Cwebp)
3. 创建发布者，这里会将你的发布插件和这个创建发布者关联， https://link.juejin.cn/?target=https%3A%2F%2Faka.ms%2Fvscode-create-ublispher
4. 在终端输入如下命令 登陆发布者账号，然后他会提示你输入token，将2步生成的token粘贴上去就可以了
```javascript
vsce login "发布者名称" // 在第三步创建发布者名称
```
5. 发布上线
这里有个前提你的 package.json 文件里面要加上发布者名称(第三步创建的发布者名称)，如下代码示例：
```javascript
"publisher":"发布者名称"
```
配置完后将插件发布上线：
```javascript
vsce publish 0.01 // 这里的 0.01是版本号，要和package.json里面配置的版本号要一一致
```
6. 发布完插件几分钟就可以在[插件市场](https://marketplace.visualstudio.com/search?term=%E8%82%A1%E7%A5%A8&target=VSCode&category=All%20categories&sortBy=Relevance)中看到：
7. 插件官方地址：**汇添富中****汇添富中**
注意：像 cursor 等ai编程工具可能不一定搜的到，因为cursor6月25号开始将插件市场改成了openvsx，具体发布可参考这篇文章([在Cursor中搜不到Vs Code插件解决方案](https://aicoding.juejin.cn/post/7522057991949303827))，本文不再赘述.
![vscode插件](https://cdn.nlark.com/yuque/0/2025/png/2488285/1753828988657-9942888b-f5cf-4f85-983c-8d5c0223b09d.png?x-oss-process=image%2Fformat%2Cwebp)

## 参考文章链接
[从0到1开发一款自己的vscode插件]https://juejin.cn/post/7010765441144455199?from=search-suggest<br/>
[试开发一款vscode插件（上）]https://juejin.cn/post/7151062725517377549?searchId=202507261947484D4E3FC48B683C7D9548<br/>
[试开发一款vscode插件（下）]https://juejin.cn/post/7151821927701544996?from=search-suggest<br/>
[vscode插件开发源码链接]https://github.com/372798735/vs-quick-operation</br>
