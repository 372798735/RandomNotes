# React.js

## 安装

安装基于 webpack 的 React 项目

```javascript
// 安装脚手架
npm install create-react-app -g
// 创建项目
create-react-app my-app
cd my-app
npm run start
```

## react 基础

### 什么是JSX和基本使用

一、概念：
JSX 是 JavaScript和XML（HTML）的缩写，表示在JS代码中编写HTML模板结构，它是React中编写UI模版的方式
优势：1.HTML的声明式模版写法
     2.JS的可编程能力

JSX的本质
JSX并不是标准的JS语法，它是JS的语法扩展，浏览器本身不能识别，需要通过解析工具(BABEL)做解析之后才能在浏览器中运行

二、JSX中使用JS表达式
在JSX中可以通过 大括号语法 {} 识别javaScript中的表达式，比如常见的常量、函数调用、方法调用等等

1. 使用引号传递字符串
2. 使用javaScript变量
3. 函数调用和方法调用
4. 使用javaScript对象

```javascript
// 项目的根组件
const count = 100;
const getName = () => {
  return "Tony";
};
function App() {
  return (
    <div className="App">
      this is App
      {/**使用引号传递字符串 */}
      {"this is message"}
      {/**识别js变量 */}
      {count}
      {/**函数调用 */}
      {getName()}
      {/**方法调用 */}
      {new Date().getDate()}
      {/**使用js对象 */}
      <div style={{ color: "red" }}>this is div</div>
    </div>
  );
}
export default App;
```

三、JSX中实现列表渲染

```javascript
// 项目的根组件
const list = [
  { id: 1, name: 'vue' },
  { id: 2, name: 'react' },
  { id: 3, name:'angular' }
]
function App() {
  return (
    <div className="App">
      <ul>
        {list.map(item =>
         {/** 结构复杂，使用()优结构 */}
          (<li key={item.id}>{ item.name}</li>)
        )}
      </ul>
    </div>
  );
}

export default App;
```

四、JSX中实现条件渲染
在JSX中可以通过 三目运算符 或 && 运算符 实现条件渲染

```javascript
// 项目的根组件
const isLogin = true;
function App() {
  return (
    <div className="App">
      {/**三目运算符 */}
      {isLogin ? <div>this is login</div> : <div>this is not login</div>}
      {/**&& 运算符 */}
      {isLogin && <div>this is login</div>}
    </div>
  );
}

export default App;
```

五、JSX中实现复杂条件渲染

```javascript
// 项目的根组件
const articleType = 1;

// 定义核心函数（根据文章类型返回不同的JSX模板）
function renderArticle(articleType) {
  switch (articleType) {
    case 1:
      return <div>这是一个Vue文章</div>;
    case 2:
      return <div>这是一个React文章</div>;
    case 3:
      return <div>这是一个Angular文章</div>;
    default:
      return <div>未知文章类型</div>;
  }
}

function App() {
  return <div className="App">{renderArticle(articleType)}</div>;
}

export default App;
```

六、JSX中实现事件绑定

语法：on + 事件名称 = {事件处理函数},整体上遵循驼峰命名法

```javascript
function App() {
  // 基本事件绑定
  const handleClick1 = () => {
    console.log("button被点击了");
  };
  // 事件参数e
  const handleClick2 = (e) => {
    console.log(e);
  };
  // 传递自定义参数
  const handleClick3 = (name) => {
    console.log(name);
  };
  // 同时传递自定义参数和事件对象e
  const handleClick4 = (name, e) => {
    console.log(name, e);
  };
  return (
    <div className="App">
      <button onClick={(e) => handleClick4("张三", e)}>点击我</button>
    </div>
  );
}

export default App;

```

七、基础组件使用

```javascript
function App() {
  // 1. 定义组件
  function Button() {
    // 业务逻辑组件
    return <Button>Click me!</Button>;
  }
  return (
    <div className="App">
      {/** 自闭和 */}
      <Button />
      {/** 非自闭和 */}
      <Button>Click me!</Button>
    </div>
  );
}

export default App;

```

