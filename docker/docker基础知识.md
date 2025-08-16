## Docker 基础知识（入门到实战速查）

\> 面向日常开发与部署的高频知识点与可复制示例。按需速查即可，用时约 1~2 小时完成入门实践。

### 一、什么是 Docker

*   **容器**：一种轻量级、可移植的运行环境，将应用及其依赖打包在一起。与虚拟机相比，容器共享宿主机内核，启动更快、资源开销更小。
*   **核心价值**：一次构建、处处运行；环境一致性；更快的交付与回滚；更好的隔离与资源控制。

### 二、核心概念

*   **Image（镜像）**：只读模板，包含文件系统与依赖。分层存储（Layered），可复用。
*   **Container（容器）**：镜像的可运行实例，具有可写层。容器应尽量无状态，状态放卷（Volume）。
*   **Dockerfile**：描述如何构建镜像的脚本文件。
*   **Registry**：镜像仓库（如 Docker Hub、私有 Harbor 等）。
*   **Volume / Bind Mount**：持久化与数据共享方案。
*   **Network**：容器间通信。常见网络驱动：bridge、host、none、自定义 bridge。

### 三、安装与环境（Windows 简述）

*   建议使用 **Docker Desktop + WSL2**（更好文件系统性能与兼容性）。
*   将代码放在 WSL2 的 Linux 文件系统内（如 `/home/<user>/project`），避免 `/mnt/c` 读写开销。
*   注意换行符：在 Windows 上请避免 CRLF 导致脚本不可执行，使用 LF。

### 四、常用命令速查

```plaintext
# 版本/信息
docker version
docker info

# 镜像
docker images
docker pull nginx:latest
docker rmi nginx:latest
docker build -t myapp:1.0 .
docker tag myapp:1.0 myrepo/myapp:1.0
docker push myrepo/myapp:1.0
docker save -o myapp.tar myapp:1.0
docker load -i myapp.tar

# 容器生命周期
docker ps                  # 运行中
docker ps -a               # 包含已退出
docker run --name web -d -p 8080:80 nginx:alpine
docker logs -f web
docker exec -it web sh     # 进入容器交互
docker stop web
docker start web
docker restart web
docker rm web              # 删除容器

# 检查与资源
docker inspect web
docker stats               # 资源占用
docker top web             # 进程

# 清理
docker system df
docker system prune -f     # 清理无用数据，谨慎使用
```

### 五、Dockerfile 基础

常用指令与要点：

*   **FROM**：基础镜像（尽量选择体积小的，如 `alpine` 或 **distroless**）
*   **WORKDIR**：设置工作目录
*   **COPY/ADD**：复制文件（`ADD` 会解压本地 tar 并支持 URL，通常优先 `COPY`）
*   **RUN**：构建时执行命令
*   **ENV/ARG**：设置环境变量/构建参数
*   **EXPOSE**：文档化端口（不等于映射端口）
*   **CMD/ENTRYPOINT**：容器启动时命令（`ENTRYPOINT` 更固定，`CMD` 可提供默认参数）
*   **USER**：避免使用 root 运行
*   **HEALTHCHECK**：健康检查

示例一：最小 Node.js 应用（多阶段构建）

```plaintext
# 构建阶段
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
# 如果需要编译：RUN npm run build

# 运行阶段
FROM node:20-alpine
WORKDIR /app
COPY --from=build /app /app
ENV NODE_ENV=production
EXPOSE 3000
USER node
CMD ["node", "server.js"]
```

示例二：最小 Python 应用

```plaintext
FROM python:3.12-alpine
WORKDIR /app
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["python", "app.py"]
```

`.dockerignore` 示例：减少构建上下文体积

```plaintext
node_modules
.git
.DS_Store
dist
build
*.log
```

### 六、网络与端口

*   **bridge（默认）**：容器间可通过服务名（自定义网络）相互访问。
*   **host（Linux）**：直接使用宿主网络（Windows/OSX 下表现不同，慎用）。
*   **none**：无网络。

常用：端口映射与自定义网络

```plaintext
# 端口映射：宿主 8080 -&gt; 容器 80
docker run -d -p 8080:80 nginx

# 自定义网络 + 服务名互通
docker network create app_net
docker run -d --name db --network app_net -e POSTGRES_PASSWORD=pass postgres:16
docker run -d --name api --network app_net myapi:latest
# 在 api 容器内可通过主机名 db:5432 访问数据库
```

