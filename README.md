# music-autotag2nfo Quickstart

一个给云盘音乐库用的整理控制台。

如果你的音乐文件主要放在远程服务器、NAS、AList 或云盘里，不方便用本地标签工具慢慢改，这个项目就是为这种场景准备的。

它把原本分散的几件事收进一个网页里：
- 接入数据源
- 扫描远程目录
- 识别和整理歌曲
- 补歌词、补封面、补元数据
- 查看任务状态
- 找出哪些结果值得人工再看一眼


## 为什么会需要它

很多人的问题不是“没有音乐文件”，而是文件越来越多以后，整理这件事开始变得不顺手。

常见情况基本是这些：
- 音乐都在远端，想改标签、补资料、重跑任务，不想来回拷到本地
- 自动跑完以后，不知道哪些结果能直接用，哪些最好先人工确认
- 歌词、封面、元数据总有一块不完整，媒体库看起来始终差一点
- 想批量处理时，只能翻目录、看日志、重跑命令，过程很散

这个项目就是把这些动作集中起来，让你先看状态，再做操作。

## 它能做什么

启动后，你会得到一个 Web 控制台：
- 控制台：`http://127.0.0.1:8000/app`
- 健康检查：`http://127.0.0.1:8000/api/healthz`

最常用的页面有这些：
- `总览`
  - 先看整体状态和今天优先要处理的问题
- `数据源`
  - 接入来源，管理输出目录和启停状态
- `待处理列表`
  - 看新增、缺失和需要重处理的内容
- `任务中心`
  - 看最近执行了什么，哪里失败，哪里需要重试
- `轨迹目录`
  - 逐首复核结果
- `规则配置`
  - 管理识别规则和补跑策略
- `自动化设置`
  - 管理授权和自动任务

真实页面和操作流程见：
- [用户使用手册](./manual/user-guide.md)

## 它不是什么

这个项目不是播放器，也不是单纯只跑一次的导入脚本。

它更适合“长期维护一套音乐库”的场景，而不是临时整理几首歌。

## 适合谁

比较适合这些人：
- 已经有一批云盘音乐，想把整理过程做得稳定一点
- 不想继续靠零散脚本和手工目录操作维护库
- 希望有一个能看全局状态，也能落到单首问题上的控制台

如果你只是偶尔改几首歌的标签，或者本来就不在意歌词、封面、元数据这些细节，那它未必是你现在最需要的工具。

## 安装其实很短

默认部署入口只有一个：
- [compose.yaml](./compose.yaml)

普通版和增强版共用同一套部署参数。

增强是否生效，不取决于你换了另一套 `docker compose` 文件，而取决于两件事：
1. 你有没有拿到 license。
2. 你有没有在控制台里完成录入和校验。

### 3 分钟启动

1. 装好 Docker 和 Docker Compose
2. 按需检查 [compose.yaml](./compose.yaml)
3. 执行：

```bash
docker compose up -d
```

4. 验证：

```bash
docker compose ps
python - <<'PY'
import urllib.request
print(urllib.request.urlopen('http://127.0.0.1:8000/api/healthz', timeout=10).read().decode())
PY
```

只要返回 `{"status":"ok", ...}`，服务就已经起来了。

## 增强版是干什么的

增强版不是另一套安装方式。

它还是同一个 [compose.yaml](./compose.yaml)，只是当你拿到 license 后，再补齐：
- `STRM_LICENSE_INSTANCE_IP`
- `STRM_LICENSE_INSTANCE_MARK`
- 控制台里的 license 录入

它更适合这些情况：
- 自动结果大体能用，但你还想把识别质量再往上提
- 你需要手工维护元数据
- 你在意歌词和封面完整度
- 你希望批量增强任务更省事

当前项目刚开始推广，镜像里已经内置了一个 1 个月有效期的增强版试用 license。

欢迎大家先试用，再通过仓库 Issue 提建议或反馈使用体验。

如果你想先试试看增强版是不是适合自己，可以在仓库 Issue 里联系我。

我可以提供一个 1 个月有效期的 license，供免费试用。

详细说明见：
- [增强能力说明](./advanced/features.md)
- [增强激活说明](./quickstart/enhanced.md)

## 推荐怎么开始看

如果你是第一次接触这个项目，建议按这个顺序：
1. [用户使用手册](./manual/user-guide.md)
2. [首次启动说明](./quickstart/free.md)
3. [增强激活说明](./quickstart/enhanced.md)
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
- `manual/user-guide.md`
  - 面向最终使用者的操作手册，包含真实截图
- `manual/screenshots/`
  - 手册配图
- `quickstart/free.md`
  - 首次启动说明
- `quickstart/enhanced.md`
  - 增强激活说明
- `advanced/features.md`
  - 普通版和增强版能力对照
- `advanced/activation.md`
  - 授权和实例绑定说明
- `deploy/storage-layout.md`
  - 数据目录和备份建议
- `deploy/upgrade.md`
  - 升级、回滚和版本固定建议
- `faq/troubleshooting.md`
  - 常见启动和激活问题
