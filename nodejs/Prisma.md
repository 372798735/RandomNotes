# Prisma 技术文档

## 一、什么是 Prisma？

Prisma 是一个现代的、类型安全的 Node.js 和 TypeScript ORM（对象关系映射）框架。它提供了一种全新的方式来处理数据库操作，让开发者能够用更直观、更安全的方式与数据库交互。

**官方网站**：https://www.prisma.io/

### 1.1 核心理念

- **类型安全**：自动生成类型定义，在编译时就能发现错误
- **自动完成**：IDE 能够提供完整的智能提示
- **声明式数据建模**：使用简洁的 Schema 语言定义数据模型
- **数据库迁移**：自动生成和管理数据库迁移文件

---

## 二、Prisma 的核心组件

### 2.1 Prisma Schema

数据模型定义文件（通常是 `schema.prisma`），用于定义：
- 数据源（数据库连接）
- 数据模型（表结构）
- 生成器配置

### 2.2 Prisma Client

自动生成的、类型安全的数据库客户端，提供：
- CRUD 操作
- 关联查询
- 事务处理
- 原始 SQL 查询

### 2.3 Prisma Migrate

数据库迁移工具，用于：
- 创建数据库迁移
- 应用迁移到数据库
- 管理数据库版本

### 2.4 Prisma Studio

可视化数据库管理工具（GUI），用于：
- 查看和编辑数据
- 可视化数据关系
- 快速测试查询

---

## 三、支持的数据库

Prisma 支持多种主流数据库：

- **PostgreSQL** ✅
- **MySQL** ✅
- **SQLite** ✅
- **SQL Server** ✅
- **MongoDB** ✅ （预览版）
- **CockroachDB** ✅

---

## 四、安装和配置

### 4.1 初始化项目

```bash
# 创建新项目
mkdir my-prisma-app
cd my-prisma-app

# 初始化 Node.js 项目
npm init -y

# 安装 TypeScript（推荐）
npm install typescript ts-node @types/node --save-dev

# 初始化 TypeScript
npx tsc --init
```

### 4.2 安装 Prisma

```bash
# 安装 Prisma CLI（开发依赖）
npm install prisma --save-dev

# 安装 Prisma Client（生产依赖）
npm install @prisma/client
```

### 4.3 初始化 Prisma

```bash
# 创建 Prisma Schema 文件
npx prisma init

# 如果使用 SQLite（用于快速测试）
npx prisma init --datasource-provider sqlite
```

执行后会生成：
- `prisma/schema.prisma` - Schema 文件
- `.env` - 环境变量文件（存储数据库连接字符串）

---

## 五、基本使用示例

### 5.1 定义数据模型

编辑 `prisma/schema.prisma`：

```prisma
// 数据源配置
datasource db {
  provider = "postgresql"  // 或 mysql, sqlite 等
  url      = env("DATABASE_URL")
}

// 生成器配置
generator client {
  provider = "prisma-client-js"
}

// 用户模型
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String?
  password  String
  posts     Post[]   // 一对多关系
  profile   Profile? // 一对一关系
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

// 文章模型
model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String?
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
  categories Category[] // 多对多关系
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

// 用户资料模型（一对一）
model Profile {
  id     Int     @id @default(autoincrement())
  bio    String?
  avatar String?
  user   User    @relation(fields: [userId], references: [id])
  userId Int     @unique
}

// 分类模型（多对多）
model Category {
  id    Int    @id @default(autoincrement())
  name  String @unique
  posts Post[]
}
```

### 5.2 配置数据库连接

在 `.env` 文件中设置数据库连接字符串：

```env
# PostgreSQL
DATABASE_URL="postgresql://username:password@localhost:5432/mydb?schema=public"

# MySQL
DATABASE_URL="mysql://username:password@localhost:3306/mydb"

# SQLite（用于开发测试）
DATABASE_URL="file:./dev.db"
```