### 七、数据持久化：Volume 与 Bind Mount

*   **Volume（推荐）**：由 Docker 管理的卷，跨容器持久化，备份迁移方便。
*   **Bind Mount**：将宿主目录挂载到容器内，适合本地开发热更新。

```plaintext
# Named volume
docker volume create pgdata
docker run -d --name db -v pgdata:/var/lib/postgresql/data postgres:16

# Bind mount（Windows + WSL2 建议挂载 WSL 路径）
docker run -d --name web -p 3000:3000 \
  -v $(pwd):/app \
  node:20-alpine sh -c "cd /app &amp;&amp; npm i &amp;&amp; npm run dev"
```

### 八、Docker Compose 入门

优点：声明式编排多个服务、网络与卷；一条命令启动/停止本地多服务环境。

最小示例：`docker-compose.yml`

```plaintext
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8080:3000"
    environment:
      - NODE_ENV=development
    depends_on:
      - db
  db:
    image: postgres:16
    environment:
      - POSTGRES_PASSWORD=pass
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

常用命令：

```plaintext
docker compose up -d        # 后台启动
docker compose logs -f      # 观察所有服务日志
docker compose ps
docker compose exec web sh  # 进容器
docker compose down         # 停止并清理网络（保留卷）
```

### 九、安全与最佳实践

*   **最小化镜像**：优先 `alpine` 或 **distroless**；移除构建依赖，使用多阶段构建。
*   **非 root 运行**：使用 `USER` 指令或基础镜像内已有用户。
*   **.dockerignore**：避免把源码仓库与构建无关文件打进镜像。
*   **固定版本**：基础镜像、依赖尽量锁定版本，确保可重复构建。
*   **健康检查**：使用 `HEALTHCHECK` 提升可观测性与自愈能力。
*   **配置与密钥**：不要写死在镜像内，用环境变量、Secret 或外部配置管理。
*   **单进程原则**：一个容器只跑一个主进程，避免复杂 init；需要时在镜像中安装 `tini` 处理僵尸进程与信号。
*   **资源限额**：通过 `--memory`、`--cpus` 控制资源，避免“吃满”。

### 十、排错技巧（高频场景）

```plaintext
# 看日志 &amp; 退出码
docker logs <name>
docker inspect <name> --format='{{.State.ExitCode}}'

# 进入容器排查网络/依赖
docker exec -it <name> sh
apk add --no-cache curl   # alpine
apt-get update &amp;&amp; apt-get install -y curl # debian/ubuntu
curl -I http://localhost:3000

# 检查端口是否映射正确
docker ps --format 'table {{.Names}}\t{{.Ports}}'

# 查看网络与容器连接
docker network ls
docker network inspect <network>

# 磁盘空间紧张
docker system df
docker system prune -f
```

Windows/WSL2 常见问题：

*   文件权限与可执行位：确保脚本 `chmod +x`，避免 CRLF 换行。
*   挂载路径性能：优先 WSL 内路径（如 `/home/user/project`），避免 `/mnt/c`。
*   端口占用：确认本机无服务占用同端口；必要时更换宿主映射端口。

### 十一、术语小抄

*   **镜像（Image）**：打包好的“只读文件系统快照”。
*   **容器（Container）**：镜像运行时，带可写层实例。
*   **卷（Volume）**：持久化数据位置，由 Docker 管理。
*   **绑定挂载（Bind Mount）**：宿主机目录直接挂进容器。
*   **编排（Compose/K8s）**：声明式管理多服务。

### 十二、动手练习（建议 30 分钟）

1.  运行 Nginx 并访问欢迎页：

```plaintext
docker run --name web -d -p 8080:80 nginx:alpine
# 浏览器打开 http://localhost:8080
```

1.  为你的应用写一个最小 Dockerfile 并构建：

```plaintext
docker build -t myapp:dev .
docker run --rm -p 3000:3000 myapp:dev
```

1.  用 Compose 同时启动 Web + DB：

```plaintext
docker compose up -d
docker compose logs -f
```

### 参考路径（建议）

*   官方文档（概念与命令速查）
*   Dockerfile 最佳实践
*   Docker Compose 规范

你可以先从“动手练习”开始，遇到问题回到对应章节速查即可。