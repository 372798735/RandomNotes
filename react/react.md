# React

## react.js 介绍

react中文官网[https://zh-hans.react.dev/]

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
 *    npx 会自动下载并执行指定的包
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
