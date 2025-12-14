# PM2 使用指南

## 基本命令

### 启动服务器
```bash
pm2 start ecosystem.config.js
```

### 查看状态
```bash
pm2 status
```

### 查看日志
```bash
pm2 logs
```

### 重启服务器
```bash
pm2 restart github-trending-monitor
```

### 停止服务器
```bash
pm2 stop github-trending-monitor
```

### 删除进程
```bash
pm2 delete github-trending-monitor
```

### 监控面板
```bash
pm2 monit
```

## 开机自启动（可选）

如果想让电脑开机时自动启动服务器：

```bash
pm2 startup
pm2 save
```

## 注意事项

1. **服务器现在会在后台持续运行**，即使关闭命令行窗口
2. **每天 8:30 会自动生成报告**（只要电脑开机）
3. **每天 9:30 会自动发送提醒**（目前只在控制台显示）
4. **日志文件保存在 logs/ 目录**中
5. **可以通过 pm2 logs 查看实时日志**