### 5.3 创建数据库迁移

```bash
# 创建迁移文件  生成数据库表 迁移 = 在数据库造房子
npx prisma migrate dev --name init

# 这个命令会：
# 1. 创建迁移文件
# 2. 应用迁移到数据库
# 3. 生成 Prisma Client
```

### 5.4 生成 Prisma Client

```bash
# 手动生成客户端（通常在迁移后自动执行）根据 schema.prisma 生成操作数据库的代码(生成增删改查的代码)
npx prisma generate
```

---

## 六、常用 CRUD 操作

### 6.1 初始化 Prisma Client

创建 `src/index.ts`：

```typescript
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()

// 程序退出时断开连接
process.on('beforeExit', async () => {
  await prisma.$disconnect()
})
```

### 6.2 创建数据（Create）

```typescript
// 创建单个用户
const user = await prisma.user.create({
  data: {
    email: 'alice@example.com',
    name: 'Alice',
    password: 'hashed_password',
  },
})

// 创建用户的同时创建关联数据
const userWithPost = await prisma.user.create({
  data: {
    email: 'bob@example.com',
    name: 'Bob',
    password: 'hashed_password',
    posts: {
      create: [
        {
          title: 'My first post',
          content: 'Hello World!',
        },
      ],
    },
    profile: {
      create: {
        bio: 'I love coding!',
      },
    },
  },
  include: {
    posts: true,
    profile: true,
  },
})

// 批量创建
const users = await prisma.user.createMany({
  data: [
    { email: 'user1@example.com', name: 'User 1', password: 'pass1' },
    { email: 'user2@example.com', name: 'User 2', password: 'pass2' },
  ],
})
```

### 6.3 查询数据（Read）

```typescript
// 查询所有用户
const allUsers = await prisma.user.findMany()

// 查询第一个用户
const firstUser = await prisma.user.findFirst()

// 根据唯一字段查询
const user = await prisma.user.findUnique({
  where: { email: 'alice@example.com' },
})

// 根据 ID 查询（仅当 ID 字段唯一时）
const userById = await prisma.user.findUnique({
  where: { id: 1 },
})

// 条件查询
const filteredUsers = await prisma.user.findMany({
  where: {
    email: {
      contains: '@example.com',  // 包含
    },
    name: {
      startsWith: 'A',  // 以...开头
    },
  },
  orderBy: {
    createdAt: 'desc',  // 排序
  },
  take: 10,  // 限制数量
  skip: 0,   // 跳过数量（分页）
})

// 包含关联数据
const userWithRelations = await prisma.user.findUnique({
  where: { id: 1 },
  include: {
    posts: true,    // 包含文章
    profile: true,  // 包含资料
  },
})

// 选择特定字段
const userEmails = await prisma.user.findMany({
  select: {
    email: true,
    name: true,
  },
})

// 聚合查询
const userCount = await prisma.user.count()
const avgAge = await prisma.user.aggregate({
  _avg: {
    id: true,
  },
})
```

### 6.4 更新数据（Update）

```typescript
// 更新单个记录
const updatedUser = await prisma.user.update({
  where: { id: 1 },
  data: {
    name: 'Alice Updated',
  },
})

// 更新或创建（upsert）
const upsertUser = await prisma.user.upsert({
  where: { email: 'charlie@example.com' },
  update: {
    name: 'Charlie Updated',
  },
  create: {
    email: 'charlie@example.com',
    name: 'Charlie',
    password: 'hashed_password',
  },
})

// 批量更新
const updateMany = await prisma.user.updateMany({
  where: {
    email: {
      contains: '@example.com',
    },
  },
  data: {
    name: 'Updated Name',
  },
})

// 更新关联数据
const updateWithRelations = await prisma.user.update({
  where: { id: 1 },
  data: {
    posts: {
      create: {
        title: 'New Post',
      },
    },
  },
})
```

