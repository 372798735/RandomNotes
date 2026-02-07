# interview

微前端[微前端-架构设计](https://juejin.cn/post/7215122422244024357)
Monorepo[Monorepo-架构设计](https://juejin.cn/post/7428154082224226330?searchId=202512301719236F39B8E5531A49074CC4)
React-query[React-query](https://juejin.cn/post/7105426666020667405?searchId=202601030037467652D8484772D8ED7172)
gis地图服务[gis地图服务](https://juejin.cn/post/7215122422244024357)
three.js[three.js](https://juejin.cn/post/7215122422244024357)
react.native[React Native](https://juejin.cn/post/7030771637796470791?searchId=20260107231810235D9EC32A9D95B16C38)

## 最近遇到比较难的问题是怎么解决的

问题：Element Plus 表格加载超 几百行 行数据具有编辑功能表格时时，出现严重渲染卡顿与操作延迟。
原因：渲染数据比较多，而且每一行数据都有可以编辑的功能，且全量 DOM 渲染导致性能瓶颈。
解决方案：
我的解决思路是分两步：先优化渲染，再优化计算。
Element Plus虚拟表格：升级到2.4.0+版本，使用官方el-table-v2组件，通过虚拟滚动技术只渲染可视区域约20-30行DOM。
Web Worker数据处理：将排序、筛选等复杂计算放入Worker线程，避免主线程阻塞。
优化结果：
万级数据表格滚动帧率稳定在 55-60 FPS
内存占用降低约 65%
操作响应时间从 2-3 秒降至 200 毫秒内

## 原型和原型链是什么

原型是 JavaScript 实现继承的基础。每个函数都有一个 prototype 属性，它指向一个对象，这个对象包含了由该函数创建的实例共享的属性和方法。
原型链是 JavaScript 查找对象属性的机制。每个对象都有一个 __proto__ 属性，指向它的原型对象。当访问一个对象的属性时，如果自身没有，就会沿着 __proto__ 这条链向上查找，直到找到或到达链的尽头（null）。
核心关系：
实例对象.__proto__ === 其构造函数.prototype
一句话总结：
原型（prototype）是构造函数的共享属性库；原型链（__proto__）是实例对象的查找机制。