八、useState 基础使用
useState 是一个 React Hook 函数，它允许我们向组件添加一个状态变量，从而控制影响组件的渲染结果
本质：和普通JS变量不同的是，状态变量一旦发生变化组件的视图UI也会跟着变化（数据驱动视图）

```javascript
import { useState } from "react";
function App() {
  // 1. 调用useState添加一个状态变量
  // count 状态变量
  // setCount 更新count状态变量的函数
  const [count, setCount] = useState(0);
  // 2. 点击事件回调
  const handleClick = () => {
    setCount(count + 1);
  };
  return (
    <div className="App">
      <button onClick={handleClick}>{count}</button>
    </div>
  );
}
```

export default App;

状态不可变：需要通过setState更新状态变量，不能直接修改状态变量
修改对象状态：规则：对于对象类型的状态变量，应该始终传给set方法一个全新的对象来进行修改

```javascript
import { useState } from "react";
function App() {
  // 1. 调用useState添加一个状态变量
  // count 状态变量
  // setCount 更新count状态变量的函数
  const [count, setCount] = useState(0);
  // 2. 点击事件回调
  const handleClick = () => {
    setCount(count + 1);
  };

  // 修改对象状态
  const [form, setForm] = useState({ name: "jack" });
  const changeForm = () => {
    setForm({
      ...form,
      name: "john",
    });
  };
  return (
    <div className="App">
      <button onClick={handleClick}>{count}</button>
      <button onClick={changeForm}>修改form{form.name}</button>
    </div>
  );
}

export default App;

```

九、组件基础样式方案
React组件基础的样式控制有两种方式

1. 行内样式(不推荐)
2. class类名控制：一般是写一个 .css 文件,通过外部文件引入 .css 文件

十、className 插件的适用
插件下载

```javascript
npm install classnames
```

使用案例

```javascript
import { useState } from "react";
import "./index.css";
function App() {
  const [showColor, setShowColor] = useState(true);
  const handleClick = () => {
    setShowColor(!showColor);
  };
  return (
    <div className="App">
      <div className={showColor ? "red" : "blue"}>字体颜色</div>
      <button onClick={handleClick}>改变颜色</button>
    </div>
  );
}

export default App;

```

十一、input输入框双向绑定

```javascript
import { useState } from "react";
function App() {
  // 1. 声明一个react状态
  const [value, setValue] = useState(true);

  return (
    <div className="App">
      <div>
        <div>{value}</div>你好
        <input
          value={value}
          onChange={(e) => setValue(e.target.value)}
          type="text"
        ></input>
      </div>
    </div>
  );
}

export default App;

```

十二、React获取DOM
使用 useRef 生成ref对象

```javascript
import { useRef } from "react";
function App() {
  // 1. 声明一个react状态
  const inputRef = useRef(null);
  // 2. 定义一个函数，用于获取dom元素
  // 渲染完毕之后dom生成之后才可用
  const showDom = () => {
    console.log(inputRef.current.value);
  };
  return (
    <div className="App">
      <input type="text" ref={inputRef}></input>
      <button onClick={showDom}>获取dom</button>
    </div>
  );
}

export default App;

```

十三、父传子基础实现
1、props可传递任意的数据：数值、字符串、布尔值、数组、对象、函数、JSX
2、props遵循单项数据流，所以props在子组件里面是只读对象：子组件只能读取props中的数据，不能直接进行修改，父组件的数据只能由父组件修改

```javascript
// 父传子
// 1. 父组件传递数据 子组件标签身上绑定属性
// 2. 子组件接收数据 props的参数

function Son(props) {
  // props：对象里面包含了父组件传递过来的所有数据
  return <div>this is son,{props.name}</div>;
}

function App() {
  const name = "this is app name";
  return (
    <div className="App">
      <Son name={name}></Son>
    </div>
  );
}

export default App;

```
