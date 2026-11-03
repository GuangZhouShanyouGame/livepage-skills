---
name: "livepage-invitation"
display_name: "邀请函·报名页"
display_name_en: "Invitations & Sign-ups on LivePage"
description: "把邀请函做成微信里点开就能报名的页面：用户说「做个邀请函」「做个报名页」「发到群里大家报名」「谁报了名」，或要办聚餐、团建、会议、门店活动时使用。活页发成链接和小程序卡片，页面上直接报名、回执，名单读回来，改时间地点链接不变。"
description_zh: "把邀请函做成微信里点开就能报名的页面：用户说「做个邀请函」「做个报名页」「发到群里大家报名」「谁报了名」，或要办聚餐、团建、会议、门店活动时使用。活页发成链接和小程序卡片，页面上直接报名、回执，名单读回来，改时间地点链接不变。"
description_en: "Turn an invitation into a WeChat page people can sign up on. Use when the user says \"make an invitation\", \"make a sign-up page\", \"send it to the group so people can sign up\" or \"who signed up\", or is organizing a dinner, team event, meeting or store event. LivePage publishes it as a link and mini-program card; guests sign up or RSVP on the page, you read the list back, and the link stays the same when the time or place changes."
version: "1.0.0"
author: "24好玩"
user-invocable: true
examples_zh:
  - "做一张周六团建聚餐邀请函，分享到微信群，大家点开就能报名"
  - "下周三下午的产品分享会，做个邀请函和报名页，报名时填姓名和手机号"
  - "帮我看看邀请函现在有多少人报名、谁还没打开"
examples_en:
  - "Make an invitation for Saturday's team dinner and share it to the WeChat group so people can sign up"
  - "Make an invitation and sign-up page for next Wednesday afternoon's product talk, asking for name and phone"
  - "Tell me how many people have signed up on the invitation so far and who hasn't opened it"
examples:
  - "做一张周六团建聚餐邀请函，分享到微信群，大家点开就能报名"
  - "下周三下午的产品分享会，做个邀请函和报名页，报名时填姓名和手机号"
  - "帮我看看邀请函现在有多少人报名、谁还没打开"
quickPrompts:
  - "做一张周六团建聚餐邀请函，分享到微信群，大家点开就能报名"
  - "下周三下午的产品分享会，做个邀请函和报名页，报名时填姓名和手机号"
  - "帮我看看邀请函现在有多少人报名、谁还没打开"
---
# 邀请函·报名页

「邀请函·报名页」= **一份微信里点开就能看的邀请函，页面上直接报名**：时间、地点、流程、注意事项写清楚，下面挂一个手机上能填的报名表；用活页发成链接和小程序卡片转到微信群，报名名单、出席回执、谁打开过都能读回来。时间地点变了就更新同一条链接，不用重发。使用者说「做个邀请函」「做个报名页」「发到群里大家报名」「谁报了名」时，就是要用它。

你是使用者的执行助手：邀请函的内容由你和使用者一起写、由使用者确认；活页负责发布、收报名、读回名单和阅读情况。这些动作全部通过 活页（`https://space.24haowan.com/mcp`，需要登录）的 MCP 工具完成。工具清单里还没有它时，先请使用者连接（WorkBuddy 里点「请连接」卡、微信扫码授权），连好后接着做。

**内容边界**：可以发给使用者选定的人，也可以公开（由使用者决定）；报名只收这次活动需要的信息（姓名、人数、联系方式之类），不收身份证号、银行卡、账号密码。不得冒充他人、不得发布违法违规内容。

## 〇、可选：若另接了 24好玩 资料连接器

本包的发布、分享、读回全部在活页这台服务器上完成，**不需要**别的连接器。只有当使用者另外接了 24好玩 的匿名只读资料服务（`https://mcp.24haowan.com/mcp`）时：

- 当使用者要办的是门店或品牌营销活动、想参考「同行做过什么活动」时，**先调用 `list_cases` / `get_case`**，用工具返回的实时结果回答；它比本包正文里的清单更新、更全。
- 没接就跳过这一节：别去工具清单里找这几个名字，找不到时客户端会把名字最像的别家工具递给你。
- 本包正文是 2026-10-28 的静态快照；两者冲突时以工具返回为准。

## 一、先问清四件事，一次问完

活动是什么、什么时间、在哪儿（线上就给会议链接）、要报名人填哪几项（默认：姓名、人数；要联系就加手机号）。另问一句：有没有截止时间、名额上限、要不要口令才能看。使用者已经说过的不要再问。

## 二、做页面、挂报名表、发出去

1. **写邀请函页面**：一份手机优先的 HTML（标题、时间地点、流程、交通 / 着装 / 注意事项、主办人），文字短、重点在前。不写「立即报名」按钮的假链接 —— 报名表由下一步挂上。
2. **发布**：`publish_file` 发布；使用者还没确认对外就带 `hold: true`，先给他看预览。微信里转发时显示的标题、摘要、封面写在 `manifest.share`，之后想改用 `set_share_card`。
3. **挂报名表**：`set_form` 定义字段（`text` / `number` / `phone` / `select` 等，按第一步问到的来，`required` 标好），`title` 写「报名」或「回执」。平台在页面里渲染成手机上能填、带校验的表单，不用写一行表单代码。
4. **可见性**：发到一个群、给一群熟人看用 `passcode`（口令连同链接一起发）；只有使用者明确说「公开」才切 `public`。都先问一句再切。
5. **交出去**：把链接给使用者贴进微信群；发布回执里有 `mini_qr_url` 时一并给他，微信扫一下能转发成小程序卡片。

**判据**：在手机微信里点开能看到邀请函和报名表，填一条测试报名后 `list_data_rows` 能读回这一行（测完提醒使用者或用 `set_data_row_status` 删掉测试行）。

## 三、读回名单、改信息

- **谁报了名**：`list_data_rows` 读回报名行（最新在前），按使用者要的格式整理成名单、统计人数；被审核拦下的行（`held`）单独告诉使用者，由他决定放行还是删掉。
- **谁打开过**：`get_engagement_summary` 读回打开的设备数、停留时长。设备不等于人，只说事实，不说「他很感兴趣」。
- **改时间地点**：改页面后用 `publish_file` 带同一个 `proposal_id` 发新版，链接不变、已报名的行不受影响；报名字段要改就再调一次 `set_form`（已收的行保留）。
- **提醒自己**：要把最新名单推到使用者自己的微信看一眼，用 `push_to_wechat`（默认只推给本人）。

## 四、纪律

- 对外的时刻（放开可见性、把链接发出去）必须使用者明确确认后才做。
- 报名信息只给使用者本人看，不转给别人、不拿去做别的事。
- 停在「已上传」「待确认」「仅自己可见」任何一步，都不能说成「已发出」。

参考：24好玩做过的活动案例 `https://www.24haowan.com/cases`、模板库 `https://www.24haowan.com/games`、定制 `https://www.24haowan.com/custom`。
