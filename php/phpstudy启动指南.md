# phpstudy 启动 ThinkPHP 项目指南

## 启动总览
1. 下载 phpstudy 软件开启 mysql和nginx
2. phpstudy 的软件管理 打开 phpAdmin 登陆数据库 账号：root 密码：root 然后新建数据库并导入sql文件
## 📦 项目信息
- **项目类型**: ThinkPHP 5 + FastAdmin
- **项目路径**: `D:/wai_bao/chao_wan_you_xi/php-server`
- **Web目录**: `D:/wai_bao/chao_wan_you_xi/php-server/public`
- **PHP要求**: >= 7.2.0

---

## 📚 技术栈说明

### ThinkPHP 5

**简介**  
ThinkPHP 5 是一个快速、兼容且简单的轻量级国产PHP开发框架，遵循Apache2开源协议发布。ThinkPHP 从诞生以来一直秉承简洁实用的设计原则，在保持出色的性能和至简代码的同时，更注重易用性。

**核心特性**

1. **MVC架构**
   - Model（模型）：数据层，负责数据库操作
   - View（视图）：视图层，负责页面展示
   - Controller（控制器）：业务逻辑层，处理请求和响应

2. **路由系统**
   - 支持路由到模块/控制器/操作
   - 支持RESTful规范
   - 支持路由参数和路由分组
   - URL模式：`http://域名/模块/控制器/操作/参数/值`

3. **数据库操作**
   - 原生支持多种数据库：MySQL、PostgreSQL、SQLite、MongoDB等
   - 链式操作：`Db::table('user')->where('id', 1)->find()`
   - 查询构造器，支持复杂查询
   - 支持模型关联（一对一、一对多、多对多）

4. **模板引擎**
   - 内置模板引擎，支持原生PHP模板
   - 支持模板继承和布局
   - 内置标签：`{$变量}`、`{foreach}` 等
   - 支持第三方模板引擎（Smarty、Blade等）

5. **安全特性**
   - 表单令牌验证（CSRF防护）
   - XSS过滤
   - SQL注入防护
   - 输入变量过滤

**目录结构**
```
thinkphp-project/
├── application/          # 应用目录（核心业务代码）
│   ├── admin/           # 后台模块
│   ├── api/             # API模块
│   ├── index/           # 前台模块
│   ├── common.php       # 公共函数文件
│   ├── config.php       # 应用配置
│   ├── database.php     # 数据库配置
│   └── route.php        # 路由配置
├── public/              # Web入口目录（对外访问目录）
│   ├── index.php        # 入口文件
│   ├── static/          # 静态资源（CSS、JS、图片）
│   └── uploads/         # 上传文件目录
├── runtime/             # 运行时目录（缓存、日志）
│   ├── cache/           # 缓存文件
│   ├── log/             # 日志文件
│   └── temp/            # 临时文件
├── thinkphp/            # 框架核心目录
├── vendor/              # 第三方类库（Composer）
└── composer.json        # Composer依赖配置
```

**常用操作示例**

```php
// 1. 控制器示例
namespace app\index\controller;
use think\Controller;

class Index extends Controller
{
    public function index()
    {
        return $this->fetch(); // 渲染模板
    }
    
    public function hello($name = '')
    {
        return 'Hello, ' . $name;
    }
}

// 2. 数据库查询
use think\Db;

// 查询单条
$user = Db::table('user')->where('id', 1)->find();

// 查询多条
$list = Db::table('user')->where('status', 1)->select();

// 插入数据
Db::table('user')->insert([
    'name' => '张三',
    'email' => 'zhangsan@example.com'
]);

// 更新数据
Db::table('user')->where('id', 1)->update(['name' => '李四']);

// 3. 模型操作
namespace app\index\model;
use think\Model;

class User extends Model
{
    // 查询
    public static function getUserById($id)
    {
        return self::find($id);
    }
    
    // 关联查询
    public function profile()
    {
        return $this->hasOne('Profile');
    }
}
```

