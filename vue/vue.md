# vue
## 一、vue3自定义指令
定义：Vue自定义指令是Vue提供的底层DOM操作API，让你能直接操作 DOM 元素。当内置指令（v-if、v-model、v-show等）无法满足需求时，就可以使用自定义指令。
核心概念：自定义指令本质上是一个包含生命周期钩子的对象，这些钩子会在指令绑定的元素不同阶段被调用。
定义自定义指令有两种方式：全局指令和局部指令

生命周期：
vue2和vue3对比
| vue2 | vue3 |
|--------|------|
| bind | beforeMount |
| inserted | mounted |
| update | 改用：beforeUpdate + updated |
| componentUpdated | updated |
| unbind | unmounted |
核心要点：
最常用：mounted(Vue2的 inserted)

vue3自定义指令的每个生命周期钩子都接收4个固定参数，按顺序如下：
```javascript
const myDirective = {
    mounted(el,binding,vnode,prevVnode){
        // 1. el 指令绑定的元素（DOM 元素）
        // 2. binding - 指令的完整信息对象
        // 3. vnode - 虚拟节点
        // 4. prevVnode - 上一个虚拟节点（仅 beforeUpdate/updated 有值）
    }
}
```
各参数详解：
1. el-绑定元素（最常用）
指令绑定的真实DOM元素
直接操作DOM的目标
2. binding-指令信息对象（最常用）
```javascript
{
    value:'hello' // 指令的值 v-dir="hello"
    oldValue:'world' // 之前的值（仅更新时可用）
    arg:'foo', // 参数 v-dir:foo = "..."
    modifiers:{a:true} // 修饰符 v-dir.a.b
    instance: null // 使用指令的组件实例
    dir:{}  // 指令定义的对象
}
```
3. vnode-虚拟节点
当前绑定的虚拟节点
包含组件信息，props、children等
一般很少直接使用
4. prevVnode-上一个虚拟节点
仅在 beforeUpdate 和 updated 中存在
其他钩子中为 null
用于对比更新前后的变化

## 二、vue3不常用API和新增生命周期
1. shallowRef/shallowReactive
只跟踪顶层属性的变化，忽略嵌套对象的响应式；
适用：优化大对象性能，或管理第三方不可变书记
2. triggerRef
强制出发依赖 shallowRef 的更新
使用 shallowRef 实现防抖 示例：
```javascript
<script setup>
import { customRef } from 'vue'
// 自定义防抖 ref
function useDebouncedRef(value, delay = 500){
    let timeoutId
    return customRef((track, trigger)=> ({
        get(){
           track() // 收集依赖
           return value
        }
        set(newValue){
            clearTimeout(timeoutId)
            timeoutId = setTimeout(() => {
                value = newValue
                trigger() // 触发更新 
            }, delay)
        }
    }))
}

const searchText = useDebounceRef('', 500)
</script>
<template>
    <input v-mode="searchText" placeholder="搜索..." />
</template>
```
```javascript
// 修改深层属性 示例：
const state = shallowRef({
    user:{name:'Alice'}
})
// 深层修改不会触发更新
state.value.user.name = 'Bob'
// 手动强制触发深层修修改更新
triggerRef(state)
```
3. customRef
自定义ref行为，可控制依赖追踪和更新出发
3. markRaw
标记对象永不被转为响应式
适用：存储大型列表、避免重复转换性能开销
4. readonly
创建只读代理，防止修改数据

vue3新增生命周期
1. onRenderTracked: 用于开发调试，跟踪组件的响应式依赖是如何被收集的。当响应式数据被模板或计算属性读取时，此钩子会被调用，仅开发模式可用
2. onRenderTriggered: 也用于开发调试，跟踪响应式依赖触发重新渲染的原因。当响应式数据变化导致组件需要重新渲染时，此勾子会被调用，可以查看
   是哪个属性触发了更新。仅开发模式可用。
   
