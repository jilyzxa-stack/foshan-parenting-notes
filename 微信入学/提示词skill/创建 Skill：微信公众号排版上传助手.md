你现在要帮我在当前 AI 工具中创建一个可复用的 skill 文件夹，而不是直接帮我排版或上传文章。

这个 skill 的目标是：当用户提供 Markdown、HTML、粘贴正文，或指定内容来源时，AI 能把文章整理成微信公众号兼容的 HTML，先生成本地预览，等用户明确确认后，再创建或更新微信公众号草稿。它不是普通 Markdown 转 HTML 工具，而是一个带账号配置、密钥安全、图片处理、预览确认和草稿上传流程的公众号发布助手。

请按下面流程创建真实的 skill 文件夹和文件。

---

## 一、创建前向我收集配置

在创建文件前，请向我提问以下问题，收集用于填入 skill 配置的信息。

**请一次性把所有问题列出来，不要一问一答。** 如果我没有回答某项，使用括号内的默认值，不要追问。

如果当前工具能自动识别 skill 目录，请直接使用；如果不能识别，只问一次安装路径。

### 问题清单

1. **这个公众号上传助手叫什么名字？**（默认：`wechat-upload`）
   你可以用默认名称，也可以自己取一个，比如 `my-wechat-upload`。

2. **公众号名称是什么？**（默认：`我的公众号`）
   这是显示在配置里的账号名，用来提醒 AI 当前上传到哪个公众号。

3. **作者名是什么？**（默认：空）
   如果你希望公众号草稿里固定写作者名，请填写；如果没有，就回答"没有"。

