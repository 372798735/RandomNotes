# React

## 使用脚手架新建一个项目

```javascript
/**
 * 方式一：不用下载脚手架，命令解释(推荐使用)：
 * npx - Node.js 包执行器：
 *     用于运行npm包而不需要全局安装
 *     会自动下载并执行指定的包
 * create-react-app React官方脚手架工具：
 *     Facebook官方提供的创建 React 应用的命令行工具
 *     自动配置 Webpack、Babel、ESLint 等开发工具
 * -- template typescript 模板参数，
 *     指定使用TypeScript模板
 *     会创建带有 TypeScript 配置的项目
 *     所有文件将使用 .tsx 和 .ts 扩展名
 * hook-test 项目名称：
 *     新创建的React应用的文件夹名称
 *     会在当前目录下创建 hook-test 文件夹
 * 为什么不需要下载脚手架：
 *     npx 会自动下载并执行指定的包
 *    即使你的系统上没有安装 create-react-app, npx 也会临时下载它
 *    执行完成后，临时下载的包会被清理
 */
 npx create-react-app --template typescript hook-test
```

---

## Redux Toolkit 的 createSlice 详解

### 什么是 Redux Toolkit 和 createSlice

#### 传统 Redux 的痛点

在传统的 Redux 中，创建状态管理需要编写大量样板代码：

```javascript
// 1. 定义 Action Types
const INCREMENT = 'counter/INCREMENT'
const DECREMENT = 'counter/DECREMENT'

// 2. 创建 Action Creators
const increment = () => ({ type: INCREMENT })
const decrement = () => ({ type: DECREMENT })

// 3. 编写 Reducer
const initialState = { value: 0 }

function counterReducer(state = initialState, action) {
  switch (action.type) {
    case INCREMENT:
      return { ...state, value: state.value + 1 }
    case DECREMENT:
      return { ...state, value: state.value - 1 }
    default:
      return state
  }
}

// 4. 配置 Store
const store = createStore(counterReducer)
```

这种方式需要：

- 手动定义 action types
- 手动创建 action creators
- 手动编写 switch-case 语句
- 手动处理不可变更新（使用展开运算符）
- **代码量大、重复性高、容易出错**

---

#### Redux Toolkit 的 createSlice 简化方案

Redux Toolkit 的 `createSlice` 将上述所有步骤合并成一个函数调用：

```typescript
import { createSlice } from '@reduxjs/toolkit'

// 一个 createSlice 搞定所有事情
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    // 直接定义 reducer 函数，自动生成 action
    increment: (state) => {
      state.value += 1  // 可以直接修改 state（内部使用 Immer）
    },
    decrement: (state) => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    }
  }
})

// 自动生成的 actions
export const { increment, decrement, incrementByAmount } = counterSlice.actions

// 导出 reducer
export default counterSlice.reducer
```

---

### createSlice 的优势

#### 1. 代码量大幅减少

- 传统 Redux: ~40 行代码
- Redux Toolkit: ~15 行代码
- **减少 60% 以上的代码量**

#### 2. 自动生成 Action Creators

```typescript
// 不需要手动写这些了
const increment = () => ({ type: 'counter/increment' })

// createSlice 自动生成
counterSlice.actions.increment()
// 结果: { type: 'counter/increment' }
```

#### 3. 可以直接"修改"状态（内部使用 Immer）

```typescript
// 传统 Redux（必须不可变更新）
case INCREMENT:
  return {
    ...state,
    value: state.value + 1,
    nested: {
      ...state.nested,
      count: state.nested.count + 1
    }
  }

// Redux Toolkit（看起来像直接修改）
increment: (state) => {
  state.value += 1
  state.nested.count += 1  // 简洁明了
}
```

