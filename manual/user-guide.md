# 用户使用手册

这份手册面向第一次部署、第一次激活、第一次日常使用的用户。

说明：
- 当前仓库附带的两张图片是操作示意图，用来帮助用户快速定位入口。
- 拿到可运行镜像后，建议再替换成真实界面截图。

如果你只想先把服务跑起来，先看：
- [首次启动说明](../quickstart/free.md)

如果你准备启用增强版，再看：
- [增强激活说明](../quickstart/enhanced.md)

## 1. 你会用到什么

默认访问地址：
- 控制台：`http://127.0.0.1:8000/app`
- 健康检查：`http://127.0.0.1:8000/api/healthz`

默认本地目录：
- `./data`

## 2. 首次启动

### 步骤 1：检查 [compose.yaml](../compose.yaml)

确认这几个位置符合你的环境：
- `image`
- `ports`
- `STRM_LICENSE_INSTANCE_IP`
- `STRM_LICENSE_INSTANCE_MARK`

如果你现在还没启用增强版，后两个值可以先留空。

### 步骤 2：启动服务

```bash
docker compose up -d
```

### 步骤 3：确认服务正常

```bash
docker compose ps
curl http://127.0.0.1:8000/api/healthz
```

成功时你应该能看到 `{"status":"ok"}`。

### 步骤 4：打开控制台

访问：
- <http://127.0.0.1:8000/app>

控制台首页示意图：

![控制台首页](./screenshots/app-home.png)

## 3. 启用增强版

### 步骤 1：补齐绑定参数

编辑 [compose.yaml](../compose.yaml)，填写：
- `STRM_LICENSE_INSTANCE_IP`
- `STRM_LICENSE_INSTANCE_MARK`

建议同时把 `image` 固定到明确版本号。

说明：
- 官方镜像已经内置授权公钥
- 客户部署时不需要再额外挂载 `public.pem`

### 步骤 2：重启服务

```bash
docker compose up -d
```

### 步骤 3：录入 license

打开控制台后进入：
- `自动化设置 -> 授权管理`

把 license 粘贴进去，再点击保存。

授权页示意图：

![授权管理页](./screenshots/license-page.png)

## 4. 日常使用时最常做的事

### 查看服务状态

```bash
docker compose ps
```

### 查看实时日志

```bash
docker compose logs -f
```

### 停止服务

```bash
docker compose down
```

### 重新启动服务

```bash
docker compose up -d
```

## 5. 升级与回滚

最稳的做法是：
1. 先备份 `./data`
2. 把 [compose.yaml](../compose.yaml) 里的 `image` 改成目标版本
3. 执行 `docker compose pull`
4. 执行 `docker compose up -d`

详细说明见：
- [升级与回滚](../deploy/upgrade.md)

## 6. 排障顺序

当你觉得“不对劲”的时候，不要先猜前端问题。按这个顺序查：

1. `docker compose ps`
2. `docker compose logs -f`
3. `http://127.0.0.1:8000/api/healthz`
4. `http://127.0.0.1:8000/app`
5. 如果是增强版，再检查 license 绑定值和当前镜像版本

详细说明见：
- [常见问题排查](../faq/troubleshooting.md)
