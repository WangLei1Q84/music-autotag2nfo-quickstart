# music-autotag2nfo Quickstart

这个仓库不是源码仓库，它更像一个面向最终用户的交付入口。

它的目标很直接：
- 先让用户明白这个软件值不值得用
- 再让用户用最短路径把镜像跑起来
- 最后把增强版激活和日常操作讲清楚

主项目源码仓库：
- <https://github.com/WangLei1Q84/music-autotag2nfo>

## 这个软件能给用户带来什么

它不是单纯“把音乐文件扫一遍”的工具。

它更像一个云盘音乐整理控制台，把这些原本分散的事情收进一个地方：
- 数据源接入
- 音乐识别与补跑
- 歌词、封面、元数据补齐
- 输出目录整理
- 任务状态查看
- 人工复核与修正

对用户来说，最直接的价值是三件事：
- 不再靠一堆零散脚本和手工目录操作来维护云盘音乐库
- 不再只能“盲跑”，而是能看到哪里缺歌词、哪里缺封面、哪里要人工处理
- 不再把识别、整理、补资料、任务状态分散在不同工具里

## 它解决的典型痛点

如果你手里已经有一批云盘音乐，常见痛点通常是这些：
- 来源很多，输出目录越来越乱
- 自动识别结果参差不齐，但又不知道该先修哪里
- 歌词、封面、元数据经常缺一块，媒体库展示不完整
- 想批量处理，却缺少一个统一的控制台看全局状态
- 任务跑过了也不放心，不知道哪些结果值得信、哪些需要人工复核

这个项目的意义，不是“又多一个命令”，而是把这些痛点收敛成一个能看、能管、能补、能修的界面。

## 用户第一次打开后会看到什么

启动后，你会得到一个 Web 控制台：
- 控制台：`http://127.0.0.1:8000/app`
- 健康检查：`http://127.0.0.1:8000/api/healthz`

控制台里最重要的几块页面是：
- `总览`
  - 先看整体状态，判断今天先补资料、清积压，还是直接人工修正
- `数据源`
  - 把扫描入口、输出目录和来源配置接进来
- `待处理列表`
  - 看新增、缺失和需要补跑的内容
- `任务中心`
  - 看最近执行了什么、哪里卡住了
- `轨迹目录`
  - 逐首检查和修正结果
- `规则配置`
  - 管理识别规则和补跑策略
- `自动化设置`
  - 管理授权和自动任务

完整操作流程和真实截图见：
- [用户使用手册](./manual/user-guide.md)

## 安装和启动，只占很小一部分

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

如果返回 `{"status":"ok", ...}`，说明服务已经起来了。

## 增强版怎么理解

增强版不是另一套部署系统。

你还是用同一个 [compose.yaml](./compose.yaml)，只是当你拿到 license 后，再补齐：
- `STRM_LICENSE_INSTANCE_IP`
- `STRM_LICENSE_INSTANCE_MARK`
- 控制台里的 license 录入

增强版适合这些场景：
- 自动结果能用，但还想进一步提高识别质量
- 需要手工维护元数据
- 需要批量增强任务
- 希望补在线歌词
- 希望补在线封面

详细说明见：
- [增强能力说明](./advanced/features.md)
- [增强激活说明](./quickstart/enhanced.md)

## 推荐阅读顺序

如果你是第一次接触这个项目，推荐按这个顺序看：
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
