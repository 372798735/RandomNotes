# Cursor AI 编辑器入门教程

## 一、Cursor简介

### 1.1 什么是Cursor？

Cursor是一款真正意义上的AI代码编辑器，基于VSCode开发，集成了强大的AI功能。它不仅仅是一个VSCode插件，而是创造性地构建了一个高效的人机协作编程环境，旨在让开发者获得超凡的生产力。

### 1.2 Cursor的主要特点

- **基于GPT模型**：Cursor集成了强大的AI模型，可能基于GPT-4（官方未明确确认）
- **免费使用**：基础功能免费，但偶尔会出现"Maximum Capacity"提示
- **无需翻墙**：国内可直接使用
- **多语言支持**：支持Python、Java、C#、JavaScript等多种编程语言
- **熟悉的界面**：基于VSCode开发，对VSCode用户友好

## 二、安装与设置

### 2.1 下载安装

1. 访问Cursor官网：[https://www.cursor.com](https://www.cursor.com)
2. 点击"Download"下载适合你操作系统的版本（Windows/Mac/Linux）
3. 安装程序并启动Cursor

### 2.2 初始设置

首次启动Cursor时，你需要：

1. 选择界面主题（深色/浅色）
2. 可选择登录账号（也可跳过，直接使用）
3. 熟悉基本界面布局（与VSCode类似）

## 三、使用技巧
1. git提交信息自动生成，如下图所示点击图标即可生成提交信息：
![生成git提交信息](https://cdn.nlark.com/yuque/0/2025/png/2488285/1753588478803-cc9ff514-b87c-4b05-b505-5c152b5ff093.png?x-oss-process=image%2Fformat%2Cwebp)

2. 常用@命令讲解
在cursor对话框中有个@命令选择
- Files & Folders 引用的文件或者文件夹 
  这两个是最常用的@命令，可以引用项目中的特定文件和文件夹作为上下文，如果要引用文件，直接将文件或文件夹拖拽到窗口会更方便。
- Code代码解释
- Docs 文档解析，可以快速知道这个文档的具体内容，实际开发中很有用。
- git 分析提价记录
- rules 生成代码的规则设置

![@命令](https://cdn.nlark.com/yuque/0/2025/png/2488285/1753593192012-196ba81f-f5d3-40df-9786-c3eaed7e9aae.png?x-oss-process=image%2Fformat%2Cwebp)

3. 设置AI生成代码规则
设置生成代码规则，提高工作效率，如下图所示
![rules](https://cdn.nlark.com/yuque/0/2025/png/2488285/1753590849263-4c944c3b-17ae-482b-93c0-b9d94c9236c8.png?x-oss-process=image%2Fformat%2Cwebp)

生成 typeScript 规则：
```javascript
Description:
TypeScript 开发规范，确保类型安全和代码质量

Globs:
**/*.ts, **/*.tsx

# TypeScript 规则
- 禁用 any 类型，使用 unknown 替代
- 启用 strict 模式
- 使用类型推导减少冗余类型声明
- 公共函数必须包含 JSDoc 注释
- 使用 type 而不是 interface（除了 Props）
```
## 三种AI辅助模式(Agent、Ask、Manual)详解：
| 模式 | 用途 | 特点 | 适用场景 |
|------|------|------|----------|
| **Agent** | 让 AI 主动执行任务，自动完成代码编写、文件操作等 | • AI 可以自主调用工具和函数<br>• 能够自动创建、修改、删除文件<br>• 可以运行终端命令<br>• 适合复杂的多步骤任务 | • 创建新项目<br>• 重构代码<br>• 调试问题<br>• 自动化任务 | 
| **Ask** | 与 AI 进行对话交流，获取建议和解释 | • 纯对话模式，不执行实际操作<br>• AI 提供建议、解释、代码示例<br>• 用户需要手动执行 AI 的建议<br>• 适合学习和咨询 | • 代码问题咨询<br>• 学习新技术<br>• 获取编程建议<br>• 代码审查 |
| **Manual** | 手动控制 AI 的行为，精确指定要执行的操作 | • 用户完全控制 AI 的行为<br>• 可以精确指定要修改的文件和内容<br>• AI 按照用户的指令执行<br>• 适合精确的代码修改 | • 精确的代码修改<br>• 特定文件的编辑<br>• 需要严格控制的操作<br>• 调试特定问题 |


## 四、Auto-Run Mode 全自动模式
之前介绍的Agent模式，Agent会根据你的提示，判断是否要执行命令，如果需要执行命令，会提示你确认。
Auto-Run 模式则进一步，Agent将无需确认就能执行命令和文件操作，朝着“全自动驾驶”又迈进了一步。
使用建议：
1. **谨慎使用**

   * 建议在新项目或简单的 Demo 项目中使用
   * 对于重要的生产项目，可以关闭此模式，防止误删、误改文件或者开启后，每次修改之后都要做下 Review
   * 确保设置了合适的命令白名单/黑名单

2. **监控执行过程**

   * 虽然是自动执行，但要留意 Agent 的操作
   * 如果发现不当操作，及时终止
   * 保持文件的备份

3. **渐进式采用**

   * 先在小项目中尝试
   * 熟悉了工作方式后再在更大的项目中使用
   * 根据项目需求调整配置

## 六、使用限制

### 6.1 免费版限制

- 偶尔会出现"Maximum Capacity"提示<mcreference link="https://blog.csdn.net/2301_79270724/article/details/136071749" index="1">1</mcreference>
- 每月提供200次GPT3.5和50次GPT4.0的使用次数<mcreference link="https://ai-bot.cn/sites/906.html" index="4">4</mcreference>

### 6.2 付费版

- 每月20美金<mcreference link="https://blog.csdn.net/2301_79270724/article/details/136071749" index="1">1</mcreference>
- 移除"Maximum Capacity"限制
- 可能提供更多高级功能和更高的使用配额

## 八、总结

Cursor作为一款强大的AI代码编辑器，通过集成先进的AI功能，显著提升了开发效率。它不仅能帮助你编写和编辑代码，还能解释代码、重构代码、回答问题，成为你编程过程中的智能助手。

无论你是初学者还是经验丰富的开发者，Cursor都能帮助你更快、更高效地完成编程任务。通过熟练掌握本教程中介绍的功能和技巧，你将能够充分发挥Cursor的潜力，提升你的编程体验。

## 九、参考资源

- [Cursor官网](https://www.cursor.com)
- [Cursor功能介绍](https://www.cursor.com/cn/features)
- [Cursor GitHub仓库](https://github.com/getcursor/cursor)