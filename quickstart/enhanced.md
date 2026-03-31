# 增强激活说明

增强版不是另一套部署系统。

你还是使用同一个 [compose.yaml](../compose.yaml)，只是把增强版需要的参数补齐，然后在控制台里录入 license。

## 1. 你需要准备什么

通常你会从交付方拿到这些内容：
- Docker 镜像标签，例如 `stuart1q84/music-autotag2nfo:v0.1.0`
- 一串离线 license
- 部署实例 IP
- 部署实例标识

如果这些东西还没到手，先不要动增强参数，先联系交付方。

说明：
- 官方镜像已经内置授权公钥
- 客户部署时不需要再额外挂载 `public.pem`

## 2. 修改 [compose.yaml](../compose.yaml)

把下面这些位置改成你的实际值：

```yaml
image: stuart1q84/music-autotag2nfo:v0.1.0
STRM_LICENSE_INSTANCE_IP: "203.0.113.10"
STRM_LICENSE_INSTANCE_MARK: "https://demo.example.com"
```

说明：
- `image` 建议改成明确版本，不建议继续用 `latest`
- `STRM_LICENSE_INSTANCE_IP` 要和 license 绑定值一致
- `STRM_LICENSE_INSTANCE_MARK` 要和 license 绑定值一致

## 3. 启动或重启

```bash
docker compose up -d
```

## 4. 验证服务

```bash
docker compose ps
curl http://127.0.0.1:8000/api/healthz
```

## 5. 在控制台里录入 license

进入：
- `自动化设置 -> 授权管理`

把 license 粘贴进去，然后点击：
- `保存并校验授权`

如果部署参数和 license 一致，状态应该变成：
- 已生效
- 增强版

## 6. 什么时候值得启用增强版

如果你已经遇到这些需求，就值得升级：
- 希望识别质量更高
- 需要手工维护元数据
- 需要批量增强任务
- 希望补在线歌词
- 希望补在线封面

详细对照见：
- [增强能力说明](../advanced/features.md)

## 7. 卡住时先看哪里

如果你在这里卡住，先看：
- [增强激活细节](../advanced/activation.md)
- [常见问题排查](../faq/troubleshooting.md)