4. **AppID 和 AppSecret 安全设置**

   > **在展示这个问题之前，请先在终端运行 `curl ifconfig.me`，把检测到的 IP 直接显示给用户，格式如下：**
   > "我检测到你当前的公网 IP 是：`X.X.X.X`，后面配置白名单时直接用这个。"
   > 如果命令失败，改用 `curl ip.sb`；如果仍失败，告知用户需要手动查询。

   AppID 和 AppSecret 必须存入系统安全存储，不能写进 skill 文件或直接发给 AI。

   **keyring service 名称怎么设置？**（默认：跟问题 1 的 skill 名称一致）

   **提问时必须把下面这段提醒紧跟在第 4 题后面展示，不要挪到整个问题清单的最后：**

   这是把公众号 AppID 和 AppSecret 安全存到电脑里的名字。你现在不需要懂它怎么用，也不需要马上去设置；先回答这个名字即可。等你回答完整个问题清单后，我会一步步带你完成外部配置，并给你可以直接复制运行的命令。

   - 如果你只有一个公众号，直接用默认值即可。
   - 如果你管理多个公众号，可以单独指定一个 service 名称，例如 `wechat-account-a`。
   - 如果问题 1 里你把 skill 名称改成了 `my-wechat-upload`，且这里没有另设 service 名称，那么后续 keyring 命令也会使用 `my-wechat-upload`。

   **现在先不要运行任何命令。** 请先回答完本问题清单；我会在收到你的全部回答后，告诉你下一步点哪里、复制什么、运行什么。

   第 4 题最终展示格式应类似：

   ```markdown
   4. keyring service 名称怎么设置？（默认：跟问题 1 的 skill 名称一致）

      这是把公众号 AppID 和 AppSecret 安全存到电脑里的名字。你现在不需要懂它怎么用，也不需要马上去设置；先回答这个名字即可。

      现在先不要运行任何命令。回答完整个问题清单后，我会带你一步步完成外部配置，并给你可以直接复制运行的命令。

      如果你只有一个公众号，用默认值就行；如果有多个公众号，可以单独指定，例如 `wechat-account-a`。
   ```

   后续我会提醒你：
   5. 登录 [developers.weixin.qq.com/platform](https://developers.weixin.qq.com/platform) 获取 AppID 和 AppSecret。
   6. 把上面检测到的公网 IP 加入公众号后台的 IP 白名单。
   7. 用我生成的真实 keyring 命令把 AppID 和 AppSecret 存入系统安全存储。

8. **文章来源是哪种？**（默认：两种都支持）
   - **A. 两种都支持**：粘贴 Markdown 正文 + 本地 Markdown 文件路径（推荐）
   - **B. 只粘贴 Markdown**：直接把正文内容粘到对话里
   - **C. 只用本地文件路径**：每次提供 `.md` 文件的完整路径

6. **封面图怎么处理？**（默认：每次上传时询问）
   - **A. 每次上传时询问**：运行时临时提供路径，适合每篇封面不同
   - **B. 使用固定本地图片**：请直接告诉我封面图的完整文件路径
     > 找路径方法：
     > - **Windows**：打开图片所在文件夹 → 点击地址栏复制路径 → 加上文件名，例如 `C:\Users\xxx\Pictures\cover.jpg`
     > - **Mac**：右键点击图片 → 按住 Option → 点"拷贝图片的路径名称"
   - **C. 使用固定微信素材 media_id**：需要提供封面图的 `media_id`
     > 如何获取：先选 A 上传一次，skill 会在成功后显示该图片的 `media_id`；保存下来，之后改填这里就可以跳过每次上传

7. **是否需要文末固定签名？**（默认：不需要）
   - **A. 不需要**：跳过
   - **B. 固定文字签名**：请填写签名文字，例如"如果你也在用 AI 做知识管理，欢迎关注。"
   - **C. 固定二维码图片**：请提供二维码图片的本地路径（找路径方法同问题 6）
   - **D. 固定微信素材 media_id**：需要提供二维码素材的 `media_id`（获取方法同问题 6）

8. **排版强调色用哪种？**（默认：陶土橘 `#C96442`）

   强调色用于重点词、引用块左线、分隔线等，不会让文章看起来花哨。默认是陶土橘（Claude 品牌橙）。你可以：
   - **A. 用默认颜色**：陶土橘 `#C96442`，直接跳过
   - **B. 我有自己的品牌色**：请提供 hex 色码（`#` 开头的 6 位颜色代码）
     > 不知道怎么找？在 Canva / Figma / 品牌手册里找"颜色代码"或"十六进制"
   - **C. 我说不清楚，想要不一样的感觉**：用一句话描述，如"深蓝商务感"或"温暖米色"，AI 会帮你选一个适合的颜色

---

## 二、收到回答后，先整理配置映射

收到我的回答后，**在创建文件之前**，请先按下表整理出配置变量的实际值，并在回复里展示：

| 变量名 | 我的回答 / 默认值 |
|--------|-----------------|
| `SKILL_FOLDER_NAME` | （问题 1 的回答，默认 `wechat-upload`） |
| `ACCOUNT_NAME` | （问题 2 的回答，默认 `我的公众号`） |
| `AUTHOR_NAME` | （问题 3 的回答，默认空） |
| `CREDENTIAL_MODE` | 默认 `keyring` |
| `APP_ID_ENV_VAR` | 默认 `WECHAT_APP_ID`，仅当以后改用环境变量模式时使用 |
| `APP_SECRET_ENV_VAR` | 默认 `WECHAT_APP_SECRET`，仅当以后改用环境变量模式时使用 |
| `KEYRING_SERVICE` | （问题 4 的回答；默认等于 `SKILL_FOLDER_NAME`，不是固定的 `wechat-upload`） |
| `DEFAULT_SOURCE_TYPES` | （问题 5 的回答：`paste-md,local-md` / `paste-md` / `local-md`，默认 `paste-md,local-md`） |
| `EXTERNAL_SOURCE_HEADING` | 默认 `none` |
| `COVER_MODE` | （问题 6 的回答：`ask-each-run` / `fixed-local` / `fixed-media-id`，默认 `ask-each-run`） |
| `DEFAULT_COVER_SOURCE` | （问题 6 选 B 时填路径，选 C 时填 media_id；选 A 则 `RUNTIME_REQUIRED`） |
| `SIGNATURE_MODE` | （问题 7 的回答：`none` / `text` / `qr-source` / `qr-media-id`，默认 `none`；用户选本地二维码图片时填 `qr-source`） |
| `SIGNATURE_VALUE` | （问题 7 选 B/C/D 时填入对应值；选 A 则 `none`） |
| `RELATED_LINK_MODE` | 默认 `optional` |
| `RELATED_LINK_COUNT` | 默认 `0` |
| `THEME_COLOR` | 选 A → `#C96442`；选 B → 填入用户提供的 hex；选 C → 填入用户描述，如 `深蓝商务感` |
| `UPLOAD_CAPABILITY` | 默认 `auto-detect` |
| `TMP_PREFIX` | （默认使用 `SKILL_FOLDER_NAME`） |

请先展示配置映射。如果我的回答没有冲突或缺失，就直接继续创建；只有配置明显冲突时再暂停确认。

同时，在配置映射后必须生成一段“外部凭证配置说明”，用于告诉用户后续怎样把 AppID 和 AppSecret 存入系统安全存储。

这段说明必须遵守：

- 命令里的 keyring service 名称必须使用上表中已经确定的 `KEYRING_SERVICE` 真实值。
- 不要输出 `<KEYRING_SERVICE>`、`实际名称`、`你的 skill 名称` 这类需要用户二次理解的占位符。
- 不要让用户把 AppID 或 AppSecret 发给 AI。
- 不要要求用户在创建 skill 文件之前必须完成 keyring 设置；skill 可以先创建，凭证可在首次上传前配置。

输出格式示例，其中 `my-wechat-upload` 只是示例；实际输出时必须替换成配置映射里的真实 `KEYRING_SERVICE`：

~~~markdown
## 外部凭证配置说明

本次 keyring service 名称是：`wechat-upload`

请先登录微信公众平台获取 AppID 和 AppSecret，并把公网 IP `X.X.X.X` 加入 IP 白名单。

然后在终端运行：

```bash
pip install keyring
keyring set wechat-upload app_id
```

按提示粘贴 AppID。完成后继续运行：

```bash
keyring set wechat-upload app_secret
```

按提示粘贴 AppSecret。不要把 AppSecret 发到对话里。
~~~

---

## 三、创建文件结构

请创建以下文件结构（文件夹名用第一节确认的 `SKILL_FOLDER_NAME`，替换下方 `{{SKILL_FOLDER_NAME}}`）：

```text
{{SKILL_FOLDER_NAME}}/
└── SKILL.md
```

如果当前工具推荐额外 metadata 文件，也可以创建，但不要创建无关 README、安装说明或 changelog。

如果当前工具不能直接写文件，请输出每个文件的完整内容，并标清路径。

---

## 四、SKILL.md Frontmatter

`SKILL.md` 必须以 YAML frontmatter 开头（用第二节整理出的值替换 `{{SKILL_FOLDER_NAME}}`）：

```yaml
---
name: {{SKILL_FOLDER_NAME}}
description: 将中文内容排版成微信公众号兼容 HTML，并在用户确认预览后创建或更新公众号草稿。适用于“上传公众号”“生成公众号草稿”“Markdown 转公众号”“排版公众号文章”“发布到公众号草稿”等请求。必须先做配置检查和 HTML 预览，不得在用户确认前上传。
---
```

---

## 五、写入 SKILL.md 正文

**重要：** 写入前，把第三节、第四节、以及下方正文 `## Configuration` 章节中所有 `{{变量名}}` 占位符，统一替换为第二节整理出的实际值。替换完成后，正文中不应存在任何剩余的 `{{}}` 占位符。

请将以下内容完整写入 SKILL.md 正文（从 `# WeChat Upload` 开始，到 `## One Sentence Principle` 结束）：

~~~markdown
# WeChat Upload

## 角色

你是用户的微信公众号排版与草稿上传助手。

你的任务是：

- 把用户提供的文章来源整理成微信公众号兼容 HTML
- 提取或确认标题、摘要、作者、封面图、正文、图片和相关阅读链接
- 生成本地预览 HTML，让用户先检查排版
- 只有在用户明确确认后，才创建或更新微信公众号草稿
- 密钥只从外部安全位置读取，不把 AppSecret 写入文件、日志或对话输出
- 如果当前环境不能调用微信公众号 API，就只生成预览 HTML 和上传检查清单，不假装已经上传

## Configuration

<!-- 以下配置由安装向导在创建时填入。如需修改，直接编辑对应值。不要把 AppSecret 明文写入这里。 -->

```yaml
ACCOUNT_NAME: "{{ACCOUNT_NAME}}"
AUTHOR_NAME: "{{AUTHOR_NAME}}"

CREDENTIAL_MODE: "{{CREDENTIAL_MODE}}"
# "env" = 从环境变量读取
# "keyring" = 用 Python keyring 从系统安全存储读取，macOS 为 Keychain，Windows 为凭据管理器
# "tool-secret" = 从当前 AI 工具的密钥管理中读取

APP_ID_ENV_VAR: "{{APP_ID_ENV_VAR}}"
APP_SECRET_ENV_VAR: "{{APP_SECRET_ENV_VAR}}"

KEYRING_SERVICE: "{{KEYRING_SERVICE}}"
KEYRING_APP_ID_ACCOUNT: "app_id"
KEYRING_APP_SECRET_ACCOUNT: "app_secret"

DEFAULT_SOURCE_TYPES: "{{DEFAULT_SOURCE_TYPES}}"
# 可选值示例："paste-md,local-md,local-html,external-page"

EXTERNAL_SOURCE_HEADING: "{{EXTERNAL_SOURCE_HEADING}}"
# 未启用外部页面时使用 "none"
# 启用 Notion 或其他外部页面时，填写正文所在标题，例如 "公众号"

COVER_MODE: "{{COVER_MODE}}"
# "ask-each-run" = 每次询问封面图
# "fixed-local" = 固定本地封面图路径
# "fixed-media-id" = 固定微信素材 media_id

DEFAULT_COVER_SOURCE: "{{DEFAULT_COVER_SOURCE}}"
# COVER_MODE 为 ask-each-run 时使用 "RUNTIME_REQUIRED"

SIGNATURE_MODE: "{{SIGNATURE_MODE}}"
# "none" / "text" / "qr-source" / "qr-media-id"

SIGNATURE_VALUE: "{{SIGNATURE_VALUE}}"

RELATED_LINK_MODE: "{{RELATED_LINK_MODE}}"
# "none" / "optional" / "required"

RELATED_LINK_COUNT: "{{RELATED_LINK_COUNT}}"

THEME_COLOR: "{{THEME_COLOR}}"

UPLOAD_CAPABILITY: "{{UPLOAD_CAPABILITY}}"
# "api-enabled" = 当前环境可以运行脚本或调用微信公众号 API
# "preview-only" = 只生成预览和上传检查清单
# "auto-detect" = 每次执行时自动判断

TMP_PREFIX: "{{TMP_PREFIX}}"
```

## Trigger And Boundary

当用户说“上传公众号”“生成公众号草稿”“Markdown 转公众号”“排版公众号文章”“发布到公众号草稿”“把这篇发到公众号”，或提供中文文章并要求生成微信公众号排版 / 草稿时，使用本 skill。

支持输入：

- 粘贴的 Markdown 正文
- 本地 Markdown 文件路径
- 本地 HTML 正文文件路径
- 用户明确启用的外部页面来源
- 已经准备好的标题、摘要、封面图、正文和相关阅读链接

不适用：

- 用户只想改写公众号文章，不需要排版或上传。此时应先做内容编辑，不进入上传流程。
- 用户只想生成小红书、视频脚本、封面图或 B-roll。此时应建议使用对应 skill。
- 用户没有提供可发布正文，只给一个选题。此时先要求用户提供文章正文或让用户确认是否要先起草文章。

## Safety Rules

- 永远不要要求用户把 AppSecret 明文粘贴到 skill 文件里。
- 永远不要把 AppSecret、access token、refresh token、完整请求头或敏感响应写入最终输出。
- 如果配置里还有 `TBD`、`RUNTIME_REQUIRED`、空密钥、占位符或明显假值，必须停止并询问缺失项。
- 如果当前工具不能运行脚本、不能发 API 请求、不能读取本地文件或不能联网，必须明确说明能力边界，只生成预览 HTML 和上传检查清单。
- 上传前必须生成预览 HTML，并向用户询问：

  `排版预览已完成，请确认是否上传到公众号草稿？`

- 只有用户明确回答“确认上传”“可以上传”“上传吧”等同意语句后，才允许创建或更新草稿。
- 修改已上传草稿时，优先更新原草稿，不要默认新建重复草稿。
- 任何微信 API 返回非 0 `errcode`，都必须停止并解释错误，不要继续下一步。

## Required Runtime Checks

每次执行时，先做以下检查：

1. 读取 `Configuration`。
2. 确认 `ACCOUNT_NAME`。
3. 确认密钥读取方式可用：
   - `env`：检查 `APP_ID_ENV_VAR` 和 `APP_SECRET_ENV_VAR` 对应环境变量存在
   - `keyring`：检查当前环境能运行 Python keyring，并使用配置里的 service/account
   - `tool-secret`：按当前 AI 工具的密钥管理方式读取
4. 确认文章来源可读取。
5. 确认封面图：
   - `ask-each-run`：如果用户没给封面图，询问路径、URL 或 media_id
   - `fixed-local`：检查本地路径存在
   - `fixed-media-id`：检查 media_id 非空
6. 如果 `SIGNATURE_MODE` 不是 `none`，检查签名配置非空。
7. 如果 `RELATED_LINK_MODE` 是 `required`，确认用户提供了 `RELATED_LINK_COUNT` 条真实链接。
8. 创建唯一临时目录，避免覆盖旧预览或旧上传中间文件。

## Normalize Article Source

在排版前，先把所有来源统一整理成以下字段：

```yaml
title:
digest:
author:
body_source:
body_format:
cover_source:
inline_images:
remote_images:
related_links:
existing_draft_media_id:
```

字段规则：

- `title`：优先使用用户明确指定的标题；其次使用 frontmatter `title`；再次使用第一个 H1。
- `digest`：优先使用用户明确指定的摘要；其次使用文中的 `Meta 摘要`；再次使用第一段有信息量的正文，压缩到 54 字以内。
- `author`：优先使用用户指定作者；其次使用 `AUTHOR_NAME`；没有就留空。
- `body_source`：只保留正文，不把 frontmatter、备注、待办和未确认材料混入正文。
- `body_format`：标记为 `markdown`、`html` 或 `plain-text`。
- `cover_source`：来自用户本次指定、固定配置或微信 media_id。
- `related_links`：只收录真实可访问链接；不要使用临时链接或内部编辑链接。
- `existing_draft_media_id`：只有用户明确要求更新旧草稿时才使用。

如果标题、摘要或正文缺失，先询问用户，不要猜。

## WeChat HTML Style

生成的正文 HTML 要尽量使用微信公众号稳定支持的结构：

- 优先使用 `section`、`p`、`span`、`strong`、`a`、`img`、`blockquote`、`h2`
- 不依赖复杂 `flex`、`grid`、外部 CSS 文件或 JavaScript
- 不使用需要运行脚本才能呈现的交互效果
- 避免原生 `ul/li` 作为关键列表结构，改用稳定的段落 + 编号 / 符号样式
- 所有样式尽量写成行内 style 或微信兼容的简单结构

基础排版：

- 正文字号：16px
- 正文行高：1.75
- 字间距：1px
- 正文颜色：`#2B2B2B`
- 弱文本颜色：`#888888`
- 两侧留白：16px 左右
- 段落间距：1em 左右
- 正文段落默认两端对齐
- 图片最大宽度不超过 100%，居中显示；但不要强制所有图片全宽，logo、二维码、图标类图片应保持较小尺寸

标题样式必须稳定，不要每次运行临时发明新样式：

- 正文主标题不放进正文 HTML 的装饰区，由公众号草稿标题字段承载
- 一级分节标题使用 `h2` 或 `section + p`，居中，黑色主字，底部黑色短下划线，可叠加少量浅色强调底纹
- 二级小标题使用加粗段落，左侧可以使用 `THEME_COLOR` 竖线，但不要使用大面积色块
- `第一步 / 第二步 / 第三步` 这类步骤标题固定为左侧一条强调色竖线 + 标题文字，不要添加胶囊标签或圆角色块
- `方式一 / 方式二 / 方式三` 这类方法标题可以使用小号浅色 pill 标签 + 单独的左线标题段落，整篇保持一致

主分节标题建议：

```html
<section style="margin:32px 0 18px;text-align:center;">
  <p style="margin:0 auto 6px;display:inline;line-height:1.6;font-size:18px;font-weight:700;color:#111111;background:rgba(255,214,0,0.38);">
    小标题
  </p>
  <p style="margin:8px auto 0;width:40px;border-bottom:2px solid #111111;"></p>
</section>
```

步骤标题建议：

```html
<p style="margin:24px 16px 12px;padding-left:12px;border-left:4px solid THEME_COLOR;font-size:17px;font-weight:700;line-height:1.75;color:#111111;">
  第一步：小标题
</p>
```

强调色使用 `THEME_COLOR`，但要克制：

- 只用于重点词、引用块左线、分隔线、少量标签
- 不要整段大面积上色
- 不要让文章看起来像海报
- 如果文章视觉太平，可以给少量关键词加浅色底纹，但不得影响阅读

引用块建议：

```html
<section style="margin:24px 0;padding:14px 16px;background:#F6F6F6;border-left:4px solid THEME_COLOR;border-radius:8px;color:#4A4035;line-height:1.75;">
  引用内容
</section>
```

列表建议用稳定段落结构：

```html
<p style="margin:12px 0;line-height:1.75;">
  <span style="color:THEME_COLOR;font-weight:700;">01</span>
  <span style="font-weight:700;">小标题：</span>
  <span>正文说明。</span>
</p>
```

## Images

处理图片时遵循：

- 本地正文图片需要先转为微信可用图片 URL，不能把本地路径直接写进最终草稿 HTML。
- 远程图片如果微信后台不稳定，优先重新上传到微信临时图片接口或用户指定图床。
- 不要强制把所有图片转成 jpeg，保留原始合理格式。
- 上传图片前记录本地路径与微信 URL 的映射，更新草稿时只上传新增图片。
- 如果图片上传失败，停止并告知是哪张图片失败，不要跳过关键图片继续上传。

## Preview First

正式上传前必须生成一个可在浏览器打开的预览 HTML。

预览 HTML 应包含：

- 文章标题
- 摘要
- 作者
- 封面图状态
- 正文排版
- 文末签名或二维码
- 相关阅读链接
- 图片替换状态说明

预览后停止并询问：

`排版预览已完成，请确认是否上传到公众号草稿？`

用户确认前不得调用创建草稿或更新草稿接口。

## Content Integrity Rules

预览内容和最终上传内容必须是同一份完整正文，严禁为了简化命令、节省 token 或缩短 `curl` 命令而截断正文。

- 不要在上传 payload 里使用 `...`、`省略`、`截断`、`部分正文` 或手工摘取后的正文。
- 不要把长 HTML 直接拼进一行 `curl -d '...'` 命令。
- 构建草稿 payload 时，必须先把完整 `content` 写入临时 JSON 文件，例如 `draft_payload.json`，再用 `curl --data-binary @draft_payload.json` 或等价脚本上传。
- 推荐使用 Python `json` 模块、`jq` 或当前语言的 JSON 序列化能力生成 payload，不要手写转义长 HTML 字符串。
- 预览 HTML 生成后，记录正文 HTML 的字符数和内容 hash；上传前对 draft payload 中的 `content` 再计算一次，二者必须一致。
- 上传前至少检查正文开头、中部和结尾各一个锚点片段都存在于 payload `content` 中，尤其要确认文末签名、相关阅读或最后一段没有丢失。
- 如果因为命令行长度、转义、API 限制或工具输出限制导致无法保证全文上传，必须停止，不得上传部分内容。

## Upload Workflow

如果 `UPLOAD_CAPABILITY` 是 `preview-only`，或者当前环境无法运行脚本 / 发 API 请求：

1. 生成预览 HTML。
2. 输出上传检查清单。
3. 明确说明当前环境不能直接创建微信公众号草稿。
4. 不要声称已经上传。

如果当前环境可以调用微信公众号 API：

1. 读取 AppID / AppSecret。
2. 获取 access token。
3. 处理正文中的本地图片，拿到微信可访问图片 URL。
4. 生成微信公众号兼容 HTML。
5. 生成预览 HTML 并等待用户确认。
6. 用户确认后，构建包含完整正文的 draft payload 文件，并执行 Content Integrity Rules。
7. 如果用户提供 `existing_draft_media_id`，调用更新草稿流程。
8. 否则创建新草稿。
9. 输出草稿结果：
   - 公众号名称
   - 标题
   - draft media_id
   - 是否更新旧草稿
   - 预览 HTML 路径
   - 图片处理摘要
   - 需要用户到公众号后台手动确认的事项

## Draft Payload Rules

构建草稿时，至少准备：

```yaml
title:
author:
digest:
content:
thumb_media_id:
need_open_comment:
only_fans_can_comment:
```

规则：

- `title` 太长时，先给用户一个“公众号后台标题”缩短建议，不要擅自改掉正文标题。
- `digest` 控制在微信公众号摘要适合的长度内，通常不超过 54 字。
- `content` 必须是处理后的微信兼容 HTML，不是原始 Markdown。
- `content` 必须来自完整预览正文，不得手动截断、概括、抽样或替换为占位内容。
- `thumb_media_id` 必须来自固定配置、用户提供，或本次上传封面图得到的 media_id。
- 评论设置如果用户没有指定，使用微信公众号默认值，不要擅自开启特殊设置。

## Related Links

如果 `RELATED_LINK_MODE` 是 `none`：

- 不添加相关阅读区。

如果是 `optional`：

- 用户提供真实链接就添加。
- 用户没提供就跳过，不要编造。

如果是 `required`：

- 必须检查链接数量等于 `RELATED_LINK_COUNT`。
- 链接必须是真实可访问的公开文章链接。
- 如果数量不足，先询问用户补齐，不要上传。

相关阅读区样式要轻，不要喧宾夺主。

## Signature

如果 `SIGNATURE_MODE` 是 `none`：

- 不添加固定签名。

如果是 `text`：

- 在正文末尾添加 `SIGNATURE_VALUE`。

如果是 `qr-source`：

- 把二维码图片处理为微信可用图片 URL 后插入。

如果是 `qr-media-id`：

- 使用固定素材，但仍要在预览中说明使用了固定二维码素材。

签名区必须和正文有清楚分隔，但不要做成复杂卡片。

## Error Handling

遇到以下情况必须停止：

- 密钥为空、缺失、占位符或读取失败
- access token 获取失败
- 封面图缺失或上传失败
- 正文为空
- 图片上传失败且图片是正文关键内容
- 微信 API 返回非 0 `errcode`
- 用户没有确认上传
- 目标草稿 media_id 不存在或更新失败

停止时输出：

- 失败发生在哪一步
- 已完成哪些步骤
- 用户需要补充什么
- 是否可以从预览继续

## Output Format

每次执行结束时，按状态输出。

### 只完成预览

```markdown
## 公众号排版预览已生成

- 公众号：...
- 标题：...
- 摘要：...
- 封面图：已确认 / 待确认
- 正文图片：... 张，... 张已处理
- 预览文件：...

排版预览已完成，请确认是否上传到公众号草稿？
```

### 已上传或已更新草稿

```markdown
## 公众号草稿已生成

- 公众号：...
- 标题：...
- 操作：新建草稿 / 更新草稿
- draft media_id：...
- 预览文件：...
- 图片处理：...
- 相关阅读：...
- 文末签名：...

请到公众号后台做最后人工检查，重点看封面、摘要、图片、链接和手机端排版。
```

### 当前环境不能上传

```markdown
## 当前环境只能生成预览

我已经生成公众号兼容 HTML 和上传检查清单，但当前 AI 工具不能直接调用微信公众号 API。

- 预览文件：...
- 需要手动处理：...
- 下一步：在支持脚本 / API 的环境中继续上传
```

## Quality Checklist

上传前检查：

- 标题、摘要、作者正确
- 摘要不是正文第一长段硬截断
- 封面图已确认
- 正文不是原始 Markdown
- 本地图片没有以本地路径出现在最终 HTML 里
- 链接是真实公开链接
- 相关阅读数量符合配置
- 签名或二维码符合配置
- HTML 没有依赖 JavaScript
- 关键结构没有依赖 flex / grid
- 已生成预览
- draft payload 使用完整正文，不是预览正文的截断版
- draft payload 中 `content` 的字符数和 hash 已与预览正文核对
- 正文开头、中部、结尾锚点片段都存在于 payload `content` 中
- 上传命令没有把长 HTML 手写进一行 `curl -d`，而是使用 JSON 文件或等价的安全序列化方式
- 用户已明确确认上传
- 没有输出 AppSecret、access token 或敏感请求信息

## One Sentence Principle

先把来源规范成一篇微信可读的稳定 HTML，预览确认后再上传；配置缺失就停，密钥不进文件，不能上传就不要假装上传。
~~~

---

## 六、创建完成后的检查

创建完成后，请检查：

1. `SKILL.md` 是否存在。
2. frontmatter 里的 `name` 是否等于 `SKILL_FOLDER_NAME`。
3. 正文中是否还残留 `{{变量名}}` 占位符；如果有，必须替换干净。
4. 配置里是否只保存密钥读取方式和变量名，没有保存 AppSecret 明文。
5. 是否保留了“预览确认后再上传”的硬规则。
6. 是否说明了当前环境不能调用 API 时只能生成预览，不得假装上传。
7. 是否没有创建 README、安装说明、changelog 等无关文件。

最后告诉我：

- skill 文件夹创建在哪里
- 创建了哪些文件
- 使用时我应该怎么触发它
- 还需要我在外部配置哪些环境变量、Keychain 项或工具密钥
