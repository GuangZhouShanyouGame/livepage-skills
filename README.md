# LivePage · Agent Skills

[LivePage（活页）](https://space.24haowan.com) 是 agent 的发布按钮：把 AI 干完的活，变成一个可分享、会更新的网页，在微信里点开就能看。同一页可反复更新，链接不变。

本仓库收录 4 个 WorkBuddy 版技能，供支持 `skills/` 目录结构的 agent 客户端直接读取。

| 技能 | 做什么 |
| :-- | :-- |
| `livepage-publish` | 把 HTML、文档、PPT 一键发成微信里能打开的链接；读回谁看了、看到哪、反馈 |
| `livepage-app` | 做看板、报名页、投票、打卡墙；页面读同一份方案上的数据表，之后只改表不重发页面 |
| `livepage-invitation` | 把邀请函做成微信里点开就能报名的页面，读回名单 |
| `livepage-wechat-push` | 每天定时把行业动态、日报、复盘更新到同一链接并推送到微信 |

## 接入

活页是一台**需要登录**的 MCP 服务，端点只有一个：

```
https://space.24haowan.com/mcp
```

Streamable HTTP，不带任何 header 填进去，首次调用工具时客户端会打开浏览器，微信扫码登录、点「允许」即接入。

## 相关仓库

- [livepage-plugin](https://github.com/GuangZhouShanyouGame/livepage-plugin) — Claude / Codex / Cursor / Gemini 插件包（含 3 个场景技能与连接器配置）
- 技能全文网页版：`https://www.24haowan.com/open-skills/<技能名>`

## License

MIT