**原理**: createSlice 内部使用 [Immer](https://immerjs.github.io/immer/) 库，允许你写"可变"代码，但实际返回的是不可变更新。

#### 4. TypeScript 支持更好

```typescript
interface CounterState {
  value: number
  loading: boolean
}

const initialState: CounterState = {
  value: 0,
  loading: false
}

const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    // TypeScript 自动推导类型
    increment: (state) => {
      state.value += 1  // ✅ 类型安全
      state.invalid += 1  // ❌ TypeScript 报错
    }
  }
})
```

---

### 实际项目应用示例

#### 示例：用户管理模块

```typescript
import { createSlice, PayloadAction } from '@reduxjs/toolkit'

interface User {
  id: string
  name: string
  email: string
}

interface UserState {
  list: User[]
  loading: boolean
  error: string | null
}

const initialState: UserState = {
  list: [],
  loading: false,
  error: null
}

const userSlice = createSlice({
  name: 'user',
  initialState,
  reducers: {
    // 开始加载
    fetchUsersStart: (state) => {
      state.loading = true
      state.error = null
    },
    // 加载成功
    fetchUsersSuccess: (state, action: PayloadAction<User[]>) => {
      state.loading = false
      state.list = action.payload
    },
    // 加载失败
    fetchUsersFailure: (state, action: PayloadAction<string>) => {
      state.loading = false
      state.error = action.payload
    },
    // 添加用户
    addUser: (state, action: PayloadAction<User>) => {
      state.list.push(action.payload)
    },
    // 删除用户
    removeUser: (state, action: PayloadAction<string>) => {
      state.list = state.list.filter(user => user.id !== action.payload)
    },
    // 更新用户
    updateUser: (state, action: PayloadAction<User>) => {
      const index = state.list.findIndex(u => u.id === action.payload.id)
      if (index !== -1) {
        state.list[index] = action.payload
      }
    }
  }
})

export const {
  fetchUsersStart,
  fetchUsersSuccess,
  fetchUsersFailure,
  addUser,
  removeUser,
  updateUser
} = userSlice.actions

export default userSlice.reducer
```

#### 在组件中使用

```typescript
import { useDispatch, useSelector } from 'react-redux'
import { addUser, removeUser } from './userSlice'

function UserList() {
  const dispatch = useDispatch()
  const { list, loading } = useSelector(state => state.user)

  const handleAdd = () => {
    dispatch(addUser({
      id: '123',
      name: 'John',
      email: 'john@example.com'
    }))
  }

  const handleRemove = (id: string) => {
    dispatch(removeUser(id))
  }

  return (
    <div>
      {loading ? 'Loading...' : list.map(user => (
        <div key={user.id}>
          {user.name}
          <button onClick={() => handleRemove(user.id)}>删除</button>
        </div>
      ))}
      <button onClick={handleAdd}>添加用户</button>
    </div>
  )
}
```

---

### 简历中的实际含义

当简历写"**基于 Redux Toolkit 的 createSlice 简化状态管理代码**"时，表示你：

1. ✅ **了解传统 Redux 的痛点**（样板代码多）
2. ✅ **掌握现代化的状态管理方案**（Redux Toolkit）
3. ✅ **能够编写更简洁、可维护的代码**
4. ✅ **理解不可变更新和 Immer 的原理**
5. ✅ **具备优化代码结构的能力**

这是一个**技术升级和代码质量提升**的体现，说明你能够选择合适的工具来提高开发效率和代码质量。

---

### 总结

**createSlice 简化了什么？**

- ❌ 不需要手动定义 action types
- ❌ 不需要手动创建 action creators
- ❌ 不需要编写 switch-case 语句
- ❌ 不需要手动处理不可变更新
- ✅ 一个函数搞定所有状态管理逻辑
- ✅ 代码量减少 60%+
- ✅ 更好的 TypeScript 支持
- ✅ 更易维护和理解

这就是"简化状态管理代码"的核心含义！

---

## React Query 管理服务端状态与缓存优化

### 什么是 React Query（TanStack Query）

React Query（现在称为 TanStack Query）是一个强大的服务端状态管理库，专门用于处理异步数据的获取、缓存、同步和更新。

### 为什么需要 React Query？

#### 传统方式的问题

在没有 React Query 之前，我们通常这样管理服务端数据：

```typescript
import { useState, useEffect } from 'react'
import axios from 'axios'

function UserList() {
  const [users, setUsers] = useState([])
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState(null)

  useEffect(() => {
    const fetchUsers = async () => {
      setLoading(true)
      setError(null)
      try {
        const response = await axios.get('/api/users')
        setUsers(response.data)
      } catch (err) {
        setError(err.message)
      } finally {
        setLoading(false)
      }
    }

    fetchUsers()
  }, [])

  if (loading) return <div>Loading...</div>
  if (error) return <div>Error: {error}</div>

  return (
    <div>
      {users.map(user => (
        <div key={user.id}>{user.name}</div>
      ))}
    </div>
  )
}
```

**存在的问题**：
- ❌ 需要手动管理 loading、error、data 三个状态
- ❌ 没有缓存机制，每次组件挂载都会重新请求
- ❌ 多个组件请求同一数据会导致重复请求
- ❌ 无法轻松实现数据预取、后台刷新
- ❌ 数据过期管理复杂
- ❌ 乐观更新和回滚困难

---

### React Query 的解决方案

使用 React Query，上述代码可以简化为：

```typescript
import { useQuery } from '@tanstack/react-query'
import axios from 'axios'

function UserList() {
  const { data: users, isLoading, error } = useQuery({
    queryKey: ['users'],
    queryFn: async () => {
      const response = await axios.get('/api/users')
      return response.data
    }
  })

  if (isLoading) return <div>Loading...</div>
  if (error) return <div>Error: {error.message}</div>

  return (
    <div>
      {users.map(user => (
        <div key={user.id}>{user.name}</div>
      ))}
    </div>
  )
}
```

**优势**：
- ✅ 自动管理 loading、error、data 状态
- ✅ 自动缓存数据
- ✅ 自动去重请求
- ✅ 后台自动刷新
- ✅ 支持数据预取
- ✅ 内置重试机制

---

### React Query 的核心概念

#### 1. Query（查询）

用于获取数据的基本单位，每个查询都有唯一的 `queryKey`。

```typescript
// 简单查询
const { data } = useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers
})

// 带参数的查询
const { data } = useQuery({
  queryKey: ['user', userId],  // queryKey 包含参数
  queryFn: () => fetchUserById(userId)
})

// 依赖查询（只有 userId 存在时才执行）
const { data } = useQuery({
  queryKey: ['user', userId],
  queryFn: () => fetchUserById(userId),
  enabled: !!userId  // 条件查询
})
```

#### 2. Mutation（变更）

用于修改服务端数据（POST、PUT、DELETE 等操作）。

```typescript
import { useMutation, useQueryClient } from '@tanstack/react-query'

function AddUser() {
  const queryClient = useQueryClient()

  const mutation = useMutation({
    mutationFn: (newUser) => {
      return axios.post('/api/users', newUser)
    },
    onSuccess: () => {
      // 变更成功后，使 users 查询失效并重新获取
      queryClient.invalidateQueries({ queryKey: ['users'] })
    }
  })

  const handleAdd = () => {
    mutation.mutate({
      name: 'John',
      email: 'john@example.com'
    })
  }

  return (
    <button onClick={handleAdd} disabled={mutation.isPending}>
      {mutation.isPending ? 'Adding...' : 'Add User'}
    </button>
  )
}
```

#### 3. Query Invalidation（查询失效）

让缓存的数据过期，触发重新获取。

```typescript
// 使特定查询失效
queryClient.invalidateQueries({ queryKey: ['users'] })

// 使所有以 'users' 开头的查询失效
queryClient.invalidateQueries({ queryKey: ['users'], exact: false })

// 立即重新获取
queryClient.invalidateQueries({
  queryKey: ['users'],
  refetchType: 'active' // 只重新获取活跃的查询
})
```

---

### 缓存策略优化

#### 1. 缓存时间配置

```typescript
const { data } = useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
  // 数据被认为是新鲜的时间（默认 0）
  staleTime: 5 * 60 * 1000,  // 5分钟内不会重新请求

  // 未使用的数据在缓存中保留的时间（默认 5分钟）
  gcTime: 10 * 60 * 1000,  // 10分钟后清除缓存
})
```

**staleTime vs gcTime**：
- `staleTime`: 数据"新鲜"的时间，在此期间不会发起新请求
- `gcTime`: 未使用的数据在内存中保留的时间

```
请求 ─→ 新鲜数据 ─→ 过期数据 ─→ 垃圾回收
       (staleTime)  (gcTime)
```

#### 2. 后台自动刷新

```typescript
const { data } = useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
  // 窗口重新获得焦点时自动刷新
  refetchOnWindowFocus: true,  // 默认 true

  // 网络重新连接时刷新
  refetchOnReconnect: true,  // 默认 true

  // 组件挂载时刷新
  refetchOnMount: true,  // 默认 true

  // 定时轮询
  refetchInterval: 30000,  // 每30秒刷新一次

  // 只在窗口聚焦时轮询
  refetchIntervalInBackground: false
})
```

#### 3. 预取数据（Prefetching）

在用户需要之前提前加载数据：

```typescript
import { useQueryClient } from '@tanstack/react-query'

function UserListItem({ userId }) {
  const queryClient = useQueryClient()

  // 鼠标悬停时预取用户详情
  const handleMouseEnter = () => {
    queryClient.prefetchQuery({
      queryKey: ['user', userId],
      queryFn: () => fetchUserById(userId),
      staleTime: 10000  // 10秒内不重复预取
    })
  }

  return (
    <div onMouseEnter={handleMouseEnter}>
      <Link to={`/users/${userId}`}>查看详情</Link>
    </div>
  )
}
```

#### 4. 乐观更新（Optimistic Updates）

在请求完成前先更新 UI，失败时回滚：

```typescript
const mutation = useMutation({
  mutationFn: updateUser,
  onMutate: async (newUser) => {
    // 取消正在进行的查询
    await queryClient.cancelQueries({ queryKey: ['users'] })

    // 保存当前数据（用于回滚）
    const previousUsers = queryClient.getQueryData(['users'])

    // 乐观更新
    queryClient.setQueryData(['users'], (old) => {
      return old.map(user =>
        user.id === newUser.id ? newUser : user
      )
    })

    // 返回上下文（用于回滚）
    return { previousUsers }
  },
  onError: (err, newUser, context) => {
    // 失败时回滚
    queryClient.setQueryData(['users'], context.previousUsers)
  },
  onSettled: () => {
    // 完成后重新获取数据
    queryClient.invalidateQueries({ queryKey: ['users'] })
  }
})
```

#### 5. 分页查询

```typescript
import { useQuery } from '@tanstack/react-query'

function PaginatedUsers() {
  const [page, setPage] = useState(1)

  const { data, isLoading, isPreviousData } = useQuery({
    queryKey: ['users', page],
    queryFn: () => fetchUsers(page),
    // 保留前一页数据，切换时不显示 loading
    keepPreviousData: true
  })

  return (
    <div>
      {isLoading ? (
        <div>Loading...</div>
      ) : (
        <>
          {data.users.map(user => (
            <div key={user.id}>{user.name}</div>
          ))}

          <button
            onClick={() => setPage(old => Math.max(old - 1, 1))}
            disabled={page === 1}
          >
            上一页
          </button>

          <button
            onClick={() => setPage(old => old + 1)}
            disabled={isPreviousData || !data.hasMore}
          >
            下一页
          </button>
        </>
      )}
    </div>
  )
}
```

#### 6. 无限滚动

```typescript
import { useInfiniteQuery } from '@tanstack/react-query'

function InfiniteUsers() {
  const {
    data,
    fetchNextPage,
    hasNextPage,
    isFetchingNextPage
  } = useInfiniteQuery({
    queryKey: ['users'],
    queryFn: ({ pageParam = 1 }) => fetchUsers(pageParam),
    getNextPageParam: (lastPage, pages) => {
      // 返回下一页的参数，返回 undefined 表示没有更多数据
      return lastPage.hasMore ? pages.length + 1 : undefined
    }
  })

  return (
    <div>
      {data?.pages.map((page, i) => (
        <div key={i}>
          {page.users.map(user => (
            <div key={user.id}>{user.name}</div>
          ))}
        </div>
      ))}

      <button
        onClick={() => fetchNextPage()}
        disabled={!hasNextPage || isFetchingNextPage}
      >
        {isFetchingNextPage
          ? 'Loading...'
          : hasNextPage
          ? '加载更多'
          : '没有更多了'}
      </button>
    </div>
  )
}
```

---

### 完整实战示例：用户管理模块

```typescript
// api/users.ts
import axios from 'axios'

export interface User {
  id: string
  name: string
  email: string
}

export const fetchUsers = async (): Promise<User[]> => {
  const { data } = await axios.get('/api/users')
  return data
}

export const fetchUserById = async (id: string): Promise<User> => {
  const { data } = await axios.get(`/api/users/${id}`)
  return data
}

export const createUser = async (user: Omit<User, 'id'>): Promise<User> => {
  const { data } = await axios.post('/api/users', user)
  return data
}

export const updateUser = async (user: User): Promise<User> => {
  const { data } = await axios.put(`/api/users/${user.id}`, user)
  return data
}

export const deleteUser = async (id: string): Promise<void> => {
  await axios.delete(`/api/users/${id}`)
}
```

```typescript
// hooks/useUsers.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query'
import * as api from '../api/users'

// 获取用户列表
export const useUsers = () => {
  return useQuery({
    queryKey: ['users'],
    queryFn: api.fetchUsers,
    staleTime: 5 * 60 * 1000,  // 5分钟内认为数据是新鲜的
  })
}

// 获取单个用户
export const useUser = (id: string) => {
  return useQuery({
    queryKey: ['user', id],
    queryFn: () => api.fetchUserById(id),
    enabled: !!id,  // 只有 id 存在时才查询
  })
}

// 创建用户
export const useCreateUser = () => {
  const queryClient = useQueryClient()

  return useMutation({
    mutationFn: api.createUser,
    onSuccess: (newUser) => {
      // 方法1: 使查询失效，触发重新获取
      queryClient.invalidateQueries({ queryKey: ['users'] })

      // 方法2: 直接更新缓存（性能更好）
      // queryClient.setQueryData(['users'], (old) => [...old, newUser])
    },
  })
}

// 更新用户
export const useUpdateUser = () => {
  const queryClient = useQueryClient()

  return useMutation({
    mutationFn: api.updateUser,
    // 乐观更新
    onMutate: async (updatedUser) => {
      await queryClient.cancelQueries({ queryKey: ['users'] })

      const previousUsers = queryClient.getQueryData(['users'])

      queryClient.setQueryData(['users'], (old: any) =>
        old.map((user: any) =>
          user.id === updatedUser.id ? updatedUser : user
        )
      )

      return { previousUsers }
    },
    onError: (err, updatedUser, context) => {
      queryClient.setQueryData(['users'], context?.previousUsers)
    },
    onSettled: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] })
    },
  })
}

// 删除用户
export const useDeleteUser = () => {
  const queryClient = useQueryClient()

  return useMutation({
    mutationFn: api.deleteUser,
    onSuccess: (_, deletedId) => {
      // 直接从缓存中移除
      queryClient.setQueryData(['users'], (old: any) =>
        old.filter((user: any) => user.id !== deletedId)
      )
    },
  })
}
```

```typescript
// components/UserList.tsx
import { useUsers, useCreateUser, useDeleteUser } from '../hooks/useUsers'

function UserList() {
  const { data: users, isLoading, error } = useUsers()
  const createUser = useCreateUser()
  const deleteUser = useDeleteUser()

  const handleAdd = () => {
    createUser.mutate({
      name: 'New User',
      email: 'newuser@example.com'
    })
  }

  const handleDelete = (id: string) => {
    if (confirm('确认删除？')) {
      deleteUser.mutate(id)
    }
  }

  if (isLoading) return <div>Loading...</div>
  if (error) return <div>Error: {error.message}</div>

  return (
    <div>
      <button onClick={handleAdd} disabled={createUser.isPending}>
        {createUser.isPending ? 'Adding...' : 'Add User'}
      </button>

      {users?.map(user => (
        <div key={user.id}>
          {user.name} - {user.email}
          <button
            onClick={() => handleDelete(user.id)}
            disabled={deleteUser.isPending}
          >
            Delete
          </button>
        </div>
      ))}
    </div>
  )
}
```

```typescript
// App.tsx - 配置 QueryClient
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'

// 配置全局默认选项
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 60 * 1000,  // 默认 1 分钟
      gcTime: 5 * 60 * 1000,  // 默认 5 分钟
      retry: 3,  // 失败重试 3 次
      refetchOnWindowFocus: false,  // 关闭窗口聚焦时自动刷新
    },
  },
})

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <UserList />
      {/* 开发工具（只在开发环境显示） */}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  )
}
```

---

### 高级缓存策略

#### 1. 结构化数据规范化

对于关联数据，使用规范化缓存：

```typescript
// 不好的做法：重复存储用户数据
queryClient.setQueryData(['post', 1], {
  id: 1,
  title: 'Post 1',
  author: { id: 1, name: 'John' }  // 用户数据
})

queryClient.setQueryData(['post', 2], {
  id: 2,
  title: 'Post 2',
  author: { id: 1, name: 'John' }  // 重复的用户数据
})

// 好的做法：分离存储
queryClient.setQueryData(['post', 1], {
  id: 1,
  title: 'Post 1',
  authorId: 1  // 只存储 ID
})

queryClient.setQueryData(['user', 1], {
  id: 1,
  name: 'John'  // 用户数据单独缓存
})
```

#### 2. 依赖查询

```typescript
function UserPosts({ userId }) {
  // 先获取用户信息
  const { data: user } = useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId)
  })

  // 只有用户信息获取成功后才获取文章列表
  const { data: posts } = useQuery({
    queryKey: ['posts', userId],
    queryFn: () => fetchUserPosts(userId),
    enabled: !!user,  // 依赖 user 存在
  })

  return <div>{/* ... */}</div>
}
```

#### 3. 并行查询

```typescript
function Dashboard() {
  const users = useQuery({ queryKey: ['users'], queryFn: fetchUsers })
  const posts = useQuery({ queryKey: ['posts'], queryFn: fetchPosts })
  const comments = useQuery({ queryKey: ['comments'], queryFn: fetchComments })

  // 使用 useQueries 更优雅
  const results = useQueries({
    queries: [
      { queryKey: ['users'], queryFn: fetchUsers },
      { queryKey: ['posts'], queryFn: fetchPosts },
      { queryKey: ['comments'], queryFn: fetchComments },
    ]
  })

  const [usersQuery, postsQuery, commentsQuery] = results
}
```

---

### 性能优化最佳实践

#### 1. 选择性订阅（避免不必要的重渲染）

```typescript
// 不好：整个对象变化都会触发重渲染
const { data } = useQuery({ queryKey: ['user', id], queryFn: fetchUser })

// 好：只订阅需要的字段
const name = useQuery({
  queryKey: ['user', id],
  queryFn: fetchUser,
  select: (data) => data.name  // 只有 name 变化时才重渲染
})
```

#### 2. 使用 staleTi优化请求频率

```typescript
// 对于不常变化的数据，设置较长的 staleTime
const { data } = useQuery({
  queryKey: ['config'],
  queryFn: fetchConfig,
  staleTime: Infinity,  // 永不过期（适用于静态配置）
})

// 对于实时数据，使用轮询
const { data } = useQuery({
  queryKey: ['realtime'],
  queryFn: fetchRealtime,
  refetchInterval: 1000,  // 每秒刷新
})
```

#### 3. 错误重试策略

```typescript
const { data } = useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
  retry: (failureCount, error) => {
    // 404 错误不重试
    if (error.response?.status === 404) return false
    // 最多重试 3 次
    return failureCount < 3
  },
  retryDelay: (attemptIndex) => {
    // 指数退避：1s, 2s, 4s, 8s...
    return Math.min(1000 * 2 ** attemptIndex, 30000)
  },
})
```

---

### 简历中的实际含义

当简历写"**使用 React Query 管理服务端状态，优化缓存策略**"时，表示你：

1. ✅ **理解客户端状态和服务端状态的区别**
2. ✅ **掌握现代化的数据获取方案**（React Query）
3. ✅ **能够实现智能缓存策略**（staleTime、gcTime）
4. ✅ **具备性能优化能力**（预取、乐观更新、去重请求）
5. ✅ **了解数据同步和一致性**（invalidation、refetch）
6. ✅ **能够处理复杂的异步场景**（分页、无限滚动、依赖查询）

这体现了你：
- 🎯 关注用户体验（减少 loading、优化响应速度）
- 🎯 注重性能优化（减少不必要的请求）
- 🎯 代码质量高（简洁、可维护）
- 🎯 具备架构思维（合理的缓存策略设计）

---

### 总结对比

| 特性 | 传统方式 | React Query |
|------|---------|-------------|
| 状态管理 | 手动管理 loading/error/data | 自动管理 |
| 缓存 | 无或手动实现 | 自动缓存 + 智能失效 |
| 请求去重 | 无 | 自动去重 |
| 后台刷新 | 需要手动实现 | 自动后台刷新 |
| 重试机制 | 需要手动实现 | 内置重试 + 指数退避 |
| 乐观更新 | 复杂 | 简单易用 |
| 预取数据 | 需要手动实现 | 一行代码实现 |
| 开发工具 | 无 | DevTools 可视化调试 |
| 代码量 | 多 | 少（减少 70%+） |

**核心价值**：React Query 通过智能缓存和自动化的数据同步机制，让开发者专注于业务逻辑，而不是处理繁琐的状态管理和数据获取细节。
