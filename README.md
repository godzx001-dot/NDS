# NDS - NetDisk Search 📁

网盘资源搜索工具，基于 [开源搜索 API](https://github.com/fish2018/pansou)，支持 **14 种网盘类型**、**80+ 搜索插件**，Docker 一键部署。

## ✨ 特性

- 🔍 **多网盘搜索**：百度、夸克、阿里、天翼、UC、115、PikPak、迅雷、123、磁力...
- 🧩 **80+ 插件**：覆盖主流资源站，持续更新
- 🔗 **链接检测**：批量验证网盘链接是否有效
- 🐳 **一键部署**：Docker 开箱即用
- 📊 **智能排序**：按时间、相关度、插件等级综合排序
- 🔐 **认证支持**：可选 JWT 认证保护

## 🚀 快速开始

### 1. 部署

```bash
# 一键部署脚本
bash scripts/deploy.sh

# 或手动 Docker
docker run -d --name netdisk-search -p 8888:8888 ghcr.io/fish2018/pansou:latest
```

### 2. 搜索资源

```bash
# 基础搜索
python3 scripts/search.py "速度与激情"

# 指定网盘类型
python3 scripts/search.py "Python教程" --types baidu,aliyun

# 过滤结果（包含"合集"，排除"预告"）
python3 scripts/search.py "唐朝诡事录" --include "合集" --exclude "预告"

# JSON 输出
python3 scripts/search.py "机器学习" --format json --limit 10
```

### 3. 检测链接

```bash
# 检测单个链接
python3 scripts/check_links.py --url "https://pan.quark.cn/s/xxx"

# 批量检测
python3 scripts/check_links.py --file links.txt
```

## 📂 文件结构

```
NDS/
├── SKILL.md                    # Skill 完整文档
├── README.md                   # 本文件
├── scripts/
│   ├── search.py               # 搜索脚本（终端/JSON/Markdown 输出）
│   ├── check_links.py          # 链接有效性检测
│   └── deploy.sh               # Docker 一键部署
├── references/
│   └── api-reference.md        # API 完整参考文档
└── templates/
```

## 🌐 支持的网盘类型

| 网盘 | 标识 | | 网盘 | 标识 |
|------|------|-|------|------|
| 百度网盘 | `baidu` | | 115网盘 | `115` |
| 阿里云盘 | `aliyun` | | PikPak | `pikpak` |
| 夸克网盘 | `quark` | | 迅雷网盘 | `xunlei` |
| 天翼云盘 | `tianyi` | | 123网盘 | `123` |
| UC网盘 | `uc` | | 磁力链接 | `magnet` |
| 移动云盘 | `mobile` | | 电驴链接 | `ed2k` |

## ⚙️ 环境变量

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `NETDISK_API_URL` | API 服务地址 | 自动探测 |
| `AUTH_ENABLED` | 启用认证 | `false` |
| `AUTH_USERS` | 用户配置 | - |
| `PROXY_URL` | 代理地址 | - |

## 📖 API 端点

| 端点 | 方法 | 说明 |
|------|------|------|
| `/api/search` | POST/GET | 搜索资源 |
| `/api/check/links` | POST | 检测链接有效性 |
| `/api/health` | GET | 健康检查 |
| `/api/auth/login` | POST | 登录获取 Token |

详见 [API Reference](references/api-reference.md)

## 🛠️ 使用场景

- **找资源**：搜索电影、电视剧、学习资料、软件
- **验链接**：批量检测网盘分享链接是否失效
- **搭服务**：自建网盘搜索 API，集成到自己的工具中

## 📄 许可证

MIT License

## 🙏 致谢

- [fish2018/pansou](https://github.com/fish2018/pansou) - 高性能网盘搜索 API 服务
