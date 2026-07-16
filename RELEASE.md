# 发布流程

## v1.1.666 发布边界

- 在前版本 `v1.1.0` 纯净安装版基础上增量发布。
- GD Studio 源歌词功能：在 server.js 新增 `/api/gd/lyric` 路由，前端 `fetchLyric` 新增 gdstudio 分支，支持多内部源自动兜底获取歌词。
- 修复 package.json JSON 语法错误。

## 发布前检查

- [x] `package.json` 版本号 `1.1.666` 正确。
- [x] `mineradio.update.owner/repo` 指向 `BA-Capple/Mineradio`。
- [x] `.cookie`、`.qq-cookie`、`updates/`、`node_modules/` 没有进入 git。
- [x] 语法检查：`node --check server.js` 通过。
- [x] Git 空白检查：`git diff --check` 未检测到空白错误。
- [x] 从当前源码执行 `npm run build:win` 生成 Windows 安装包。

## GitHub Release

Release tag：

```text
v1.1.666
```

Release 标题：

```text
Mineradio v1.1.666
```

建议上传资产：

- `dist/Mineradio-1.1.666-Setup.exe`
- `dist/Mineradio-1.1.666-Setup.exe.blockmap`
- `dist/latest.yml`
- `dist/Mineradio-1.1.666-SHA256SUMS.txt`

## 更新检测

应用通过 GitHub Releases latest 检测更新。本次 Release 设为公开 latest，旧客户端可通过软件内更新获取。
