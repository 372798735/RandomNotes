## 一、Claude Code介绍

Claude Code是Anthropic推出的一个代理式命令行工具，这个工具允许开发者直接从终端将编码任务委托给Claude来完成。通过Claude Code,开发者可以在命令行环境中与Claude进行交互，
让Claude帮助处理各种编程相关的任务。

## 二、windows上安装wsl环境

> WSL（Windows Subsystem for Linux）是微软为Windows系统提供的一个兼容层功能，允许用户直接在Windows上运行Linux环境(无需虚拟机或双系统)，必须运行在Window 10版本2004及更高版本(内部版本19041即以上)或Windows11才能使用以下命令。
> windows查看当前系统版本：
> 1、win+R打开运行窗口
> 2、输入winver查看当前系统版本

> 为什么要安装wsl?
> 很多应用只能在Linux环境下执行，比如最近很火的AI工具，Claude Code 只支持在Linux下的环境下使用

windiws上启用WSL：

在windows电脑上开启wsl功能，在搜索应用中输入：启用或关闭Windows功能，出现如下图，勾选【适用于Linux的Windows子系统】,然后**重启电脑** 就生效了。
![图片](https://cdn.nlark.com/yuque/0/2025/png/2488285/1752339272185-4e73bafc-0699-47f4-ade0-f75fabf4a190.png?x-oss-process=image%2Fformat%2Cwebp)

## 三、windows系统基于 wsl 环境安装linux 系统

1. 在终端上查询是否安装 了linux系统，  运行命令：wsl --list,出现如下就没有安装。
![未安装linux](https://cdn.nlark.com/yuque/0/2025/png/2488285/1752323798515-cc49e84c-f734-4f5a-bedc-b2f07d5bc9b0.png?x-oss-process=image%2Fformat%2Cwebp)

2. 未安装 根据命令安装就行：

```bash
wsl --install -d Ubuntu-22.04
```

3. 安装报错和解决：

```javascript
// 安装linux虚拟机报错如下
Installing, this may take a few minutes...
WslRegisterDistribution failed with error: 0x800701bc
Error: 0x800701bc WSL 2 ?????????????????? https://aka.ms/wsl2kernel
Press any key to continue...

// 原因：报错原因在于可能默认安装了wsl2，解决办法是安装成 wsl1,运行如下命令即可
wsl --set-default-version 1
```

4. 安装成功的提示:

出现如下如表示安装成功了：
![已安装](https://cdn.nlark.com/yuque/0/2025/png/2488285/1752375731858-c29ffb00-7991-42bb-b062-29d148b09518.png?x-oss-process=image%2Fformat%2Cwebp)
在系统界面也可以看到在linux下已经安装了Ubantu系统:
![linux系统](<https://cdn.nlark.com/yuque/0/2025/png/2488285/1752339197971-4deaf592-e6ed-47ec-b914-ae3aa6dddfeb.png?x-oss-process=image%2Fformat%2Cwebp>)

**小tip 终端中快速打开linux系统:**

![终端](https://cdn.nlark.com/yuque/0/2025/png/2488285/1752376186985-11e4bd83-886f-4d18-8ce7-f8ad2a46c31e.png?x-oss-process=image%2Fformat%2Cwebp)

## 四、在wsl环境下安装包nodejs
>
> 安装nodejs的原因：因为ClaudeCOde软件需要这个环境，就跟你安装游戏的时候，需要安装基础环境。

```javascript
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash  // 安装nvm 安装完后需要重启一下终端才能生效
nvm install --lts // 安装nodejs最新稳定版本 
```

## 五、安装Claude Code

1. 使用安装命令(一个命令就可以搞定，这里需要科学上网)

```javascript
npm install -g @anthropic-ai/claude-code
```

2. 验证是否安装成功

```javascript
claude --version
```

3. claude code配置：

```javascript
claude
```

当前这一步配置的时候出错了，我就看看我的ip归属地：

```powershell
# 查看当前IP归属地
curl http://ip-api.com/json | Select-String "country"

# 运行结果和结果解析如下

#{
#  "国家": "美国（United States/US）",
#  "地区": "加利福尼亚州洛杉矶（Los Angeles, CA）",
#  "坐标": "北纬34.0544°，西经118.244°",
#  "网络运营商": "Psychz Networks (AS40676)",
#  "所属机构": "TELUS通信公司",
#  "公网IP": "108.181.23.93"
#}
```
