# React
## 使用脚手架新建一个项目
```javascript
/**
 * 方式一：不用下载脚手架，命令解释(推荐使用)：
 * npx - Node.js 包执行器：
 *     用于运行npm包而不需要全局安装
 *     会自动下载并执行指定的包
 * create-react-app React官方脚手架工具：
 *     Facebook官方提供的创建 React 应用的命令行工具
 *     自动配置 Webpack、Babel、ESLint 等开发工具
 * -- template typescript 模板参数，
 *     指定使用TypeScript模板
 *     会创建带有 TypeScript 配置的项目
 *     所有文件将使用 .tsx 和 .ts 扩展名
 * hook-test 项目名称：
 *     新创建的React应用的文件夹名称
 *     会在当前目录下创建 hook-test 文件夹
 * 为什么不需要下载脚手架：
 *     npx 会自动下载并执行指定的包
 *    即使你的系统上没有安装 create-react-app, npx 也会临时下载它
 *    执行完成后，临时下载的包会被清理
 */
 npx create-react-app --template typescript hook-test
```