---
name: netdisk-search
category: web-scraping
description: 网盘资源搜索 - 通过网盘搜索API搜索百度/夸克/阿里/115等14种网盘资源，支持多关键词、过滤、链接检测。Docker一键部署。
tags: [search, cloud-storage, resource, api, docker]
triggers: 搜资源/找资源/网盘搜/搜网盘/找网盘/搜片/找片/搜电影/找电影
---

# 网盘资源搜索

## 功能概述

这是一个高性能网盘资源搜索 API 服务（GitHub: fish2018/pansou, 13.3k stars），提供以下核心能力：

- **多网盘支持**：覆盖 14 种主流网盘类型
- **插件生态**：内置 80+ 搜索插件，持续更新
- **智能排序**：按相关度、时间等维度排序搜索结果
- **链接检测**：验证网盘链接是否有效，过滤失效资源
- **灵活部署**：Docker 一键部署，支持本地和云端

## 快速开始

### Docker 一键部署

```bash
# 启动搜索服务
docker run -d --name netdisk-search -p 8888:8888 ghcr.io/fish2018/pansou:latest

# 查看服务状态
docker logs netdisk-search

# 停止服务
docker stop netdisk-search && docker rm netdisk-search
```

### 使用方式

1. **命令行搜索**：使用 `search.py` 脚本
2. **API 调用**：直接调用 REST API
3. **链接检测**：使用 `check_links.py` 验证链接有效性
4. **一键部署**：使用 `deploy.sh` 自动化部署

## API 信息

### 基础端点

- **搜索 API**：`POST /api/search`
- **链接检测**：`POST /api/check/links`
- **健康检查**：`GET /api/health`

### 搜索参数

```json
{
  "kw": "关键词",           // 必需，搜索关键词
  "cloud_types": ["baidu", "aliyun"],  // 可选，过滤网盘类型
  "filter": true,           // 可选，启用过滤
  "src": "all",            // 可选，数据源
  "plugins": [],           // 可选，指定插件
  "refresh": false         // 可选，强制刷新缓存
}
```

### 链接检测参数

```json
{
  "items": ["https://pan.baidu.com/s/xxx", "https://www.aliyundrive.com/s/xxx"],
  "proxy_url": ""          // 可选，代理地址
}
```

### 响应格式

```json
{
  "code": 0,
  "message": "success",
  "data": [
    {
      "title": "资源标题",
      "url": "https://pan.baidu.com/s/xxx",
      "cloud_type": "baidu",
      "time": "2024-01-01",
      "source": "plugin_name"
    }
  ]
}
```

## 脚本使用

### search.py - 搜索脚本

```bash
# 基础搜索
python search.py "关键词"

# 指定网盘类型
python search.py "关键词" --types baidu,aliyun

# 输出 JSON 格式
python search.py "关键词" --json

# 限制结果数量
python search.py "关键词" --limit 20
```

### check_links.py - 链接检测

```bash
# 检测单个链接
python check_links.py "https://pan.baidu.com/s/xxx"

# 批量检测
python check_links.py --file links.txt

# 使用代理
python check_links.py "https://pan.baidu.com/s/xxx" --proxy socks5://127.0.0.1:1080
```

### deploy.sh - 部署脚本

```bash
# 自动部署搜索服务
./deploy.sh

# 指定端口
./deploy.sh --port 9000

# 启用认证
./deploy.sh --auth --secret your-secret-key
```

## 环境变量配置

| 变量名 | 说明 | 默认值 |
|--------|------|--------|
| `NETDISK_API_URL` | API 服务地址 | `http://localhost:8888` |
| `AUTH_ENABLED` | 启用认证 | `false` |
| `AUTH_SECRET` | JWT 密钥 | - |
| `PROXY_URL` | 代理地址 | - |
| `LOG_LEVEL` | 日志级别 | `INFO` |

**地址优先级**：
1. 环境变量 `NETDISK_API_URL`
2. `localhost:8888`（本地默认）
3. 公共 fallback（仅限测试）

## 支持的网盘类型

| 网盘 | 类型标识 |
|------|----------|
| 百度网盘 | `baidu` |
| 阿里云盘 | `aliyun` |
| 夸克网盘 | `quark` |
| 天翼云盘 | `tianyi` |
| UC网盘 | `uc` |
| 移动云盘 | `mobile` |
| 115网盘 | `115` |
| PikPak | `pikpak` |
| 迅雷网盘 | `xunlei` |
| 123网盘 | `123` |
| 磁力链接 | `magnet` |
| 电驴链接 | `ed2k` |
| 更多... | 持续更新 |

## 故障排查

### 常见问题

**Q: 连接被拒绝**
```bash
# 检查服务是否运行
docker ps | grep netdisk-search

# 检查端口
netstat -tlnp | grep 8888

# 查看日志
docker logs netdisk-search
```

**Q: 搜索结果为空**
- 确认关键词是否正确
- 尝试使用更通用的关键词
- 检查网络连接

**Q: 认证失败**
```bash
# 检查认证配置
docker exec netdisk-search env | grep AUTH

# 重新生成 token
curl -X POST http://localhost:8888/api/auth/token -d '{"secret":"your-secret"}'
```

**Q: 链接检测超时**
- 检查网络连接
- 配置代理：`PROXY_URL=socks5://127.0.0.1:1080`
- 减少并发检测数量

## 与其他 Skill 的关系

### netdisk-search vs pansou-resource-scraper

- **netdisk-search**：底层通用能力，提供原始搜索 API 和工具
- **pansou-resource-scraper**：基于 pansou-search 的业务应用，提供更高级的功能（如自动分类、资源库管理等）

**使用建议**：
- 需要快速搜索：直接使用 netdisk-search
- 需要构建资源库：使用 pansou-resource-scraper

## 最佳实践

1. **关键词优化**：使用 2-4 个关键词，避免过于宽泛
2. **类型过滤**：明确指定网盘类型可提高搜索速度
3. **缓存利用**：默认启用缓存，避免重复请求
4. **错误处理**：实现重试机制，处理网络波动
5. **资源验证**：重要资源建议先检测链接有效性

## 相关链接

- GitHub: https://github.com/fish2018/pansou
- 文档: https://github.com/fish2018/pansou#readme
- 问题反馈: https://github.com/fish2018/pansou/issues
