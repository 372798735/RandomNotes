# react实战

## 一、react中使用typeScript

### 1、React.FC

含义：React.FC 是 TypeScript 中用于定义 React 函数组件的类型别名，是React.FunctionComponent 的缩写。
基本语法：

```typeScript
import React from 'react';

React.FC<PropsType>  
// React.FC: 表示这是一个 React 函数组件类型
// <PropsType>: 表示这是一个泛型，用于指定组件的 props 类型

// 实际项目应用
/**
 * 1、定义了一个名为 MyComponent 的函数组件
 * 2、该组件接收一个 name 属性，属性类型为字符串
 * 3、组件返回一个包含该名称的 div 元素
 */
const MyComponent: React.FC<{ name: string }> = ({ name }) => {
  return <div>Hello, {name}!</div>;
}
```

## 二、react中的基础Hook

### 1. useEffect

useEffect 是 React 中用于处理副作用操作的 Hook, 副作用是指:

- 数据获取(API请求)
- 订阅(事件监听器)
- 手动修改 DOM 元素
- 定时器(setTimeout, setInterval)
- 其它不影响组件渲染的操作

它的基本语法如下：

```typeScript
useEffect(() => {
  // 副作用操作
  return () => {
    // 清理操作
  };
}, [依赖项]);
```

使用场景

1、 无依赖项-每次渲染(是指 React 执行组件函数返回 JSX, 然后更新 DOM 的过程)后都执行

```typeScript
useEffect(() => {
  console.log('组件渲染后执行');
});
```

2、 空依赖数组-仅在组件挂在时执行一次

```typeScript
useEffect(() => {
  console.log('组件挂载后执行一次');
}, []);
```

3、 有依赖项-仅当依赖项发生变化时执行

```typeScript
useEffect(() => {
  console.log('依赖项变化时执行');
}, [依赖项]);
```

4、清理副作用函数
   当组件卸载时, 或者依赖项变化时, 可以执行清理操作, 例如取消订阅, 清除定时器等

```typeScript
useEffect(() => {
  const timer = setInterval(() => {
    console.log('定时执行');
  }, 1000);
  
  // 返回清理函数
  return () => {
    clearInterval(timer);
  };
}, []);
```
