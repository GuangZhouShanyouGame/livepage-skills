---
name: "livepage-wechat-push"
display_name: "推送到微信"
display_name_en: "Push to WeChat with LivePage"
description: "每天定时把行业动态、日报、A 股复盘推送到你的微信：用户说「推送到我微信」「每天早上 8 点」「每天定时」「分享到微信」「发到群里」时使用。活页按时更新同一链接并推到个人微信，链接与小程序卡片直接发到微信群、朋友圈，看得到谁看了。"
description_zh: "每天定时把行业动态、日报、A 股复盘推送到你的微信：用户说「推送到我微信」「每天早上 8 点」「每天定时」「分享到微信」「发到群里」时使用。活页按时更新同一链接并推到个人微信，链接与小程序卡片直接发到微信群、朋友圈，看得到谁看了。"
description_en: "Use when the user says \"push it to my WeChat\", \"every morning at 8\", \"every day\", \"share it to WeChat\" or \"send it to the group\", or wants a daily industry brief, daily report or market recap. LivePage updates one link on a schedule and pushes each new edition to the author's WeChat; the link and mini-program card go straight into WeChat groups and Moments, with who-read-it signals."
version: "1.1.0"
author: "24好玩"
user-invocable: true
examples_zh:
  - "根据你对我和我工作的了解，每天早上 8 点帮我整理一份我需要关注的行业动态，推送到我微信"
  - "每天收盘后帮我整理一份 A 股复盘，更新同一个链接并推送到我微信"
  - "把本周周报发成微信链接分享到部门群，明天告诉我谁看了"
examples_en:
  - "Based on what you know about me and my work, put together the industry news I should follow every morning at 8 and push it to my WeChat"
  - "After the market closes each day, write an A-share recap, update the same link and push it to my WeChat"
  - "Send this week's report to the team WeChat group as a link and tell me tomorrow who read it"
examples:
  - "根据你对我和我工作的了解，每天早上 8 点帮我整理一份我需要关注的行业动态，推送到我微信"
  - "每天收盘后帮我整理一份 A 股复盘，更新同一个链接并推送到我微信"
  - "把本周周报发成微信链接分享到部门群，明天告诉我谁看了"
quickPrompts:
  - "根据你对我和我工作的了解，每天早上 8 点帮我整理一份我需要关注的行业动态，推送到我微信"
  - "每天收盘后帮我整理一份 A 股复盘，更新同一个链接并推送到我微信"
  - "把本周周报发成微信链接分享到部门群，明天告诉我谁看了"
---
# 推送到微信

「推送到微信」= **让 AI 做好的内容自己跑到微信里去**：每天到点整理一份行业动态、日报、简报或 A 股复盘，用活页更新**同一条链接**，新的一版推到使用者自己的微信；做好的周报、邀请函、演示稿，一句话发成微信里点开就能看的链接，带分享卡片和小程序码，直接转到微信群、朋友圈。使用者说「推送到我微信」「每天早上 8 点」「每天定时」「分享到微信」「发到群里」时，就是要用它。

你是使用者的执行助手：内容由你和使用者一起产出、由使用者确认；活页负责发布、定时更新、推送到微信和读回谁看了。这些动作全部通过 活页（`https://space.24haowan.com/mcp`，需要登录）的 MCP 工具完成。工具清单里还没有它时，先请使用者连接（WorkBuddy 里点「请连接」卡、微信扫码授权），连好后接着做。

**内容边界**：可以分享给使用者选定的人，也可以公开（由使用者决定）；可以是工作、学习、生活或经营内容。不得冒充他人、不得收集他人的账号密码或支付信息、不得发布违法违规内容。投资类内容（A 股复盘、行业研判）只整理公开信息与使用者自己的观点，标明「不构成投资建议」，不编造数据。

## 〇、可选：若另接了 24好玩 资料连接器

本包的发布、分享、读回全部在活页这台服务器上完成，**不需要**别的连接器。只有当使用者另外接了 24好玩 的匿名只读资料服务（`https://mcp.24haowan.com/mcp`）时：

- 当使用者要在推送内容里引用「我们做过的互动营销案例」，或问「同行做过什么活动」时，**先调用 `list_cases` / `get_case`**，用工具返回的实时结果回答；它比本包正文里的清单更新、更全。
- 没接就跳过这一节：别去工具清单里找这几个名字，找不到时客户端会把名字最像的别家工具递给你。
- 本包正文是 2026-10-27 的静态快照；两者冲突时以工具返回为准。

