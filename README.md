# Dockhand - 懒猫云应用

Docker 管理工具，提供友好的 Web 界面来管理你的容器、镜像和网络。

## 📋 应用信息

- **名称**: Dockhand
- **包名**: cloud.lazycat.app.dockhand
- **版本**: 1.0.0
- **主页**: https://github.com/fnsys/dockhand

## ✨ 功能特性

- 🐳 容器管理：启动、停止、删除容器
- 📦 镜像管理：拉取、删除镜像
- 🔗 网络管理：查看和管理 Docker 网络
- 📊 资源监控：实时查看容器资源使用情况
- 🎨 友好界面：简洁直观的 Web 界面

## 🚀 快速开始

### 方式一：从懒猫应用商店安装

1. 打开懒猫云控制台
2. 进入应用商店
3. 搜索 "Dockhand"
4. 点击安装

### 方式二：本地构建安装

#### 前置要求

- 安装 lzc-cli 工具
- 准备 512x512 PNG 格式的应用图标（命名为 `icon.png`）

#### 构建步骤

```bash
# 1. 准备图标（必需）
# 请将 512x512 PNG 图标复制到此目录并命名为 icon.png

# 2. 使用自动化脚本
./build.sh
# 选择 "1. 构建应用"

# 或者手动构建
lzc-cli project build -o dockhand-1.0.0.lpk

# 3. 安装到懒猫云
lzc-cli app install dockhand-1.0.0.lpk
```

## 📦 发布到应用商店

### 使用自动化脚本（推荐）

```bash
./build.sh
# 选择 "4. 一键构建+镜像复制+发布"
```

这将自动执行以下步骤：
1. ✅ 构建应用
2. ✅ 复制镜像到懒猫仓库
3. ✅ 更新 manifest 文件
4. ✅ 重新构建
5. ✅ 发布到应用商店

### 手动步骤

```bash
# 1. 登录应用商店
lzc-cli appstore login

# 2. 复制镜像到懒猫仓库
lzc-cli appstore copy-image fnsys/dockhand:latest

# 3. 更新 lzc-manifest.yml 中的镜像地址
# 将输出的新镜像地址更新到 manifest 文件

# 4. 重新构建
lzc-cli project build -o dockhand-1.0.0.lpk

# 5. 发布
lzc-cli appstore publish dockhand-1.0.0.lpk
```

## 🔧 配置说明

### compose_override

此应用需要访问 Docker socket 来管理容器，因此在 `lzc-build.yml` 中配置了 `compose_override`:

```yaml
compose_override:
  services:
    dockhand-service:
      volumes:
        - /var/run/docker.sock:/var/run/docker.sock
```

这允许 Dockhand 与宿主机的 Docker daemon 通信。

### 存储

- **数据目录**: `/lzcapp/var/dockhand` - 存储应用数据和配置

### 资源限制

- **CPU**: 512 shares
- **内存**: 512MB

## 🌐 访问应用

安装后，通过以下方式访问：

```
https://dockhand.你的懒猫云域名
```

## 📁 文件结构

```
dockhand-lzcapp/
├── lzc-manifest.yml    # 应用配置文件
├── lzc-build.yml       # 构建配置文件
├── build.sh            # 自动化构建脚本
├── icon.png            # 应用图标（需要自行准备）
└── README.md           # 本文件
```

## ⚙️ 技术细节

### 服务架构

- **dockhand-service**: 主服务
  - 镜像: `fnsys/dockhand:latest`
  - 端口: 3000
  - 需要挂载 Docker socket

### 网络

- 使用 `upstreams` 配置 HTTP 路由
- 根路径 `/` 代理到服务端口 3000

## 🔒 安全注意事项

Dockhand 需要访问 Docker socket，这意味着：

⚠️ **重要**: 此应用具有管理所有容器的权限，请确保：
- 仅授权可信用户访问
- 定期检查应用日志
- 及时更新到最新版本

## 🐛 故障排除

### 应用无法启动

1. 检查 Docker socket 是否可访问
2. 确认资源限制是否足够
3. 查看应用日志：`lzc-cli app logs dockhand`

### 无法管理容器

1. 确认 compose_override 配置正确
2. 检查 Docker socket 权限
3. 重启应用尝试

## 📚 更多信息

- **原项目**: https://github.com/fnsys/dockhand
- **懒猫云文档**: https://developer.lazycat.cloud
- **问题反馈**: 请在原项目或懒猫云社区提交 issue

## 📝 更新日志

### v1.0.0 (初始版本)
- ✅ 基本的容器管理功能
- ✅ 镜像管理功能
- ✅ 网络管理功能
- ✅ Web 界面

## 📄 许可证

MIT License

## 👥 作者

- **原作者**: fnsys
- **懒猫应用适配**: Your Name

---

**注意**: 请在构建前准备好 512x512 的 PNG 格式图标文件！
# dockhand-lzcapp