**配置文件**
```php
// application/config.php
return [
    // 应用调试模式
    'app_debug' => true,
    
    // 应用Trace
    'app_trace' => false,
    
    // 默认模块名
    'default_module' => 'index',
    
    // URL伪静态后缀
    'url_html_suffix' => 'html',
];

// application/database.php
return [
    'type'     => 'mysql',
    'hostname' => '127.0.0.1',
    'database' => 'database_name',
    'username' => 'root',
    'password' => 'password',
    'hostport' => '3306',
    'charset'  => 'utf8mb4',
    'prefix'   => 'think_',
];
```

---

### FastAdmin

**简介**  
FastAdmin 是一款基于 ThinkPHP 5 + Bootstrap 的极速后台开发框架。它集成了大量常用功能和组件，可以快速构建功能强大的后台管理系统，大大提高开发效率。

**核心特性**

1. **后台管理系统**
   - 基于 Bootstrap 3/4 的响应式后台UI
   - 内置完整的权限管理系统（RBAC）
   - 支持无限级菜单和权限节点
   - 自适应手机、平板、PC

2. **权限管理（RBAC）**
   - **管理员管理**：创建、编辑、删除管理员
   - **角色管理**：定义不同的角色和权限
   - **规则管理**：权限节点的管理
   - **部门管理**：组织架构管理
   - 支持单管理员多角色

3. **一键CRUD**
   - 一键生成增删改查代码
   - 自动生成控制器、模型、视图、JS文件
   - 支持关联表和字段注释
   - 大大减少重复代码编写

4. **前端组件**
   - Bootstrap Table：强大的表格插件
   - SelectPage：下拉选择组件
   - CityPicker：城市选择器
   - Summernote：富文本编辑器
   - Plupload：文件上传组件
   - Layer：弹窗组件
   - Fastadmin-addons：插件扩展机制

5. **常用功能模块**
   - **附件管理**：统一的文件上传和管理
   - **配置管理**：系统配置的可视化管理
   - **日志管理**：操作日志记录和查询
   - **数据字典**：数据字典维护
   - **插件市场**：丰富的插件扩展

**目录结构**
```
fastadmin-project/
├── application/
│   ├── admin/              # 后台模块（核心）
│   │   ├── controller/     # 后台控制器
│   │   ├── model/          # 后台模型
│   │   ├── view/           # 后台视图
│   │   ├── lang/           # 语言包
│   │   └── validate/       # 验证器
│   ├── api/                # API接口模块
│   ├── index/              # 前台模块
│   └── common/             # 公共模块
│       ├── controller/
│       │   ├── Backend.php # 后台基类控制器
│       │   ├── Api.php     # API基类控制器
│       │   └── Frontend.php# 前台基类控制器
│       ├── model/
│       └── library/        # 核心类库
├── public/
│   ├── assets/            # 静态资源
│   │   ├── css/           # 样式文件
│   │   ├── js/            # JS文件
│   │   │   ├── backend/   # 后台JS
│   │   │   └── frontend/  # 前台JS
│   │   ├── img/           # 图片资源
│   │   └── libs/          # 第三方库
│   └── uploads/           # 上传文件
├── addons/                # 插件目录
└── runtime/               # 运行时目录
```

**关键功能说明**

1. **后台登录与认证**
```php
// 访问地址
http://yourdomain.com/admin

// 默认账号密码（根据实际安装配置）
账号: admin
密码: 查看数据库 fa_admin 表
```

2. **权限验证**
```php
// 在控制器中使用
namespace app\admin\controller;
use app\common\controller\Backend;

class Demo extends Backend
{
    // 无需登录的方法
    protected $noNeedLogin = [];
    
    // 无需鉴权的方法
    protected $noNeedRight = ['index'];
    
    public function index()
    {
        // 自动进行权限验证
        return $this->view->fetch();
    }
}
```

3. **一键CRUD使用**
```bash
# 命令行生成CRUD
php think crud -t table_name

# 参数说明
-t  表名（必填）
-c  控制器名称
-m  模型名称
-i  菜单名称
-u  是否生成菜单（默认yes）
-l  是否使用关联（默认no）
```

