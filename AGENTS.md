# Mineradio Project Rules

## Project Identity

Mineradio 是 Windows Electron 桌面音乐播放器，核心体验包括搜索、播放、歌单、歌词、3D 歌单架、粒子视觉预设、DIY 视觉控制台和 GitHub 自动更新。

- **Git 工作区（当前）**：`D:\WorkWithDS\Mineradio`
- **远程仓库**：`https://github.com/BA-Capple/Mineradio.git`（开发 fork）
- **上游仓库**：`https://github.com/XxHuberrr/Mineradio.git`
- **当前可运行程序**：`E:\桌面\播放器软件\Mineradio\Mineradio.exe`
- **运行版 app 目录**：`E:\桌面\播放器软件\Mineradio\resources\app`（无 .git，只影响运行）
- **当前版本**：`1.1.666`（see `package.json`）
- **统一备份区**：`E:\桌面\播放器软件\工作区备份`

## Start Every Session Here

新对话开始处理 Mineradio 前，先确认当前目录是 Git 工作区：

```powershell
D:\WorkWithDS\Mineradio
```

然后读这些文件：

- `AGENTS.md`
- `docs/PROJECT_MEMORY.md`
- 涉及玻璃 SVG 质感时读取 `docs/GLASS_SVG_TEXTURE.md`
- 涉及发布时读取 `CHANGELOG.md`、`RELEASE.md`、`package.json`

## Architecture

```
┌──────────────────────────────────────────────────────┐
│  Electron (desktop/main.js)                         │
│  • 主窗口加载 index.html（前台）                      │
│  • 桌面歌词窗口 → desktop-lyrics.html                │
│  • 壁纸模式 → wallpaper.html                         │
│  • 网易云/QQ 登录弹窗                                │
│  • 全局热键, 窗口状态, IPC 桥                         │
├──────────────────────────────────────────────────────┤
│  preload.js / overlay-preload.js                    │
│  → contextBridge 暴露 desktopWindow / desktopOverlay │
├──────────────────────────────────────────────────────┤
│  HTTP 服务 (server.js, 内嵌于 Electron)              │
│  • NeteaseCloudMusicApi 封装（搜索/歌单/歌词/登录）    │
│  • QQ 音乐接口 + cookie 持久化                       │
│  • Open-Meteo 天气电台                              │
│  • GitHub 更新检测 + 快速补丁 + 完整安装包下载        │
│  • 音频代理 / 封面代理                               │
│  • BPM / 节奏预分析 (dj-analyzer.js)                 │
├──────────────────────────────────────────────────────┤
│  前端 (public/index.html) — 135 万行单文件            │
│  • Global State — 音频/Beat/能量变量                  │
│  • Three.js 3D 场景（封面粒子、漂浮粒子、背景）        │
│  • 3D 歌单架（双模式: side / stage）                  │
│  • 歌词舞台 + SVG 玻璃质感                            │
│  • 视觉控制台（DIY 模式）                             │
│  • 音频上下文 / Web Audio API 分析                    │
│  • 离线节拍预解析                                    │
│  • API 助手 — fetch 包装                             │
│  • 所有 UI 渲染（CSS var 暗色主题, 无框架）           │
│  辅助 HTML 文件:                                     │
│  • desktop-lyrics.html — 独立歌词悬浮窗               │
│  • wallpaper.html — Canvas 2D 壁纸模式                │
└──────────────────────────────────────────────────────┘
```

### Key Architectural Facts

- **单文件前端**：所有 UI、CSS、Three.js、粒子、歌词、3D 歌单架、视觉控制台都在 `public/index.html` 一个文件里（约 1.35MB / 26000+ 行）。
- **无前端框架**：纯 vanilla JS，`<script>` 标签引入 Three.js r128 / GSAP / music-tempo（本地 vendor）。
- **Electron + HTTP 服务**：`desktop/main.js` 启动内嵌 `server.js` HTTP 服务，主窗口加载 `http://localhost:<port>/index.html`。
- **双 preload**：`preload.js` 给主窗口，`overlay-preload.js` 给歌词/壁纸窗口。
- **更新机制**：server.js 内置完整更新子系统 — GitHub Release 检测、块校验下载、快速补丁生成/应用。

## Repository Layout

