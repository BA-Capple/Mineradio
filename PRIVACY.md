# 隐私与用户数据说明

Mineradio 是本地桌面应用。项目不应把用户登录状态、Cookie、播放历史、搜索历史、自定义封面、自定义歌词或本地缓存提交到 GitHub。

## 本地数据

应用可能在本机保存以下数据：

- 网易云音乐登录 Cookie
- QQ 音乐登录 Cookie
- 搜索历史
- 自定义专辑封面
- 自定义歌词
- 歌词布局与视觉控制设置
- 本地节奏分析缓存
- 更新安装包下载缓存

这些数据用于本地体验，不属于开源仓库内容。

## 不应上传的内容

以下内容不应提交到 GitHub：

- `.cookie`
- `.qq-cookie`
- `updates/`
- `node_modules/`
- Electron 打包产物
- 用户上传的本地音乐文件
- 用户账号信息、Cookie、Token、二维码登录状态

## 第三方平台

用户通过网易云音乐、QQ 音乐等第三方平台登录时，应遵守对应平台的用户协议。Mineradio 不提供绕过付费、绕过会员、破解音质或重新分发音乐内容的能力。

## GD Studio API

Mineradio 集成了 [GD Studio 在线音乐平台 API](https://music-api.gdstudio.xyz) 作为后备音源。

- 搜索歌曲时，歌名、歌手等关键词会发送到 GD Studio API 服务器
- GD Studio API 本身不要求登录，不存储用户个人信息
- API 返回的音乐链接来自第三方 CDN，Mineradio 仅作代理转发，不存储或分发音乐文件
- GD Studio API 的隐私政策请参考其官方网站
- API 提供方联系方式：[gdstudio@email.com](mailto:gdstudio@email.com)