4. **API接口开发**
```php
// API控制器示例
namespace app\api\controller;
use app\common\controller\Api;

class User extends Api
{
    // 无需登录
    protected $noNeedLogin = ['register'];
    
    // 无需鉴权
    protected $noNeedRight = ['info'];
    
    public function info()
    {
        $user = $this->auth->getUser();
        $this->success('获取成功', $user);
    }
}
```

5. **前端表格配置**
```javascript
// 后台列表页面JS配置
define(['jquery', 'bootstrap', 'backend', 'table', 'form'], function ($, undefined, Backend, Table, Form) {
    var Controller = {
        index: function () {
            // 初始化表格参数配置
            Table.api.init({
                extend: {
                    index_url: 'demo/index',
                    add_url: 'demo/add',
                    edit_url: 'demo/edit',
                    del_url: 'demo/del',
                }
            });
            
            var table = $("#table");
            table.bootstrapTable({
                url: $.fn.bootstrapTable.defaults.extend.index_url,
                columns: [
                    {field: 'id', title: 'ID'},
                    {field: 'name', title: '名称'},
                    {field: 'status', title: '状态'},
                    {field: 'operate', title: '操作'}
                ]
            });
        }
    };
    return Controller;
});
```

**数据表前缀**
```
fa_  （FastAdmin默认表前缀）

核心数据表：
- fa_admin          管理员表
- fa_admin_log      管理员日志表
- fa_auth_group     权限组表
- fa_auth_rule      权限规则表
- fa_config         配置表
- fa_user           会员表
- fa_attachment     附件表
```

**常用功能调用**

```php
// 1. 文件上传
use think\File;
$file = request()->file('file');
$upload = \app\common\library\Upload::instance();
$attachment = $upload->upload($file);

// 2. 发送邮件
\app\common\library\Email::send($to, $subject, $content);

// 3. 发送短信
\app\common\library\Sms::send($mobile, $template, $params);

// 4. 获取配置
$value = config('site.name');
\app\common\model\Config::getConfig('key');

// 5. 获取当前登录用户
$admin = \app\admin\library\Auth::instance()->getUser();
```

**优势总结**

✅ **开箱即用**：内置完整的后台管理功能  
✅ **快速开发**：一键CRUD，减少80%重复代码  
✅ **权限完善**：RBAC权限管理，细粒度控制  
✅ **组件丰富**：集成大量优质前端组件  
✅ **文档完善**：官方文档详细，社区活跃  
✅ **插件生态**：支持插件扩展，功能灵活  
✅ **响应式设计**：自适应各种设备  

**适用场景**

- 企业管理系统
- CRM客户管理
- ERP进销存系统
- 电商后台
- 内容管理系统（CMS）
- API接口开发
- 微信公众号/小程序后台

---

### 技术栈组合优势

**ThinkPHP 5 + FastAdmin** 的组合提供了：

1. **稳定的框架基础** - ThinkPHP 5提供了成熟的MVC架构
2. **快速的后台开发** - FastAdmin大幅提升管理后台开发效率
3. **完善的权限系统** - 开箱即用的RBAC权限管理
4. **丰富的功能组件** - 文件上传、富文本编辑器、图表等
5. **良好的扩展性** - 支持模块化开发和插件扩展
6. **活跃的社区支持** - 国内使用广泛，问题易解决

---

## 🚀 快速启动步骤（新手推荐）

### 第1步：打开 phpstudy

1. 启动 phpstudy（小皮面板）
2. 确保 **Apache/Nginx** 和 **MySQL** 都已启动（绿色状态）

### 第2步：创建站点

#### 方法A：使用网站管理（推荐）

1. 点击左侧 **"网站"** 菜单
2. 点击 **"创建网站"** 按钮
3. 填写配置信息：

```
域名：localhost 或 127.0.0.1 或 自定义域名如 chaowan.test
端口：80 （或其他未占用端口如 8080）
根目录：D:/wai_bao/chao_wan_you_xi/php-server/public  ⚠️ 注意是 public 目录
PHP版本：7.2 或更高（推荐 7.4）
```

4. 点击 **"确认"** 创建

#### 方法B：手动配置（高级）

1. 点击左侧 **"配置文件"** → **"vhosts配置"**
2. 添加以下内容：