### 6.5 删除数据（Delete）

```typescript
// 删除单个记录
const deletedUser = await prisma.user.delete({
  where: { id: 1 },
})

// 批量删除
const deletedUsers = await prisma.user.deleteMany({
  where: {
    email: {
      contains: 'test',
    },
  },
})

// 删除所有记录
const deleteAll = await prisma.user.deleteMany({})
```

---

## 七、高级功能

### 7.1 事务处理

```typescript
// 方式 1：交互式事务
const result = await prisma.$transaction(async (tx) => {
  const user = await tx.user.create({
    data: {
      email: 'transaction@example.com',
      name: 'Transaction User',
      password: 'pass',
    },
  })

  const post = await tx.post.create({
    data: {
      title: 'Transaction Post',
      authorId: user.id,
    },
  })

  return { user, post }
})

// 方式 2：批量事务
const [users, posts] = await prisma.$transaction([
  prisma.user.findMany(),
  prisma.post.findMany(),
])
```

### 7.2 原始 SQL 查询

```typescript
// 原始查询
const users = await prisma.$queryRaw`
  SELECT * FROM "User" WHERE "email" LIKE ${'%@example.com'}
`

// 原始执行（不返回结果）
const result = await prisma.$executeRaw`
  UPDATE "User" SET "name" = 'Updated' WHERE "id" = ${1}
`
```

### 7.3 中间件

```typescript
// 添加日志中间件
prisma.$use(async (params, next) => {
  console.log(`Query: ${params.model}.${params.action}`)
  console.log(`Params:`, params.args)
  
  const before = Date.now()
  const result = await next(params)
  const after = Date.now()
  
  console.log(`Duration: ${after - before}ms`)
  
  return result
})
```

---

## 八、常用命令

```bash
# 初始化 Prisma
npx prisma init

# 创建迁移（开发环境）
npx prisma migrate dev --name migration_name

# 应用迁移（生产环境）
npx prisma migrate deploy

# 重置数据库（清空所有数据并重新应用迁移）
npx prisma migrate reset

# 生成 Prisma Client
npx prisma generate

# 打开 Prisma Studio（可视化管理工具）
npx prisma studio

# 格式化 Schema 文件
npx prisma format

# 验证 Schema 文件
npx prisma validate

# 从现有数据库拉取 Schema（数据库优先）
npx prisma db pull

# 将 Schema 推送到数据库（不创建迁移）
npx prisma db push

# 查看数据库状态
npx prisma migrate status
```

---

## 九、与其他 ORM 对比

| 特性 | Prisma | TypeORM | Sequelize |
|------|--------|---------|-----------|
| 类型安全 | ✅ 完全类型安全 | ⚠️ 部分支持 | ❌ 需要手动定义 |
| 学习曲线 | 🟢 较低 | 🟡 中等 | 🟡 中等 |
| 迁移工具 | ✅ 内置 | ✅ 内置 | ✅ 内置 |
| 查询性能 | 🚀 优秀 | 🟢 良好 | 🟢 良好 |
| 可视化工具 | ✅ Prisma Studio | ❌ 无 | ❌ 无 |
| 自动完成 | ✅ 完整 | ⚠️ 部分 | ❌ 较差 |
| 数据建模 | 声明式 Schema | 装饰器 | 模型类 |
| 社区活跃度 | 🔥 非常活跃 | 🔥 活跃 | 🟡 逐渐减少 |

---

## 十、优势与劣势

### 10.1 优势

✅ **类型安全**：完全的 TypeScript 支持，编译时发现错误  
✅ **自动生成**：自动生成 Client 和类型定义  
✅ **简洁易用**：API 设计直观，学习曲线低  
✅ **性能优秀**：查询优化和批处理  
✅ **迁移管理**：强大的数据库迁移工具  
✅ **可视化工具**：内置 Prisma Studio  
✅ **文档完善**：官方文档详细且易懂  
✅ **多数据库支持**：轻松切换数据库

