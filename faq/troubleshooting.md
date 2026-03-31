# 常见问题排查

## 1. `docker compose up -d` 后服务没起来

先按这个顺序看：

```bash
docker compose ps
docker compose logs -f
```

重点看有没有这些问题：
- 端口被占用
- 镜像拉取失败
- 宿主机目录权限问题

如果你改过端口，确认浏览器访问的是改后的端口，不是默认 `8000`。

## 2. 健康检查没通过

先执行：

```bash
curl http://127.0.0.1:8000/api/healthz
```

如果你不是用 `8000`，把端口换掉。

如果容器刚启动，给它几十秒缓冲时间。启动初期短暂 `starting` 是正常的。

## 3. `/app` 打不开

先不要猜前端问题。

先看：
1. 容器是不是 `Up`
2. `/api/healthz` 是否正常
3. `ports` 左侧端口是不是你实际访问的端口

当前前端由 FastAPI 托管在 `/app`，不是独立前端端口。

## 4. 增强版提示“实例 IP 不一致”

说明 [compose.yaml](../compose.yaml) 里的：
- `STRM_LICENSE_INSTANCE_IP`

和 license 里的绑定值不一致。

## 5. 增强版提示“实例标识不一致”

说明 [compose.yaml](../compose.yaml) 里的：
- `STRM_LICENSE_INSTANCE_MARK`

和 license 里的绑定值不一致。

## 6. license 录入后还是普通版

先排查这四件事：
1. 实例 IP 是否一致。
2. 实例标识是否一致。
3. license 是否过期。
4. 当前镜像版本是否包含最新授权逻辑。

## 7. 正式环境建议用 `latest` 吗

不建议。

第一次试跑可以先用 `latest`。

正式环境建议在 [compose.yaml](../compose.yaml) 里把 `image` 固定到版本号，例如：

```yaml
image: stuart1q84/music-autotag2nfo:v0.1.0
```

## 8. 想升级，最稳的姿势是什么

先备份：
- `./data`

然后再改镜像版本，再执行：

```bash
docker compose pull
docker compose up -d
```

不要把“镜像升级”、“增强激活”、“改绑定信息”三件事挤在一次变更里做。
