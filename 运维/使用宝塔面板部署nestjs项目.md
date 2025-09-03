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
1. 将node项目上传到服务器
注意不要将 node_modules 包上传上来，因为很大，上传会很慢，线上下载即可
![上传node项目](https://cdn.nlark.com/yuque/0/2025/png/2488285/1756746693588-22294b05-df04-4c3a-9872-6d30cf02abc6.png?x-oss-process=image%2Fformat%2Cwebp)
2. 添加node项目和配置
![添加node项目](https://cdn.nlark.com/yuque/0/2025/png/2488285/1756746136821-1e7aac85-8183-400e-aa57-f1aadd6d0186.png?x-oss-process=image%2Fformat%2Cwebp)
![node项目配置](https://cdn.nlark.com/yuque/0/2025/png/2488285/1756746293740-eb039195-e9e8-4a3c-8e9d-3c81072f707a.png?x-oss-process=image%2Fformat%2Cwebp)
3. nginx代理配置
在新增项目时需要顺便把nginx反向代理给配置了
![nginx反向代理配置](https://cdn.nlark.com/yuque/0/2025/png/2488285/1756746430626-e7c048cd-ccba-437f-b415-4f5509f415cf.png?x-oss-process=image%2Fformat%2Cwebp)

### 2.3线上项目预览效果
至此前后端项目已经部署上线，下图为上线的项目 登陆账号密码是: 18819270610 88888888
![线上项目预览](https://cdn.nlark.com/yuque/0/2025/png/2488285/1756875321224-5657b46d-3f07-496d-9d61-5edf98ce6b11.png?x-oss-process=image%2Fformat%2Cwebp)

## 3. 项目部署注意点
1. 部署的过程中要学会面板的项目启动日志
2. 端口要搞明白：
   - 项目部署需要端口(这个端口属于服务器端口，需要在安全组那里开放)，外部网络才可以访问；
   - 项目启动属于内部端口所以不占用服务器的端口；
   - nginx反向代理的端口是服务器端口，即外网访问部署项目的端口；
3. 本地项目启动请求后端接口需要在vite.config.ts文件中做跨域处理，但是部署到服务器请求接口的时候不会经过这个文件做代理的，
   所以部署到线上需要后端那边做跨域处理





























## 参考资料
[宝塔面板部署前后端项目](https://blog.csdn.net/tsz22300/article/details/142602476):  https://blog.csdn.net/tsz22300/article/details/142602476