```text
Mineradio/                          # Git 工作区 (D:\WorkWithDS\Mineradio)
├─ public/
│  ├─ index.html                    # 主 UI + 全部前端逻辑 (~26k 行)
│  ├─ desktop-lyrics.html           # 桌面歌词独立窗口
│  ├─ wallpaper.html                # 壁纸模式 Canvas 渲染
│  ├─ default-user-fx-archive.json  # 「默认测试」视觉预设存档
│  └─ vendor/                       # 本地依赖 (three.js, gsap, music-tempo)
├─ desktop/
│  ├─ main.js                       # Electron 主进程 (~1800 行)
│  ├─ preload.js                    # 主窗口 contextBridge
│  └─ overlay-preload.js            # 歌词/壁纸窗口 contextBridge
├─ build/
│  ├─ after-pack.js                 # rcedit 图标注入
│  └─ installer.nsh                 # NSIS 安装器定制
├─ docs/                            # 项目记忆、设计偏好、专项说明
├─ server.js                        # HTTP API + 音乐源 + 更新 + 代理
├─ dj-analyzer.js                   # 播客/DJ 流分析 (BPM/Intro/分段)
├─ package.json                     # 版本号、electron-builder 配置
└─ CHANGELOG.md                     # 中文更新说明 (顶部最新)
```

## Commands

```powershell
npm start                           # Electron 启动
node --check server.js               # server.js 语法检查
npm run build:win:dir                # 构建 unpacked 目录
npm run build:win                    # 构建 NSIS 安装包 (dist/)
npm install                          # 安装依赖 (含 electron-builder)
git diff --check                     # 提交前空白检查
```

没有独立 `npm test`。前端主逻辑在 `public/index.html`，运行版在 `E:\桌面\播放器软件\Mineradio\resources\app`，改完后重启 `Mineradio.exe` 检查效果。

注意：运行版 `resources\app\node_modules` 可能只包含运行依赖。发布打包缺少 `electron-builder` 时，先在 Git 工作区 `npm install`，再 `npm run build:win`。

## Release Workflow

发布新版本时：

1. 更新 `package.json` 和 `package-lock.json` 版本号。
2. 更新 `CHANGELOG.md` 顶部中文说明。
3. 运行语法/空白检查：`git diff --check`、`node --check server.js`。
4. 执行 `npm run build:win`。
5. 上传 GitHub Release 资产：
   - `dist/Mineradio-x.y.z-Setup.exe`
   - `dist/Mineradio-x.y.z-Setup.exe.blockmap`
   - `dist/latest.yml`
   - 需要的 `Mineradio-旧版本-x.y.z.json` 轻量补丁
6. 0.9 系列补丁跳过；1.0.x 系列可按需生成跨小版本补丁。
7. GitHub CLI 考虑请求用户打开代理TUN模式后执行

## User Preferences

- 交流语言：中文。
- 用户偏好：少废话，直接做，修完验证，能发布就一起发布。
- UI 审美：精致、暗色、高级、流畅，拒绝廉价渐变、过度透明、错位、闪烁和卡顿。
- 视觉质量定义：质感、丝滑度、帧数稳定同时成立；性能优化不能牺牲既有质感。
- 玻璃质感：当前播放器 SVG 玻璃质感是黄金版本，详见 `docs/GLASS_SVG_TEXTURE.md`。
- 备份策略：不要删除旧资料；重复和历史内容移动到 `E:\桌面\播放器软件\工作区备份`。
- 重要：不要再改旧外层源码目录。旧的 `E:\桌面\播放器软件\Mineradio\public` / `desktop` 已经归档；现在只有 `E:\桌面\播放器软件\Mineradio\resources\app\public` / `desktop` 会影响运行版。

## Memory Protocol

当用户说"保留""这个做得很好""我喜欢""记住这个""保存一下""以后别忘了"或同类表达时：

1. 判断用户认可的是代码、视觉效果、交互流程、发布流程还是工作习惯。
2. 将结论追加到 `docs/PROJECT_MEMORY.md` 的对应区块。
3. 如果是玻璃 SVG、粒子预设、3D 歌单架等脆弱视觉实现，同时更新对应专项文档。
4. 记录日期、涉及文件、关键参数、不要再改坏的边界。
5. 如果本轮有代码提交，把记忆文档一起提交；如果只是记忆整理，单独提交也可以。

## Guardrails

- 不要随意重写 `public/index.html` 的大块视觉系统；先定位已有函数和状态。
- 不要动电影视觉系统，除非用户明确点名。
- 不要恢复旧的侧边栏闪烁、控制台播放暂停失效、3D 歌单架强制切回星河等问题。
- 不要把搜索结果、左侧歌单、3D 歌单架的性能优化做成一次性渲染全部内容。
- 不要把用户认可的玻璃质感改成普通毛玻璃或廉价透明面板。
