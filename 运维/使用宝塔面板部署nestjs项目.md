# 使用宝塔面板部署nestjs项目
## 1.部署mysql
1. 在宝塔面板中的软件商城安装 mysql
![安装mysql](https://cdn.nlark.com/yuque/0/2025/png/2488285/1756227665152-a0625b59-c7fb-415a-9ed6-cc213b78c3ec.png?x-oss-process=image%2Fformat%2Cwebp)
2. 添加数据库
![添加数据库](https://cdn.nlark.com/yuque/0/2025/png/2488285/1756227984065-a6101433-8676-4b40-8f61-4c9ec57299c7.png?x-oss-process=image%2Fformat%2Cwebp)
3. 宝塔面板中开放数据库(默认端口 8090)
![开放端口](https://cdn.nlark.com/yuque/0/2025/png/2488285/1756228201950-e7139293-d337-4942-87d3-62db92f73d51.png?x-oss-process=image%2Fformat%2Cwebp)
4. 使用本地电脑的 Navicat 访问远程数据库
![访问远程数据库](https://cdn.nlark.com/yuque/0/2025/png/2488285/1756228121389-d00f35ca-f496-4272-9d9d-473e00652f73.png?x-oss-process=image%2Fformat%2Cwebp)

## 2.部署nestjs项目
### 2.1 安装nodejs版本管理器和node
点击宝塔面板中的 软件商城 然后搜索 nodejs版本管理器 安装即可
![安装nodeb版本管理器](https://cdn.nlark.com/yuque/0/2025/png/2488285/1756394827624-83ccbb5a-8364-4f40-978c-ff371c6776c5.png?x-oss-process=image%2Fformat%2Cwebp)

点击nodejs版本管理器弹出弹窗安装需要的版本nodejs
![安装node](https://cdn.nlark.com/yuque/0/2025/png/2488285/1756233478503-d346c3e0-d248-4a84-817a-c386176db64f.png?x-oss-process=image%2Fformat%2Cwebp) 
验证：在宝塔中随便找一个终端 输入 node --version 有无输出版本信息即可验证
注意：安装版本后需要在上图中选择命令行版本，否则就算你安装了node上面没有选择node也是在终端找不到node的

### 2.2 部署nestjs项目