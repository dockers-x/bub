# Bub Docker Auto Build

自动构建 [bubbuild/bub](https://github.com/bubbuild/bub) 的 Docker 镜像，同步推送到 Docker Hub 和 GitHub Container Registry。

## 功能

- **每日自动构建**: 每天 UTC 00:00 自动检查源仓库更新并构建最新镜像
- **智能跳过**: 如果没有新的提交，自动跳过构建以节省资源
- **多架构支持**: 支持 `linux/amd64` 和 `linux/arm64`
- **双 Registry 推送**: 同时推送到 Docker Hub 和 GHCR
- **手动触发**: 支持手动触发构建

## 使用方法

### 拉取镜像

**Docker Hub:**
```bash
# 拉取最新版本
docker pull czyt/bub:latest

# 拉取指定日期版本
docker pull czyt/bub:20250312

# 拉取指定 commit 版本
docker pull czyt/bub:sha-abc1234
```

**GitHub Container Registry:**
```bash
# 拉取最新版本
docker pull ghcr.io/<owner>/bub:latest

# 拉取指定日期版本
docker pull ghcr.io/<owner>/bub:20250312
```

### 运行容器

```bash
docker run -it --rm \
  -v $(pwd):/workspace \
  -v ~/.bub:/root/.bub \
  czyt/bub:latest
```

### 使用 Docker Compose

```yaml
services:
  bub:
    image: czyt/bub:latest
    volumes:
      - ./:/workspace
      - bub-data:/root/.bub
    stdin_open: true
    tty: true

volumes:
  bub-data:
```

## 镜像标签

| 标签 | 说明 |
|------|------|
| `latest` | 最新构建版本 |
| `YYYYMMDD` | 按日期标记的版本，如 `20250312` |
| `sha-xxxxxx` | 按源码 commit 标记的版本 |

## 手动触发

在 GitHub 仓库页面，进入 Actions -> Build and Push Docker Image -> Run workflow，可以手动触发构建。

选项：
- **Push image to registry**: 是否推送镜像到 registry（默认 true）
- **Force build**: 强制构建，即使没有新的提交（默认 false）

## 配置要求

使用此 workflow 需要在仓库中配置以下 Secrets：

| Secret | 说明 |
|--------|------|
| `DOCKERHUB_USERNAME` | Docker Hub 用户名 |
| `DOCKERHUB_TOKEN` | Docker Hub Access Token |

### 创建 Docker Hub Token

1. 登录 [Docker Hub](https://hub.docker.com)
2. 进入 Account Settings -> Security
3. 点击 "New Access Token"
4. 选择 "Read, Write, Delete" 权限
5. 复制生成的 Token

## 相关链接

- [Bub 官网](https://bub.build)
- [源码仓库](https://github.com/bubbuild/bub)
- [Bub 文档](https://bub.build)
- [Docker Hub - czyt/bub](https://hub.docker.com/r/czyt/bub)