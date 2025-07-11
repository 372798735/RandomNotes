# trae如何对接MCP(对接微信自动化MCP),编辑器里面也可以进行微信聊天啦

## 1、trae介绍

trae是一个基于MCP协议的AI应用框架，它可以帮助开发者快速搭建基于AI的应用程序。trae提供了一个简单的接口，用于定义和实现AI应用程序的功能。开发者可以使用trae来对接MCP协议，实现与其他AI应用程序的交互。

## 2、trae对接MCP的步骤

1. 安装trae:首先，需要在本地安装trae。可以从trae的官方网站下载并安装trae。安装完成后，需要配置trae的环境变量，以便在命令行中使用trae命令。

2. 创建trae应用:在本地创建一个trae应用。可以使用trae的命令行工具，创建一个新的trae应用。创建完成后，需要在应用中配置MCP的相关信息，例如MCP的地址、端口、认证信息等。
3. 定义MCP接口:在trae应用中定义MCP接口。可以使用trae的接口定义语言，定义MCP接口的输入和输出。接口定义完成后，需要在应用中实现接口的功能。
4. 实现MCP接口:在trae应用中实现MCP接口。可以使用trae的接口实现语言，实现MCP接口的功能。实现完成后，需要对接口进行测试，确保接口的功能符合预期。
5. 启动trae应用:启动trae应用，使应用能够接收和处理MCP协议的请求。可以使用trae的命令行工具，启动trae应用。启动成功后，trae应用会监听指定的端口，等待MCP协议的请求。
6. 测试MCP接口:使用MCP协议的测试工具，测试trae应用定义的MCP接口。测试工具可以模拟发送MCP协议的请求，测试trae应用的接口功能是否正常。

## 3、以对接微信自动化MCP为例子实现接入一个完成的MCP案例

wxauto-mcp是一个基于微信自动化的MCP服务器(用python编写)，用于与微信客户端交互。 该服务器利用wxauto库实现微信消息的发送和接收功能，使大语言模型能够通过wxauto与微信进行交互。
wxauto-mcp项目地址(https://github.com/barantt/wxauto-mcp)
wxauto项目地址(https://github.com/cluic/wxauto)
wxauto官网(https://plus.wxauto.org/docs/)

### 3.1环境要求：

| 环境   | 版本       |
| ------ | ---------- |
| OS     | windows(10 | 11 | Server2016+) |
| 微信   | 3.9.x      |
| Python | 3.9+       |


### 3.2python版本安装：
根据这篇文章安装即可 [python安装教程](https://juejin.cn/post/7124985102123139086?searchId=20250709235823AA84D915FC010340EB9E)


该项目用的版本管理工具是 python 的 uv，所以要先安装包管理器 uv（这里的前提是你有安装了python）

### 3.3python包管理器 uv 介绍和安装：
uv官网地址(https://docs.astral.sh/uv/getting-started/installation/)
Python包管理工具 uv 是由 Astral(知名工具 Ruff 的开发者)基于Rust开发的新一代工具，旨在革新 Python 生态的依赖管理体验。
核心优势：
1. 性能卓越 ：依赖解析和安装速度远超传统工具（如 pip、Poetry），无缓存时比 pip 快 8 - 10 倍，有缓存时可达 80 - 115 倍
2. 功能集成 ：整合了 Python 项目管理全流程工具，包括包管理、虚拟环境管理、Python 版本管理、依赖锁定和 CLI 工具管理等
3. 兼容性强 ：支持现有 requirements.txt、pyproject.toml 文件，可无缝迁移现有项目，还支持单文件脚本的依赖管理

```
pip install uv
// 查看安装是否完运行命令
uv --version
// 看到如下例子所示就是安装成功了：
uv 0.7.19 (38ee6ec80 2025-07-02)
```
注意：安装完pyhon或者uv后，使用 cmd 可以查看安装成功，但是在 tare 编辑器内置的终端(配置的 git bash)中查看安装不成功，建议重启编辑器或者重启电脑即可。

### 3.4在trae中接入微信聊天（wxauto-mcp）
配置
1. 将MCP项目下载到本地并同步项目的依赖环境
```javascript
git clone https://github.com/barantt/wxauto-mcp.git
cd wxauto-mcp
uv sync
```
2. 手动配置MCP配置
![MCP配置窗口](https://cdn.nlark.com/yuque/0/2025/png/2488285/1752112780179-f3d6bb5c-5f5e-474c-b833-71b654635841.png?x-oss-process=image%2Fformat%2Cwebp)

wxauto-mcp 的 MCP配置，其中 path\\to\\wxauto-mcp 表示你配置的 wxauto-mcp 项目的绝对路径。:
```javascript
"mcpServers": {
  "wxauto-mcp": {
    "command": "uv",
    "args": [
      "--directory",
      "path\\to\\wxauto-mcp",  
      "run",
      "wxauto_mcp.py"
    ]
  }
}
```
如下：配置后显示 可使用 表示配置成功了
![配置后显示 可使用 表示配置成功了](https://cdn.nlark.com/yuque/0/2025/png/2488285/1752116024350-fffe814c-f917-42c6-8eac-b11175bac2ef.png?x-oss-process=image%2Fformat%2Cwebp)

### 3.4 在trae中接入微信聊天（wxauto-mcp）成果演示
![](https://cdn.nlark.com/yuque/0/2025/png/2488285/1752137456188-c88373d8-8780-49d3-94ce-a3f3477e7388.png?x-oss-process=image%2Fformat%2Cwebp)
