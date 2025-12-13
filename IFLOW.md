# GitHub 趋势项目监控器

## 项目概述

这是一个自动获取和展示 GitHub 趋势项目的全栈应用程序，能够每天自动抓取热门项目，生成 Markdown 报告，并通过 Web 界面以卡片形式展示。项目包含完整的前后端架构、定时任务系统和测试覆盖。

## 当前状态

- ✅ 项目初始化完成
- ✅ Node.js 项目依赖安装完成
- ✅ Express 服务器搭建完成
- ✅ GitHub 趋势数据获取模块实现
- ✅ Markdown 报告生成器实现
- ✅ 定时任务系统实现（每天 8:30 自动运行）
- ✅ 响应式前端界面完成
- ✅ 基础测试框架搭建
- 🔄 优化中：性能优化和错误处理增强

## 技术栈

- **后端**: Node.js + Express.js 5.x
- **前端**: HTML5 + CSS3 + JavaScript + Bootstrap 5
- **数据源**: GitHub Trending 页面 (通过 Cheerio 解析)
- **任务调度**: Node-cron 4.x
- **HTTP 客户端**: Axios 1.x
- **测试框架**: Jest + Supertest
- **环境配置**: dotenv

## 项目结构

```
D:\Projects\testone\github-trending-monitor\
├── .iflow\                    # iFlow 配置文件
├── .mcp\                      # MCP 配置文件
├── public\                    # 前端静态资源
│   ├── css\style.css         # 自定义样式
│   ├── js\app.js             # 前端 JavaScript
│   └── index.html            # 主页面
├── services\                  # 后端服务模块
│   ├── githubService.js      # GitHub 数据获取服务
│   ├── markdownGenerator.js  # Markdown 报告生成器
│   └── scheduler.js          # 定时任务调度器
├── tests\                     # 测试文件
│   ├── githubService.test.js
│   ├── markdownGenerator.test.js
│   └── server.test.js
├── .gitignore                 # Git 忽略文件
├── IFLOW.md                   # 项目文档
├── jest.config.js            # Jest 测试配置
├── package.json              # 项目配置和依赖
├── README.md                 # 项目说明
└── server.js                 # Express 服务器入口
```

## 已实现功能

### 核心功能
- [x] **自动数据获取**：每天定时从 GitHub 获取趋势项目
- [x] **智能报告生成**：自动生成格式化的 Markdown 报告
- [x] **定时任务系统**：每天 8:30 自动生成报告，9:30 发送提醒
- [x] **Web 界面**：响应式设计，支持多设备访问
- [x] **卡片展示**：以直观的卡片形式展示项目信息
- [x] **缓存机制**：1小时缓存，减少 API 请求
- [x] **错误处理**：完善的错误处理和后备方案
- [x] **健康检查**：系统健康状态监控 API

### API 接口
- `GET /` - 主页面
- `GET /api/health` - 服务器健康检查
- `GET /api/trending` - 获取趋势项目数据
- `GET /api/cache-status` - 获取缓存状态
- `GET /api/github-health` - GitHub 连接健康检查
- `GET /api/reports` - 获取历史报告列表
- `GET /api/report/:fileName` - 读取指定报告内容

## 当前依赖

```json
{
  "dependencies": {
    "axios": "^1.12.2",
    "cheerio": "^1.1.2",
    "dotenv": "^17.2.3",
    "express": "^5.1.0",
    "node-cron": "^4.2.1"
  },
  "devDependencies": {
    "jest": "^29.7.0",
    "supertest": "^7.1.4"
  }
}
```

## 运行命令

```bash
# 安装依赖
npm install

# 启动生产服务器
npm start

# 运行测试
npm test

# 运行测试（监视模式）
npm run test:watch

# 启动 MCP 服务器
npm run mcp:start

# 查看 MCP 列表
npm run mcp:dev
```

## 功能特性

### 已实现功能
1. **自动数据获取**：每天定时从 GitHub 获取趋势项目
2. **智能报告生成**：自动生成包含统计信息的 Markdown 报告
3. **定时任务系统**：自动执行报告生成和提醒任务
4. **Web 界面**：基于 Bootstrap 5 的响应式设计
5. **卡片展示**：以直观的卡片形式展示项目信息
6. **缓存机制**：智能缓存减少 API 请求
7. **错误处理**：完善的错误处理和模拟数据后备
8. **历史报告**：保存和管理历史趋势报告
9. **健康检查**：系统和服务健康状态监控

### 数据来源
- GitHub Trending 页面 (https://github.com/trending)
- 项目仓库信息
- 实时统计数据

## 开发注意事项

- 遵循 RESTful API 设计原则
- 实现适当的错误处理机制
- 确保代码可维护性和可扩展性
- 添加必要的日志记录
- 考虑 API 请求限制和缓存策略
- 使用环境变量管理配置
- 编写全面的单元测试

## 更新日志

### 2025-12-13
- 文档更新以反映当前项目状态
- 所有核心功能已实现并运行

### 2025-10-14
- 项目初始化
- 创建基础项目结构
- 制定开发计划

## 环境配置

可以通过 `.env` 文件配置以下环境变量：
- `PORT` - 服务器端口（默认：3000）

## 测试

项目使用 Jest 进行单元测试，测试覆盖：
- GitHub 服务功能测试
- Markdown 生成器测试
- Express 服务器 API 测试

运行 `npm test` 执行所有测试，或使用 `npm run test:watch` 启动监视模式。