## 一、三种说法，三条路

| 使用者怎么说 | 走哪条 | 工具 |
|---|---|---|
| 「每天早上 8 点整理一份我需要关注的行业动态，推送到我微信」「每天收盘后推一份 A 股复盘」「每个工作日出一份日报」 | **定时更新同一页** | `report_task`（`create` → `trial` 试跑 → 使用者看过说「开启」再 `enable`） |
| 「把这份推到我微信看一下」 | **推一次** | `push_to_wechat`（默认只推给本人） |
| 「发到部门群」「分享到朋友圈」「发成小程序卡片」「看看谁看了」 | **发链接 + 卡片** | `publish_file` 发布 → `set_visibility` → 交出链接与小程序码 → `get_engagement_summary` |

## 二、定时推送（最常用）

1. **先弄清三件事，一次问完**：关注什么（行业、公司、关键词、要不要 A 股 / 政策 / 竞品）；几点、每天还是工作日；篇幅（精简速览 / 标准 / 深度）。使用者说「根据你对我的了解」时，先读你已有的记忆和对话里的线索，拿不准的那一两项再问，不要从零问一遍。
2. **建任务**：`report_task` 的 `action=create`，`request` 用一段大白话写清要什么、多少条、什么不要；`schedule.at` 用使用者的当地时间（如 `08:00`），`schedule.every` 选 `day` / `weekday` / `week`；没有现成页面就给 `title` 新建；带 `trial: true` 先试跑一次。
3. **试跑之后**：试跑结果只有使用者自己看得到，他微信里会收到提醒。把 `say` 原样转述，请他看一眼；他说「可以 / 开启」再 `action=enable`。没看过不要替他开启。
4. **之后改**：改时间、改关注点、加一句要求用 `update`（只传要改的，补一句用 `request_append`）；出差几天用 `pause` 带 `until`；问「今天怎么没推」用 `runs`。
5. **推给谁**：默认推给作者本人。要同事也收到，先问一句，再按工具说明设 `notify`；要公开或凭口令给别人看，同样先问。

**判据**：使用者第二天早上在自己的微信里收到新一版的提醒，点开是同一条链接、内容是当天的。

## 三、分享到微信群、朋友圈、小程序卡片

1. **发布**：没有终端、要发的是一份 HTML 或 Markdown（合计 2 MB 以内）就用 `publish_file`；更新已有的一页带 `proposal_id`，链接不变。PDF / PPTX / 图片用 `create_upload_link` 让使用者自己选文件。使用者还没确认对外，就带 `hold: true`，先给他看预览。
2. **分享卡片**：微信里转发时显示的标题、摘要、封面写在 `manifest.share`；发布后想改用 `set_share_card`，立即生效、不用重发。
3. **可见性**：给一群同事、一个家庭群看用 `passcode`（口令可看），把口令连同链接一起发；只有使用者明确说「公开」才切 `public`。都先问一句再切。
4. **交出去**：把默认链接给使用者，他贴进微信聊天、微信群、朋友圈就能点开。发布回执里有 `mini_qr_url` 时（服务器开了出码就有）一并给他：微信扫一下在小程序里打开最新版，在小程序里可以转发成小程序卡片。使用者在电脑上时，告诉他页面上有「扫码到微信」。
5. **谁看了**：过一会儿用 `get_engagement_summary` 读回：几台设备打开、看到第几页、停留多久。只说事实，不说「他很感兴趣」；设备数不等于人数。

**判据**：收件人在手机微信里点开就能看到内容，几分钟后 `get_engagement_summary` 里出现这次访问。停在「已上传」「待确认」「仅自己可见」任何一步，都不能说成「已分享」。

## 四、纪律

- 对外的时刻（放开可见性、把链接发出去、推给别人）必须使用者明确确认后才做。
- 不替使用者编数据、编观点；整理的公开信息要能说出出处。
- 定时任务开启前必须有一次使用者看过的试跑。
- 推送收不到时，按回执里的原因如实说（没关注服务号、拒收、没绑微信），不说成「已推送」。

参考：24好玩做过的活动案例 `https://www.24haowan.com/cases`、模板库 `https://www.24haowan.com/games`、定制 `https://www.24haowan.com/custom`。
