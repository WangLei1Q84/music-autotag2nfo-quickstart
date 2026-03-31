# music-autotag2nfo Quickstart

这个仓库只做一件事，帮你把官方 Docker 镜像快速跑起来，并把“普通版”和“增强版”的使用路径讲清楚。

主项目源码仓库：
- <https://github.com/WangLei1Q84/music-autotag2nfo>

## 先说结论

这套部署现在只有一个默认入口：
- [compose.yaml](./compose.yaml)

普通版和增强版共用同一套部署参数。

增强是否生效，不取决于你换了另一套 `docker compose` 文件，而取决于两件事：
1. 你有没有从我这里拿到 license。
2. 你有没有按流程把公钥和 license 录入完成。

## 你会得到什么

服务启动后，你可以直接访问：
- 控制台：`http://127.0.0.1:8000/app`
- 健康检查：`http://127.0.0.1:8000/api/healthz`

当前镜像的运行模型：
- 单机
- 单用户
- SQLite
- 前端由 FastAPI 托管在 `/app`

## 3 分钟启动

### 1. 准备环境

你只需要先装好：
- Docker
- Docker Compose

### 2. 按需修改 [compose.yaml](./compose.yaml)

默认情况下，仓库已经可以直接启动，不需要额外复制 `.env`。

你通常只会改这几处：
- `image`
  - 首次体验可以先保留 `latest`
  - 正式环境建议改成明确版本号，比如 `stuart1q84/music-autotag2nfo:v0.1.0`
- `ports`
  - 默认是 `8000:8000`
  - 如果宿主机端口冲突，就把左侧端口改掉，比如 `8040:8000`
- `STRM_LICENSE_INSTANCE_IP`
  - 只有你准备启用增强版时才需要填写
- `STRM_LICENSE_INSTANCE_MARK`
  - 只有你准备启用增强版时才需要填写

### 3. 启动

```bash
docker compose up -d
```

### 4. 验证

```bash
docker compose ps
curl http://127.0.0.1:8000/api/healthz
```

如果返回：

```json
{"status":"ok"}
```

说明服务已经起来了。

### 5. 打开控制台

浏览器访问：
- <http://127.0.0.1:8000/app>

## 增强版怎么理解

增强版不是另一套部署系统。

你还是启动同一个 [compose.yaml](./compose.yaml)，只是当你拿到 license 后，再补齐下面这些内容：
- `STRM_LICENSE_INSTANCE_IP`
- `STRM_LICENSE_INSTANCE_MARK`
- 控制台里的 license 录入

详细说明见：
- [增强能力说明](./advanced/features.md)
- [增强激活说明](./advanced/activation.md)

## 文档顺序

推荐按这个顺序看：
1. [首次启动说明](./quickstart/free.md)
2. [增强激活说明](./quickstart/enhanced.md)
3. [用户使用手册](./manual/user-guide.md)
4. [增强能力说明](./advanced/features.md)
5. [升级与回滚](./deploy/upgrade.md)
6. [常见问题排查](./faq/troubleshooting.md)

## 最常用命令

启动：

```bash
docker compose up -d
```

查看状态：

```bash
docker compose ps
```

查看日志：

```bash
docker compose logs -f
```

停止：

```bash
docker compose down
```

## 仓库结构

- `compose.yaml`
  - 唯一默认部署入口
- `quickstart/free.md`
  - 首次启动说明
- `quickstart/enhanced.md`
  - 增强激活说明
- `manual/user-guide.md`
  - 面向最终使用者的操作手册
- `manual/screenshots/`
  - 手册配图
- `advanced/features.md`
  - 普通版和增强版能力对照
- `advanced/activation.md`
  - 公钥挂载、实例绑定和 license 录入细节
- `deploy/storage-layout.md`
  - 数据目录和备份建议
- `deploy/upgrade.md`
  - 升级、回滚和版本固定建议
- `faq/troubleshooting.md`
  - 常见启动和激活问题
