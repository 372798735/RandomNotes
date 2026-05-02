# Java Web项目结构

## Java Web项目结构 解析
![Java Web项目结构](./static/java-web项目结构.png)

核心目录说明：
|   目录/文件   |    作用    |
|---------------|------------|
| src/ | 存放所有Java源码和配置文件（最重要） |
| build/| Gradle编译后的输出目录(.class文件、打包的jar/war) |
| .idea/| intelliJ IDEA的项目配置文件（IDE专用） |
| gradle/| Gradle Wrapper的文件夹，包含 JAR 和配置文件 |
| sql/| 数据库脚本文件（建表、初始化数据等） |
| image/| 可能存放图片资源或Docker镜像相关文件 |
| log/和logs.dir_IS_UNDEFINED| 日志文件存放目录 |
| svr-gdfwmk-internal/| 可能是子模块或生成的应用目录 |

关键配置文件
|   文件   |    作用    |
|---------------|------------|
| build.gradle | Gradle构建脚本(依赖管理、插件配置) |
| common.gradle | 公共Gradle配置（可能被多个模块共享） |
| gradle.properties | Gradle属性配置（JVM参数、版本等） |
| gradlew/gradlew.bat | Gradle Wrapper脚本（跨平台执行 Gradle 命令） |
| nginx.conf | Nginx配置文件(可能用于反向代理或静态资源) |

![Java源码目录](./static/java源码目录.png)
|   文件   |    作用    |
|---------------|------------|
| java | java源码 |
| resources | 配置文件 |
| webapp | 前端资源 |
| test/java | 单元测试 |


## spring Boot 项目架构核心概念解析
### 代码解析
项目主入口文件 SpringBootStarter.java：
```java
// 包声明 表示这个类放在 com/gdfw目录下 简单记：package就是告诉Java"这个文件放在哪个文件夹里"
package com.gdfw; 
// 各种依赖
```
### java运行依赖环境是什么
1. 最核心的就是 JDK（Java Development Kit）
```java
JDK 
   |---JRE
   |    |--JVM
   |    |--核心类库
   |----开发工具（javac，jar 等）
```
|   组件   |    作用    |    类比前端的    |
|---------------|------------|------------|
| JDK | 开发和运行Java程序的环境 | Node.js |
| JVM | 运行Java字节码的虚拟机(JDK已包含) | Chrome的V8引擎 |
| JRE | 只运行不开发时用(JDK已包含) | 类似精简版Node |
| javac | 将 .java源码编译成 .class字节码 | tsc编译器 |
| java | 启动JVM运行 .class 或 .jar 文件 | node(运行JS文件) |
| jar | 将多个 .class 文件打包成 .jar 压缩包 | npm run build + 打包工具(webpack/vite) |
| javadoc | 从源码注释生成API文档 | jsdoc/typedoc |
### java项目跑起来的完整流程
写代码(.java) => 编译(.class) => 打包(.jar) => 运行(JVM执行)
### 什么是 spring Boot
Spring Boot 是一个基于 Java 的后端框架，用来快速构建 Web 应用程序和微服务。你可以把它理解为 Java 后端"Express.js/Nest.js"
### 什么是是 gradle,java构建工具
Gradle是什么？
Gradle是java世界的自动化构建工具，负责管理项目的编译、测试、打包、依赖管理等全流程，手动管理这些java项目运行非常麻烦，Gradle帮你一键搞定
Java主要有两个构建工具：
|   对比   |    Maven    |    Gradle    |
|---------------|------------|------------|
| 配置文件 | pom.xml（xml格式） | build.gradle（Groovy/Kotlin格式） |
| 语法 | 臃肿，但规范 | 简洁，灵活 |
| 性能 | 较慢 | 更快(增量编译) |
| 学习曲线 | 平缓 | 稍陡 |
| 市场占比 | 传统企业多 | Android/Spring 生态流行 |
### 前端 vs Java对照表
|   前端概念   |    Java对应概念    |
|---------------|------------|
| Node.js | JDK |
| npm | Gradle/Maven |
| package.json | build.gradle 或 pom.xml |
| node_modules | Maven本地仓库(~/.m2) |
| .js文件       | .java文件 |
| 浏览器/v8引擎 | JVM |
| npm run build | ./gradlew build |
| node server.js | java -jar app.jar |
| 依赖安装 | 依赖自动下载(通过 Gradle/Maven) |

## 基础语法学习
### 对照javascript写一个java版的语法(核心语法)


## 项目实战
### 如何实现一个功能接口
### 每次改项目代码都要重启项目吗

