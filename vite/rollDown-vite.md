## 一、vue3打包时长检测
在 PowerShell 中，你可以使用 `Measure-Command` 来测量命令的执行时间。以下是一个示例，展示了如何测量 `vite build` 命令的执行时间：

```powershell
Measure-Command { npm run build }
```
![image](https://cdn.nlark.com/yuque/0/2025/png/2488285/1751258099441-d8a46b2e-9d67-4a38-8cdd-79c2f4945416.png?x-oss-process=image%2Fformat%2Cwebp)
参数说明 表格

| 时间单位 | 值  | 说明 |
| --- | ---- | --- |
| Days    | 0 | 该时间跨度对应的天数 |
| Hours   | 0 | 该时间跨度对应的天数 |
| Minutes  |  3  |该时间跨度对应的分钟数 (取整)
| Seconds   | 11  |该时间跨度对应的秒数 (取整)
| Milliseconds  | 605  |该时间跨度对应的毫秒数(取整)
| Ticks        | 1916053800  |Ticks是—个时间单位：1秒等于10000000，该值表示该时间跨度对应的Ticks数
| TotalDays  | 0.00221765486111111  |该时间跨度对应的总天数
| TotalHours | 0.0532237166666667  | 该时间跨度对应的总小时数
| TotalMinutes | 3.193423  | 该时间跨度对应的总分钟数
| TotalSeconds  | 191.60538  | 该时间跨度对应的总秒数
综合来看，这些时间数据表示的时间宽度约为3分11秒605毫秒。