```apache
# 如果使用 Apache
<VirtualHost *:80>
    ServerName localhost
    DocumentRoot "D:/wai_bao/chao_wan_you_xi/php-server/public"
    
    <Directory "D:/wai_bao/chao_wan_you_xi/php-server/public">
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

或者（如果使用 Nginx）：

```nginx
server {
    listen 80;
    server_name localhost;
    root D:/wai_bao/chao_wan_you_xi/php-server/public;
    index index.php index.html;
    
    location / {
        if (!-e $request_filename) {
            rewrite ^(.*)$ /index.php?s=/$1 last;
        }
    }
    
    location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

### 第3步：配置伪静态（重要！）

1. 在网站列表中找到刚创建的站点
2. 点击 **"管理"** 或 **"设置"**
3. 点击 **"伪静态"** 选项卡
4. 选择 **"ThinkPHP"** 预设，或手动输入：

```nginx
location / {
    if (!-e $request_filename) {
        rewrite ^(.*)$ /index.php?s=/$1 last;
    }
}
```

5. 点击 **"保存"**

### 第4步：配置 hosts（如果使用自定义域名）

如果您使用了自定义域名（如 `chaowan.test`），需要配置 hosts：

1. 在 phpstudy 中点击 **"系统"** → **"hosts管理"**
2. 添加：
```
127.0.0.1 chaowan.test
```
3. 保存

或手动编辑 `C:\Windows\System32\drivers\etc\hosts` 文件添加上述内容。

###第5步：导入数据库

1. 点击 phpstudy 左侧 **"数据库"** 菜单
2. 点击 **"创建数据库"**
3. 填写数据库信息：
   - 数据库名：`chaowan` 或其他名称
   - 字符集：`utf8mb4`
4. 点击 **"phpMyAdmin"** 或 **"Adminer"** 打开数据库管理
5. 导入 SQL 文件：
   - 选择刚创建的数据库
   - 点击 **"导入"**
   - 选择项目中的 SQL 文件（如 `modify.sql`）
   - 点击执行

### 第6步：配置数据库连接

编辑 `application/database.php` 文件（如果不存在，可能在 `.env` 文件中）：

```php
return [
    // 数据库类型
    'type'            => 'mysql',
    // 服务器地址
    'hostname'        => '127.0.0.1',
    // 数据库名
    'database'        => 'chaowan',  // 改成你创建的数据库名
    // 用户名
    'username'        => 'root',
    // 密码
    'password'        => 'root',     // phpstudy 默认密码是 root
    // 端口
    'hostport'        => '3306',
    // 数据库编码
    'charset'         => 'utf8mb4',
    // 数据库表前缀
    'prefix'          => 'fa_',      // 根据实际情况修改
];
```

### 第7步：设置目录权限

确保以下目录有写权限：

1. 在 phpstudy 中点击 **"网站"** → 找到你的站点
2. 点击 **"权限"** 或直接右键目录设置权限
3. 需要写权限的目录：
   - `runtime/` （必须）
   - `public/uploads/` （上传文件）
   - `application/` （某些情况）

### 第8步：访问项目

在浏览器中访问：

```
http://localhost/
```

或者（如果使用了自定义域名）：
```
http://chaowan.test/
```

---

## ✅ 验证是否成功

### 测试1：访问首页
```
http://localhost/
```
应该能看到前台页面

### 测试2：访问后台
```
http://localhost/admin
```
应该能看到后台登录页面

### 测试3：访问API
```
http://localhost/api/index/index
```
应该返回 JSON 数据

### 测试4：环境检测
```
http://localhost/test_env.php
```
查看环境配置是否正确

---

## 🔧 常见问题解决

### 问题1：访问显示 404

**原因**：伪静态没有配置

**解决**：
1. 确认网站根目录是 `public` 目录
2. 配置伪静态规则（见第3步）
3. 如果使用 Apache，确保开启了 `mod_rewrite` 模块：
   - 点击 phpstudy **"软件管理"** → **"Apache"** → **"模块管理"**
   - 勾选 `rewrite_module`

### 问题2：页面空白或显示错误

**原因**：PHP 版本不符或扩展缺失

**解决**：
1. 切换 PHP 版本：
   - 点击 phpstudy 首页右侧的 **PHP 版本**
   - 选择 7.2 或更高版本
2. 检查必需扩展是否开启：
   - 点击 **"软件管理"** → **"PHP"** → **"扩展管理"**
   - 确保开启：`pdo_mysql`、`curl`、`gd2`、`openssl`、`mbstring`

### 问题3：数据库连接失败

**解决**：
1. 确认 MySQL 已启动（绿色）
2. 检查 `application/database.php` 配置
3. 测试数据库连接：
   - 打开 phpstudy 的 phpMyAdmin
   - 用户名：`root`
   - 密码：`root`（或在 phpstudy 中查看）

### 问题4：样式丢失/资源404

**原因**：静态资源路径问题

**解决**：
1. 确认访问 `http://localhost/assets/css/xxx.css` 能正常返回
2. 如果不行，检查 `public/assets` 目录是否存在
3. 检查 Nginx/Apache 的静态文件配置

### 问题5：端口被占用

**解决**：
1. 修改端口：
   - 在 phpstudy 首页点击端口号
   - 修改为其他端口（如 8080）
2. 或关闭占用 80 端口的程序：
   ```bash
   # 在命令行中查看占用情况
   netstat -ano | findstr :80
   ```

### 问题6：权限不足/无法写入

**解决**：
1. 右键项目目录 → 属性 → 安全
2. 给予 `Users` 用户组完全控制权限
3. 或在 phpstudy 中点击 **"权限修复"**

---

## 📋 phpstudy 推荐配置

### PHP 配置优化

在 phpstudy 中点击 **"软件管理"** → **"PHP"** → **"配置文件"**，修改 `php.ini`：

```ini
# 上传文件大小限制
upload_max_filesize = 20M
post_max_size = 20M

# 内存限制
memory_limit = 256M

# 执行时间
max_execution_time = 300

# 错误显示（开发环境）
display_errors = On
error_reporting = E_ALL

# 时区
date.timezone = PRC
```

### MySQL 配置优化

点击 **"软件管理"** → **"MySQL"** → **"配置文件"**：

```ini
[mysqld]
# 字符集
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

# 最大连接数
max_connections = 200
```

---

## 🎯 推荐的 phpstudy 版本

- **小皮面板（phpstudy V8）**：推荐，界面友好
- **phpstudy Pro**：功能更强大，适合开发
- **phpstudy 2018**：经典版本，稳定

---

## 📱 使用技巧

### 快速切换 PHP 版本
在 phpstudy 首页直接点击 PHP 版本号即可切换

### 查看网站日志
点击网站 → 管理 → 日志，可以查看访问日志和错误日志

### 快速重启服务
点击首页的 **"重启"** 按钮即可重启 Apache/Nginx 和 MySQL

### 性能监控
在首页可以看到 CPU、内存使用情况

---

## 🔐 安全建议

### 开发环境
- 使用默认配置即可
- 开启错误显示便于调试

### 生产环境
- 修改 MySQL root 密码
- 关闭 PHP 错误显示
- 删除测试文件（如 `test_env.php`）
- 配置防火墙规则

---

## 📚 下一步操作

启动成功后，您可以：

1. **配置后台**
   - 访问：`http://localhost/admin`
   - 使用数据库中的管理员账号登录

2. **查看路由**
   - 阅读：`查看路由.md`
   - 了解所有可用的 API 和页面

3. **开发调试**
   - 使用 `test_env.php` 检测环境
   - 查看 `runtime/log` 目录的日志文件

4. **配置 WebSocket**（如果需要）
   - 启动 workerman：`php think workerman start`
   - 配置 Nginx 反向代理（见 `nginx配置说明.md`）

---

## 🆘 还是不行？

如果按照上述步骤还是无法启动，请提供：

1. phpstudy 版本号
2. PHP 版本
3. 访问时显示的错误信息
4. `runtime/log` 目录下的错误日志
5. 浏览器访问 `http://localhost/test_env.php` 的截图

我会根据具体情况进一步帮您解决！

