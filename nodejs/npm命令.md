# npm 命令
## 版本依赖冲突
```bash
## 报错
npm ERR! code ERESOLVE
...
```
运行命令：
```js
npm install --force // 忽略依赖版本冲突，强制安装
npm install --legacy-peer-deps // 这个命令会使用旧版本 npm v6 的依赖解析方式，对版本冲突更加宽松，比 --force 更安全
```