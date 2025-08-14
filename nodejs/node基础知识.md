## 查看所有线上版本信息
需要使powerShell
命令解析：
1. Invoke-RestMethod -Uri "https://nodejs.org/dist/index.json"
Invoke-RestMethod: PowerShell的HTTP请求方法，用于发送HTTP请求并自动解析JSON响应
-Uri "https://nodejs.org/dist/index.json": 指定请求的URL，这是Node.js官方的版本信息API端点
作用: 从Node.js官网获取所有可用版本的JSON数据
2. Select-Object -First 10 
PowerShell管道操作符，将前一个命令的输出作为下一个命令的输入
Select-Object -First 10: 选择前10个结果
作用: 限制输出只显示最新的10个版本，避免信息过多
3. Format-Table version, date, lts
Format-Table: 以表格形式格式化输出
version, date, lts: 指定要显示的列
version: Node.js版本号
date: 发布日期
lts: 是否为长期支持版本（Long Term Support）
```powershell
Invoke-RestMethod -Uri "https://nodejs.org/dist/index.json" | Select-Object -First 10 | Format-Table version, date, lts
```
输出结果如下图所示：
![查看所有线上版本信息](https://cdn.nlark.com/yuque/0/2025/png/2488285/1755151247966-2eb5d799-bdad-4ad3-b202-72e065327f7d.png?x-oss-process=image%2Fformat%2Cwebp)