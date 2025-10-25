# cmd常用命令

### 查询端口占用和强制结束端口运行
```cmd
netstat -ano | findstr :3306  # 查看端口应用情况
taskkill /PID 6152 /F  # 强制关闭应用为 6152 的应用进程，需要管理员权限
```