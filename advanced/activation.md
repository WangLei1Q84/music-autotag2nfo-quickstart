# 增强版激活说明

增强版激活分成两层：

1. 容器启动时带上实例绑定信息。
2. 控制台里录入 license。

两层都对上，增强版才会生效。

## 1. 需要的输入项

你需要准备：
- 部署实例 IP
- 部署实例标识
- 离线 license

## 2. 为什么不需要再放公钥

当前官方镜像已经内置授权公钥。

客户部署时不需要再额外挂载 `public.pem`。

## 3. [compose.yaml](../compose.yaml) 里要填什么

```yaml
image: stuart1q84/music-autotag2nfo:v0.1.0
ports:
  - "8000:8000"
environment:
  STRM_LICENSE_INSTANCE_IP: "203.0.113.10"
  STRM_LICENSE_INSTANCE_MARK: "https://demo.example.com"
```

如果你改了宿主机端口，只需要改 `ports` 左侧端口。

## 4. 用哪一个命令启动

```bash
docker compose up -d
```

增强版和普通版都用同一个命令。

## 5. 控制台里怎么录

进入：
- `自动化设置 -> 授权管理`

粘贴 license 后点击：
- `保存并校验授权`

如果一切正确，界面应该显示：
- 授权状态已生效
- 当前套餐为增强版
- 已开放能力数量大于 0

## 6. 最常见的三种不生效原因

### 实例 IP 不一致

表现：
- license 绑定值和 `STRM_LICENSE_INSTANCE_IP` 不一致

### 实例标识不一致

表现：
- license 绑定值和 `STRM_LICENSE_INSTANCE_MARK` 不一致

## 7. 重要边界

这个仓库只讲“如何部署和使用增强版”。

它不负责私钥签发流程，也不建议把私钥放进公开仓库或客户服务器。私钥只应该留在签发端。
