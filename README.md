# Mineradio

![Mineradio 暗场启动页](./docs/assets/readme/cinema-beat-smoke.png)

Mineradio 是一款 Windows 桌面沉浸式音乐播放器，把天气电台、搜索播放、歌词舞台、粒子视觉和 3D 歌单架组合成一个更接近现场感的私人音乐空间。

---

## 项目地址

- **上游原项目**：[XxHuberrr/Mineradio](https://github.com/XxHuberrr/Mineradio) — Mineradio 的原始作者与主要设计者
- **当前版本**：`1.1.666`（基于上游 v1.1.1 继续开发）

---

## 新增功能

### GD Studio 第三方音乐源接入

接入 [GD Studio 音乐平台 API](https://music-api.gdstudio.xyz) 作为额外的音乐搜索与播放来源，支持自动 fallback：

- **三源轮询后备链**：网易云 → QQ 音乐 → GD Studio，任一源不可播时自动尝试下一个
- **GD Studio 内部多源轮询**：joox → bilibili → tencent → kuwo → tidal → qobuz → apple → ytmusic → spotify
- **智能跳过**：QQ / 网易云未登录时自动跳过，不弹登录框干扰
- **繁简匹配**：跨平台歌曲名自动繁简归一化，提高匹配准确率
- 详细接入文档见下方 [GD Studio API 说明](#gd-studio-api-说明)

---

## GD Studio API 说明

本软件集成了 [GD Studio 在线音乐平台 API](https://music-api.gdstudio.xyz) 作为后备音源。

- **API 提供方**：GD Studio（[gdstudio@email.com](mailto:gdstudio@email.com)）
- **授权协议**：CC BY-NC 4.0
- **使用要求**：使用本 API 时请注明出处「GD音乐台(music.gdstudio.xyz)」，尊重原作者
- **免责声明**：本站资源来自网络，仅限本人学习参考，严禁下载、传播或商用，如侵权请与我联系删除。继续使用将视为同意本声明

API 频率限制：5 分钟内不超过 50 次请求。更多信息请访问 [music-api.gdstudio.xyz](https://music-api.gdstudio.xyz)。

---

## 核心特性

- Open-Meteo 天气电台，根据当前位置、城市和天气 mood 生成更合适的播放队列
- 首页包含天气电台、每日推荐、私人电台、继续听、听歌画像和我的歌单入口
- Wallpaper 银河首页背景，未播放状态保持干净的星河氛围
- 播放后切换到 Emily / 默认播放态视觉，歌词舞台与粒子舞台同步工作
- 基于节奏的电影镜头视觉系统
- 面向长播客和 DJ 曲目的专属视觉模式
- 歌词舞台、自定义歌词、歌词位置与视觉控制
- 自定义专辑封面上传与裁剪
- 右键唤起 3D 歌单架，支持歌单队列浏览
- 网易云音乐账号、搜索、歌单、播客等体验接入
- QQ 音乐搜索、登录态与音源补充接入
- GD Studio 多源搜索与播放后备
- GitHub Releases 更新检测与下载入口
- 首次启动内置「默认测试」视觉用户存档，软件内默认视觉参数与该存档一致

---

## 开发运行

```bash
npm install
npm start            # Electron 开发模式
npm run build:win    # 构建 Windows NSIS 安装包（产物在 dist/）
```

桌面版入口由 Electron 主进程加载本地服务。

---

## 第三方音乐平台说明

Mineradio 不是网易云音乐、QQ 音乐、腾讯音乐娱乐集团或 GD Studio 的官方客户端，也不隶属于任何音乐平台。

项目中的第三方平台接入仅用于个人学习、本地客户端体验和用户自有账号的播放辅助。请遵守对应平台的用户协议、版权规则和会员权益规则。项目不会提供绕过付费、绕过会员、破解音质或重新分发音乐内容的能力。

## AI 辅助声明

本项目的部分代码、文档和配置由 AI 编程助手 **Reasonix**（基于 **deepseek-v4-flash** 模型）辅助生成。AI 在开发者指导下完成代码片段编写、Bug 修复、文档整理等工作。所有 AI 生成内容均经开发者审查和测试后合并。

---

## 用户数据与隐私

登录 Cookie、搜索历史、自定义封面、自定义歌词、节奏分析缓存等数据只应保存在本机用户数据目录或浏览器本地存储中，不应提交到仓库。

更多说明见 [PRIVACY.md](./PRIVACY.md)。

---

## 致谢

- **XxHuberrr** — Mineradio 的原始作者与主要设计打造者
- **emily** — 早期视觉底层想法与 `emily` 视觉预设改进方向的共创者和灵感来源
- **GD Studio** — 提供第三方音乐平台 API 接入支持
- **小天才e宝、应春日、锋将军、軌跡、林中、骊、风痕、花椰菜🥦** — 早期体验、测试反馈和发布准备中的帮助

---

## 版权与授权

Copyright (C) 2026 XxHuberrr.  
Copyright (C) 2026 BA-Capple.

本项目采用 GPL-3.0 授权。详见 [LICENSE](./LICENSE)。

MR Logo、Mineradio 名称、界面视觉设计与原创视觉表达归原项目作者所有；第三方依赖和第三方服务分别遵循其各自授权与服务条款。

GD Studio API 遵循 CC BY-NC 4.0 协议，使用请注明出处。
