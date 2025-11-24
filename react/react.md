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
 *    npx 会自动下载并执行指定的包
 *    即使你的系统上没有安装 create-react-app, npx 也会临时下载它
 *    执行完成后，临时下载的包会被清理
 */
 npx create-react-app --template typescript hook-test
```
## 认识 react 的 hook
### useState
* useState 是 React 提供的一个 Hook 函数，用于在函数组件中添加状态。
* 它返回一个数组，包含当前状态值和更新状态的函数。
* 状态值可以是任意类型，初始值可以是任何值。
* 更新状态的函数可以同步或异步调用，具体取决于使用方式。
* 每次调用更新状态的函数时，组件会重新渲染，显示最新的状态值。
1. 示例：简单使用
```javascript
import { useState } from "react";

function App() {
  const [num, setNum] = useState(1);

  return (
    <div onClick={() => setNum(num + 1)}>{num}</div>
  );
}
export default App;
```
2. 示例：需要处理复杂的初始数据
```javascript
const [num, setNum] = useState(() => {
    const num1 = 1 + 2;
    const num2 = 2 + 3;
    return num1 + num2
});
```
### useEffect
* useEffect 是 React 提供的一个 Hook 函数，用于在函数组件中执行副作用操作。
* 它可以在组件渲染完成后执行，也可以在状态变化时执行。
* useEffect 接受两个参数：一个回调函数和一个依赖数组。
* 回调函数在组件渲染完成后执行，依赖数组指定了哪些状态变化时需要重新执行回调函数。
* 如果依赖数组为空，回调函数只会在组件挂载时执行一次。
* 如果依赖数组包含状态变量，回调函数会在状态变化时执行。
* 使用场景：
    * 数据获取：在组件挂载时获取数据，或在依赖的状态变量变化时重新获取数据。
    * 订阅：在组件挂载时订阅事件，或在依赖的状态变量变化时重新订阅。
    * 手动修改 DOM：在组件挂载时修改 DOM，或在依赖的状态变量变化时重新修改 DOM。
1. 示例：简单使用
```javascript
import { useState, useEffect } from "react";

function App() {
  const [num, setNum] = useState(1);

  useEffect(() => {
    console.log(num);
  }, [num]);

  return (
    <div onClick={() => setNum(num + 1)}>{num}</div>
  );
}
export default App;
```
### useLayoutEffect
* useLayoutEffect 是 React 提供的一个 Hook 函数，用于在函数组件中执行副作用操作。
* 它的行为与 useEffect 类似，但是会在 DOM 更新后同步执行。
* 这意味着在 useLayoutEffect 中对 DOM 的修改会立即生效，而 useEffect 中的修改会在组件渲染完成后异步执行。
* 使用场景：
    * 手动修改 DOM：在组件挂载时修改 DOM，或在依赖的状态变量变化时重新修改 DOM。
    * 测量 DOM 元素：在组件挂载时测量 DOM 元素的尺寸或位置，或在依赖的状态变量变化时重新测量。
    * 绝大多数情况下，用useEffect,它能避免因为 effect 逻辑执行时间长导致页面卡顿(掉帧)。
    * 如果，才考虑使用 useLayoutEffect。
1. 示例：简单使用
```javascript
import { useState, useLayoutEffect } from "react";

function App() {
  const [num, setNum] = useState(1);
  useLayoutEffect(() => {
    console.log(num);
  }, [num]);
  return (
    <div onClick={() => setNum(num + 1)}>{num}</div>
  );
}
export default App;
```
### useReducer
* useReducer 是 React 提供的一个 Hook 函数，用于在函数组件中管理状态。
* 它类似于 useState，但是更适合处理复杂的状态逻辑。
* useReducer 接受一个 reducer 函数和一个初始状态值作为参数。
* reducer 函数定义了状态的更新逻辑，根据当前状态和操作返回新的状态。
* useReducer 返回一个数组，包含当前状态值和 dispatch 函数。
* dispatch 函数用于触发状态更新，调用 reducer 函数并传递操作参数。
* 使用场景：
    * 状态逻辑复杂：当组件的状态逻辑比较复杂，包含多个状态变量和多个操作时，useReducer 更适合管理状态。
    * 状态逻辑共享：当多个组件需要共享相同的状态逻辑时，使用 useReducer 可以避免重复编写状态逻辑代码。
    * 写一次方法多个地方可以使用
1. 示例：简单使用
```javascript
import { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { num: state.num + 1 };
    case "decrement":
      return { num: state.num - 1 };
    default:
      return state;
  }
}  
function App() {
  const [state, dispatch] = useReducer(reducer, { num: 1 });
  return (
    <div>
      <div>{state.num}</div>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
    </div>
  );
}
export default App;
```
### useRef
* useRef 是 React 提供的一个 Hook 函数，用于在函数组件中创建一个可变的引用对象。
* 它可以用于存储任何值，例如 DOM 元素、定时器 ID、组件实例等。
* useRef 返回一个对象，该对象的 current 属性指向存储的值。
* 使用场景：
    * 访问 DOM 元素：当需要在函数组件中访问 DOM 元素时，使用 useRef 可以避免使用 ref 属性。
    * 存储可变值：当需要在函数组件中存储一个可变的值，例如组件实例、定时器 ID 等时，使用 useRef 可以避免使用 useState 导致的重新渲染。
1. 示例：简单使用
```javascript
import { useRef } from "react";

function App() {
  const numRef = useRef(1);
  return (
    <div>
      <div>{numRef.current}</div>
      <button onClick={() => numRef.current++}>+</button>
      <button onClick={() => numRef.current--}>-</button>
    </div>
  );
}
export default App;
```

### useContext
* 用于**跨组件共享数据**，避免 props 层层传递
* 需要三步：① 创建 Context → ② Provider 提供数据 → ③ useContext 消费数据

**创建和使用示例：**
```javascript
// 1. 创建 Context
import { createContext } from 'react';
export const ThemeContext = createContext();

