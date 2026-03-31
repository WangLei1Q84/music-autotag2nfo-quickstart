# 升级与回滚

这套部署的升级思路应该很朴素：

1. 不直接赌 `latest`
2. 先记住当前版本
3. 先备份数据
4. 再切镜像版本

## 1. 第一次正式使用时就该做的事

把 [compose.yaml](../compose.yaml) 里的 `image` 固定成明确版本，例如：

```yaml
image: stuart1q84/music-autotag2nfo:v0.1.0
```

## 2. 升级前要备份什么

至少备份：
- `./data`

如果你在运行增强版，也确认下面两项没问题：
- 当前 license 绑定信息

## 3. 升级步骤

1. 修改 [compose.yaml](../compose.yaml) 里的 `image`
2. 拉取新镜像
3. 重启容器

```bash
docker compose pull
docker compose up -d
```

## 4. 升级后验证

验证顺序不要乱：
1. `docker compose ps`
2. `curl http://127.0.0.1:8000/api/healthz`
3. 打开 `/app`
4. 如果是增强版，再检查 `自动化设置 -> 授权管理`

## 5. 回滚

如果新版本有问题，最简单的回滚方式就是把 [compose.yaml](../compose.yaml) 里的 `image` 改回旧版本，再重新执行：

```bash
docker compose up -d
```

## 6. 一个很实际的建议

正式环境里，不要把“镜像升级”和“增强激活”放在同一次变更里做。

先升级镜像，确认服务稳定。再做 license 相关调整。这样出了问题，你能更快知道到底是哪一步炸了。