### 10.2 劣势

❌ **相对年轻**：生态系统不如老牌 ORM 成熟  
❌ **学习新语法**：需要学习 Prisma Schema 语法  
❌ **生成代码体积**：生成的 Client 文件较大  
❌ **某些复杂查询**：非常复杂的查询可能需要原始 SQL  
❌ **MongoDB 支持**：MongoDB 支持仍在预览阶段

---

## 十一、实战项目结构示例

```
my-app/
├── prisma/
│   ├── schema.prisma          # 数据模型定义
│   ├── migrations/            # 迁移文件
│   └── seed.ts                # 种子数据
├── src/
│   ├── index.ts               # 入口文件
│   ├── prisma.ts              # Prisma Client 实例
│   ├── services/              # 业务逻辑层
│   │   ├── user.service.ts
│   │   └── post.service.ts
│   ├── controllers/           # 控制器层
│   │   ├── user.controller.ts
│   │   └── post.controller.ts
│   └── routes/                # 路由层
│       ├── user.routes.ts
│       └── post.routes.ts
├── .env                       # 环境变量
├── package.json
└── tsconfig.json
```

---

## 十二、最佳实践

### 12.1 使用单例模式管理 Prisma Client

创建 `src/prisma.ts`：

```typescript
import { PrismaClient } from '@prisma/client'

// 全局类型声明
declare global {
  var prisma: PrismaClient | undefined
}

// 开发环境使用全局变量避免热重载创建多个实例
const prisma = global.prisma || new PrismaClient()

if (process.env.NODE_ENV !== 'production') {
  global.prisma = prisma
}

export default prisma
```

### 12.2 错误处理

```typescript
import { Prisma } from '@prisma/client'

try {
  const user = await prisma.user.create({
    data: {
      email: 'duplicate@example.com',
      name: 'Test',
      password: 'pass',
    },
  })
} catch (error) {
  if (error instanceof Prisma.PrismaClientKnownRequestError) {
    // 唯一约束冲突
    if (error.code === 'P2002') {
      console.log('Email already exists')
    }
  }
  throw error
}
```

### 12.3 使用种子数据

创建 `prisma/seed.ts`：

```typescript
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()

async function main() {
  // 创建测试用户
  const alice = await prisma.user.upsert({
    where: { email: 'alice@test.com' },
    update: {},
    create: {
      email: 'alice@test.com',
      name: 'Alice',
      password: 'hashed_password',
      posts: {
        create: [
          {
            title: 'First Post',
            content: 'Content of first post',
            published: true,
          },
        ],
      },
    },
  })

  console.log({ alice })
}

main()
  .catch((e) => {
    console.error(e)
    process.exit(1)
  })
  .finally(async () => {
    await prisma.$disconnect()
  })
```

在 `package.json` 中添加：

```json
{
  "prisma": {
    "seed": "ts-node prisma/seed.ts"
  }
}
```

运行种子数据：

```bash
npx prisma db seed
```

---

## 十三、参考资源

- 📚 **官方文档**：https://www.prisma.io/docs
- 📖 **入门教程**：https://www.prisma.io/docs/getting-started
- 🎓 **Prisma 示例**：https://github.com/prisma/prisma-examples
- 💬 **社区论坛**：https://github.com/prisma/prisma/discussions
- 📺 **视频教程**：https://www.youtube.com/c/Prisma

---

## 十四、总结

Prisma 是一个现代化的、开发者友好的 ORM 框架，特别适合以下场景：

- ✅ TypeScript 项目
- ✅ 需要类型安全的项目
- ✅ 快速开发和原型设计
- ✅ 团队协作项目
- ✅ 需要可视化数据库管理的项目

如果你正在开始一个新的 Node.js/TypeScript 项目，Prisma 是一个非常值得考虑的选择！

---

*文档更新时间：2025年10月*
