# java基础学习

## 环境配置(Windows)

安装Java JDK：
这句话的意思是在你开始编写和运行Java程序之前，你必须在你的电脑上安装一个必要的软件工具包。
JDK的核心组件包括：

- Javac: Java编译器，将你写的 .java 源代码文件翻译成计算机能理解的 .class 字节码文件。
- Java: Java应用程序启动器。负责启动 Java 虚拟机来运行你的 .class 文件。
- 其它工具: 如调试器、文档生成器等。
使用包管理器安装JDK:
- Windows: 可以使用 Chocolatey 或 Winget 等包管理器来安装 JDK.
- 使用Winget（Window11自带）

```powershell
winget install EclipseAdoptium.Temurin.21.JDK
```

检测是否安装完毕：

```powershell
java --version
javac --version
```

## 安装ide（IntelliJ IDEA）

网上有很多安装教程，这里就不展开了。学习用免费的就行(<https://blog.csdn.net/feng8403000/article/details/134727571>)

## 创建一个java项目(Spring Boot)

使用Spring Initializr 是一个官方提供的网页工具，可以通过图形界面“勾选”你项目需要的组件，然后直接生成一个可运行的项目压缩包。步骤如下：

1. 访问 Spring Initializr 官网: <https://start.spring.io//>
2. 配置项目信息（就像填表格），在网页上，你会看到以下选项：
   -Project： 保持 Maven 不变（这是最常用的项目管理工具）。
   -Language： 保持 Java 不变。
   Spring Boot：选择一个稳定版，建议就用它推荐的版本（如 3.2.5），不要用 SNAPSHOT 或 M1 等测试版。
   Project Metadata：
   - Group： 通常是你公司域名的倒写，例如 com.example。
   - Artifact： 项目的名称，例如 my-spring-boot-app。
   - Name： 项目的显示名称，例如 "My Spring Boot App"。
   - Description： 项目的简单描述，例如 "A simple Spring Boot application"。
   - Package Name： 项目的默认包名，例如 com.example.myspringbootapp。
   - Packaging： 保持 Jar 不变（这是最常用的打包方式）。
   - Java： 选择你安装的 JDK 版本，建议使用最新的 LTS 版本（如 Java 21）。
3. 添加依赖：
   - 点击 "Add Dependencies" 按钮，会打开一个依赖选择器。
   - 在搜索框中输入 Spring Web，然后选中它。这个依赖是构建 Web 应用（包括 RESTful API）所必需的。
   - 你也可以再添加 Spring Boot DevTools，它可以提供热加载等功能，方便开发。

## RESTful API

RESTful API 是一种基于 REST（Representational State Transfer）架构风格设计的 Web API，主要用于客户端和服务器之间的数据交互。

### 核心概念

1. **资源（Resources）**
   - 服务器上的具体数据或业务对象
   - 每个资源都有唯一的 URI（统一资源标识符）
   - 例如：`/users/123` 表示 ID 为 123 的用户

2. **HTTP 方法**
   - **GET**：获取资源（只读操作）
   - **POST**：创建新资源
   - **PUT**：完整更新资源
   - **PATCH**：部分更新资源
   - **DELETE**：删除资源

3. **状态码**
   - **200 OK**：请求成功
   - **201 Created**：资源创建成功
   - **204 No Content**：请求成功但无返回内容
   - **400 Bad Request**：客户端请求错误
   - **404 Not Found**：资源不存在
   - **500 Internal Server Error**：服务器错误

### 特点

- **无状态**：每个请求都包含完整的处理信息
- **客户端-服务器架构**：客户端和服务器分离
- **可缓存**：响应可以被缓存以提高性能
- **统一接口**：使用标准的 HTTP 协议
- **分层系统**：可以包含多个中间层

4. 生成项目：

   - 点击页面下方大大的 "GENERATE" 按钮
   - 浏览器会自动下载一个以你 Artifact 名命名的 .zip 压缩包（例如 my-first-spring-boot-app.zip）。

5. 解压并导入 IDE
6. 在 IntelliJ IDEA 中运行已生成的Spring Boot应用
   - 对于 Spring Boot 项目，这个类通常带有 @SpringBootApplication 注解，并且名字类似 XxxApplication。
   - 在 IDEA 的项目结构中，这个类旁边通常会有一个小小的绿色三角形图标，点击运行就行

## Maven是什么

Java项目的自动化构建和依赖管理工具 - 就像项目的"智能管家"
核心文件：pom.xml:项目的“说明书”和“菜谱”;定义项目信息、依赖库、构建方式

## 什么是 Spring Data JPA

Spring Data JPA 是一个能让你只写接口、不用写实现，就自动生成数据库CRUD操作的神奇框架。
简单说就是：你定义接口，Spring自动实现SQL。
比如你写：User findByName(String name);
Spring就自动生成：SELECT * FROM user WHERE name = ? 并执行。

## 创建一个crud项目

crud的目录结构

  src/main/java/
  ├── personnal/
  │   └── flow_list/
  │       ├── FlowListApplication.java        # 主应用启动类
  │       ├── HomeController.java             # 首页控制器
  │       ├── controller/
  │       │   └── UserController.java          # 用户REST API控制器
  │       ├── entity/
  │       │   └── User.java                   # 用户实体类 (JPA)
  │       ├── repository/
  │       │   └── UserRepository.java         # 用户数据访问层 (Spring Data JPA)
  │       ├── service/
  │       │   └── UserService.java             # 用户服务接口
  │       └── service/impl/
  │           └── UserServiceImpl.java         # 用户服务实现类

### 模块一，实体层-定义数据模型

位置：src/main/java/personnal.flow_list/entity/User.java
作用：这个类是一个实体，它映射到数据库中的一张表，类中的每一个实例对应表中的一行，类的属性对应表的列。

```java
package entity;
import jakarta.persistence.*;

@Entity // 告诉JPA这是一个实体类，需要映射到数据库表
@Table(name = "user") // 指定映射的数据库表名，如果省略则默认使用类名
public class User {

    @Id // 标识这个字段是主键
    @GeneratedValue(strategy = GenerationType.IDENTITY) // 主键生成策略：自增
    private Long id;

    @Column(name = "name", nullable = false, length = 100) // 映射到列，可指定约束
    private String name;

    @Column(unique = true) // 邮箱唯一
    private String email;

    private Integer age; // 不加@Column注解，默认属性名即为列名

    // 必须有一个无参构造函数（JPA规范要求）
    public User() {
    }

    // 带参构造函数，方便创建对象
    public User(String name, String email, Integer age) {
        this.name = name;
        this.email = email;
        this.age = age;
    }

    // Getter 和 Setter 方法 (必不可少，用于框架访问私有属性)
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}
```

### 模块二 数据访问层-仓库接口

位置：src/main/java/personnal.flow_list/repository/UserRepository.java
作用：这是一个Repository接口，它继承了Spring Data JPA提供的JpaRepository，从而自动获得了大量通用的数据库操作方法（如增删改查），你无需编写任何实现代码。

```java
// 定义这个接口在 repository 包中，遵循分层架构
package repository;
// 导入 User 实体类1，这是JPA要操作的实体
import entity.User;
// 这是Spring Data JPA的核心接口
import org.springframework.data.jpa.repository.JpaRepository;
//  注解，标识数据访问组件
import org.springframework.stereotype.Repository;
// 用于返回集合结果
import java.util.List;

@Repository // 标识这是一个数据访问组件，可省略，因为JpaRepository本身已具备
public interface UserRepository extends JpaRepository<User, Long> {
    // 以下都是“魔法”：Spring Data JPA会根据方法名自动生成SQL实现

    // 根据姓名精确查找
    User findByName(String name);

    // 根据邮箱查找
    User findByEmail(String email);

    // 查找年龄大于指定值的用户
    List<User> findByAgeGreaterThan(Integer age);

    // 根据姓名模糊查询
    List<User> findByNameContaining(String keyword);

    // 同时根据姓名和邮箱查询
    User findByNameAndEmail(String name, String email);
}
```

### 模块三 服务层-业务逻辑

位置: src/main/java/personnal.flow_list/service/UserService.java
作用：这是一个服务类，它负责实现业务逻辑。在这个类中，你可以调用Repository接口定义的方法来操作数据库，也可以添加自定义的业务逻辑。

```java
package service;

import entity.User;
import java.util.List;
import java.util.Optional;

public interface UserService {
    // 获取所有用户
    List<User> getAllUsers();

    // 根据ID获取用户
    Optional<User> getUserById(Long id);

    // 创建或更新用户
    User saveUser(User user);

    // 删除用户
    void deleteUser(Long id);

    // 根据姓名查找用户（业务逻辑示例）
    User findUserByName(String name);
}
```

实现类:位置: src/main/java/personnal.flow_list/service/impl/UserServiceImpl.java

```java
package service.impl;

import personnal.flow_list.entity.User;
import personnal.flow_list.repository.UserRepository;
import personnal.flow_list.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service // 标识这是一个服务层的Spring组件，会被自动扫描和管理
public class UserServiceImpl implements UserService {

    // 依赖注入：将UserRepository注入到当前类中
    @Autowired
    private UserRepository userRepository;

    @Override
    public List<User> getAllUsers() {
        // 直接调用JpaRepository提供的现成方法
        return userRepository.findAll();
    }

    @Override
    public Optional<User> getUserById(Long id) {
        return userRepository.findById(id);
    }

    @Override
    public User saveUser(User user) {
        // save方法既可用于新增（id为null时），也可用于更新（id有值时）
        return userRepository.save(user);
    }

    @Override
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }

    @Override
    public User findUserByName(String name) {
        // 调用我们在Repository中自定义的方法
        return userRepository.findByName(name);
    }

    // 在这里你可以添加更复杂的业务逻辑，例如数据验证、计算、调用其他服务等
}
```

### 模块四：接收和响应Web请求

位置: src/main/java/personnal.flow_list/controller/UserController.java
作用: 这是项目的API入口，负责接收前端的HTTP请求，调用服务层处理业务，并返回HTTP响应代码：

```java
package personnal.flow_list.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import personnal.flow_list.entity.User;
import personnal.flow_list.service.UserService;

import java.util.List;
import java.util.Optional;

@RestController // 标识这是一个REST风格的控制器，返回的数据直接写入响应体
@RequestMapping("/api/users") // 为这个控制器设置一个统一的前缀路径
public class UserController {

    @Autowired
    private UserService userService;

    // GET /api/users - 获取所有用户
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    // GET /api/users/1 - 根据ID获取用户
    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        Optional<User> user = userService.getUserById(id);
        // 如果用户存在则返回用户信息和200状态码，不存在则返回404状态码
        return user.map(value -> new ResponseEntity<>(value, HttpStatus.OK))
                .orElseGet(() -> new ResponseEntity<>(HttpStatus.NOT_FOUND));
    }

    // POST /api/users - 创建新用户
    @PostMapping
    public User createUser(@RequestBody User user) { // @RequestBody 将请求的JSON体映射为User对象
        return userService.saveUser(user);
    }

    // PUT /api/users/1 - 更新用户信息
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User userDetails) {
        Optional<User> user = userService.getUserById(id);
        if (user.isPresent()) {
            User existingUser = user.get();
            existingUser.setName(userDetails.getName());
            existingUser.setEmail(userDetails.getEmail());
            existingUser.setAge(userDetails.getAge());
            User updatedUser = userService.saveUser(existingUser);
            return new ResponseEntity<>(updatedUser, HttpStatus.OK);
        } else {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
    }

    // DELETE /api/users/1 - 删除用户
    @DeleteMapping("/{id}")
    public ResponseEntity<HttpStatus> deleteUser(@PathVariable Long id) {
        try {
            userService.deleteUser(id);
            return new ResponseEntity<>(HttpStatus.NO_CONTENT);
        } catch (Exception e) {
            return new ResponseEntity<>(HttpStatus.INTERNAL_SERVER_ERROR);
        }
    }
}
```

### 总结

现在，你已经拥有了一个完整、结构清晰的Spring Boot项目核心模块：

1. 实体层：定义了User类，对应数据库表。
2. 数据访问层：UserRepository接口，提供了数据库操作能力。
3. 服务层：UserService接口和UserServiceImpl实现类，封装业务逻辑。
4. 控制器层：UserController，提供了一整套RESTful API。