// 2. 使用 Provider 提供数据
function App() {
  const theme = { color: 'blue', bg: 'lightgray' };
  return (
    <ThemeContext.Provider value={theme}>
      <MyComponent />
    </ThemeContext.Provider>
  );
}

// 3. 子组件用 useContext 获取数据
import { useContext } from 'react';
function MyComponent() {
  const theme = useContext(ThemeContext);
  return <div style={{ color: theme.color }}>内容</div>;
}
```

**常见场景：** 主题切换、用户登录状态、语言设置、全局配置

**注意：** Context 值改变会导致所有使用它的组件重新渲染

## react编辑器里面的调试
![编辑器调试](https://cdn.nlark.com/yuque/0/2025/png/2488285/1763558687264-866c951f-d0b3-41a9-b354-d19243319617.png?x-oss-process=image%2Fformat%2Cwebp)

## 受控模式和非受控模式

### 模式对比表格

| 特性 | 受控模式 | 非受控模式 |
|------|----------|------------|
| 状态存储位置 | 父组件 | 子组件内部 |
| 数据流向 | 父组件 → 子组件（单向） | 子组件自己管理 |
| 父组件传入 | value + onChange | defaultValue |
| 状态更新方式 | 父组件更新 state → 触发子组件重渲染 | 子组件内部 setState |
| 父组件能否读取当前值 | ✅ 能（通过 state） | ❌ 不能直接读取 |
| 适用场景 | 需要外部控制、联动、校验 | 简单场景、不需要外部干预 |

### 详细解释

#### 受控模式（Controlled Components）
- **定义**：表单数据由React组件的state控制
- **特点**：通过`value`属性和`onChange`事件处理器管理
- **数据流**：单向数据流（state → UI）

#### 非受控模式（Uncontrolled Components）
- **定义**：表单数据由DOM自身管理
- **特点**：使用`ref`直接访问DOM元素获取值
- **数据流**：直接操作DOM

#### 代码示例
```javascript
/** 
 * 这个 Hook 来自 ahooks 库，它的作用是自动处理受控和非受控组件的逻辑。
 * 它接受一个 props 对象和一个配置对象作为参数。
 * 配置对象可以包含 defaultValue、value、onChange 等属性。
 * 它返回一个数组，包含当前值和更新值的函数。
 * 当 props 中包含 value 和 onChange 时，Hook 会返回 props 中的值和更新函数。
 * 当 props 中不包含 value 和 onChange 时，Hook 会返回 defaultValue 和更新函数。
*/ 
import { useControllableValue } from 'ahooks';
const [curValue, setCurValue] = useControllableValue<Dayjs>(props, {
        defaultValue: dayjs()
});
```

## react 组件
### Suspense
* 用于**优雅地处理异步操作的加载状态**，在等待内容加载时显示后备内容（loading）
* 主要用途：代码分割、组件懒加载、异步数据获取

#### 基本用法：懒加载组件
```javascript
import { Suspense, lazy } from 'react';

// 懒加载组件
const LazyComponent = lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>加载中...</div>}>
      <LazyComponent />
    </Suspense>
  );
}
```

#### 多个组件共享 loading
```javascript
import { Suspense, lazy } from 'react';

const UserProfile = lazy(() => import('./UserProfile'));
const UserPosts = lazy(() => import('./UserPosts'));

function UserPage() {
  return (
    <Suspense fallback={<div>正在加载用户信息...</div>}>
      <UserProfile />
      <UserPosts />
    </Suspense>
  );
}
```

#### 嵌套 Suspense（细粒度控制）
```javascript
function App() {
  return (
    <Suspense fallback={<PageLoading />}>
      <Header />

      <Suspense fallback={<Spinner />}>
        <MainContent />
      </Suspense>

      <Suspense fallback={<SidebarLoading />}>
        <Sidebar />
      </Suspense>
    </Suspense>
  );
}
```

#### 配合路由使用
```javascript
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { Suspense, lazy } from 'react';

const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const User = lazy(() => import('./pages/User'));

function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<div>页面加载中...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/user" element={<User />} />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

#### 完整示例：带错误边界
```javascript
import { Suspense, lazy } from 'react';
import { ErrorBoundary } from 'react-error-boundary';

const HeavyComponent = lazy(() => import('./HeavyComponent'));

function LoadingFallback() {
  return (
    <div style={{ textAlign: 'center', padding: '20px' }}>
      <div className="spinner">加载中...</div>
    </div>
  );
}

function ErrorFallback({ error }) {
  return (
    <div style={{ color: 'red' }}>
      <h2>加载失败</h2>
      <p>{error.message}</p>
    </div>
  );
}

function App() {
  return (
    <ErrorBoundary FallbackComponent={ErrorFallback}>
      <Suspense fallback={<LoadingFallback />}>
        <HeavyComponent />
      </Suspense>
    </ErrorBoundary>
  );
}
```

**关键属性：**
- `fallback`：加载时显示的后备内容（ReactNode）
- `children`：被 Suspense 包裹的组件（ReactNode）

**使用场景：**
- 代码分割：按需加载大型组件，减少首屏加载时间
- 路由懒加载：不同页面按需加载
- 条件渲染：根据用户操作动态加载组件
- 优化性能：延迟加载不重要的内容

**注意事项：**
- `lazy()` 必须在组件外部调用，不能在组件内部动态调用
- `fallback` 不能是 Suspense 组件本身
- 懒加载的组件必须是默认导出（`export default`）
- Suspense 会捕获其子树中所有组件的 loading 状态

