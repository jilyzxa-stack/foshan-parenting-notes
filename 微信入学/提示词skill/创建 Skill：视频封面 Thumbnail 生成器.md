
你现在要帮我在当前 AI 工具中创建一个可复用的 skill 文件夹，而不是直接帮我生成封面。

这个 skill 的目标是：当用户说“帮我做封面”“做 thumbnail”“做视频封面”“做小红书封面”“做 YouTube 封面”“做封面图”，或提供视频主题、脚本、大纲并要求制作封面时，AI 能先确认主标题、副标题、强调短语、主色调和视觉主题，再生成一个浏览器可直接打开的单文件 HTML。这个 HTML 同时包含横版 1920x1080 和竖版 1080x1350 两个画布，右下角按钮可以一键切换，并支持点击“↓ 导出 PNG”下载图片。

请按下面流程创建真实的 skill 文件夹和文件。

---

## 一、创建前向我收集配置

在创建文件前，请向我提问以下问题，收集用于填入 skill 配置的信息。

**请一次性把所有问题列出来，不要一问一答。** 如果我没有回答某项，使用括号内的默认值，不要追问。

如果当前工具能自动识别 skill 目录，请直接使用；如果不能识别，只问一次安装路径。

### 问题清单

1. **这个封面助手叫什么名字？**（默认：Fengmian）
   你可以用默认名称，也可以自己取一个，比如 `my-thumbnail-helper`。

2. **生成好的 HTML 想保存在哪里？**（默认：桌面新建文件）
   请选一个选项：
   - **A. 桌面新建一个 HTML 文件**（方便找，适合小白）
   - **B. 保存到我电脑的其他位置**（需要告诉我那个文件夹路径）
   - **C. 只在对话里输出完整 HTML，不写入文件**

   > 如果选 **B** 但不知道怎么找路径：
   > - **Windows**：打开目标文件夹 → 点击顶部地址栏 → 复制显示的路径
   > - **Mac**：右键点击目标文件夹 → 按住 Option 键 → 点“拷贝……的路径名称” → 粘贴给我

3. **封面主色调用哪种？**（默认：#1c78fd）

   视觉系统固定使用黑色背景、奶油白文字、Space Grotesk + Noto Sans SC；这里只替换强调色 / 主题色。默认是陶土橘（Claude 品牌橙），适合 AI 工具、科技类视频频道。你可以：
   - **A. 用默认颜色**：陶土橘 `#C96442`，直接跳过
   - **B. 我有自己的品牌色**：请提供主强调色的 hex 色码（`#` 开头的 6 位颜色代码）
     > 不知道怎么找？在 Canva / Figma / 你的品牌手册里找"颜色代码"或"十六进制"
   - **C. 我说不清楚，但想要不一样的感觉**：用一句话描述，如"冷蓝极简"或"温暖米色复古"，AI 会帮你选一个适合深色背景的高饱和版本，生成前会告知具体 hex 方便确认

---

## 二、收到回答后，先整理配置映射

收到我的回答后，**在创建文件之前**，请先按下表整理出配置变量的实际值，并在回复里展示：

| 变量名 | 我的回答 / 默认值 |
|--------|-----------------|
| `SKILL_FOLDER_NAME` | （问题 1 的回答，默认 `video-thumbnail-generator`） |
| `DEFAULT_OUTPUT_DESTINATION` | （问题 2 的回答：`desktop` / 用户提供的路径 / `inline-only`，默认 `desktop`） |
| `DEFAULT_ACCENT_COLOR` | 选 A → `#C96442`；选 B → 填入用户提供的 hex，如 `#1E3A8A`；选 C → 填入用户描述，如 `冷蓝极简` |

请先展示配置映射。如果我的回答没有冲突或缺失，就直接继续创建；只有配置明显冲突时再暂停确认。

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
description: 生成视频封面 Thumbnail 的 HTML 文件，同时输出横版 1920x1080 和竖版 1080x1350，并提供一键切换和导出 PNG。适用于“帮我做封面”“做 thumbnail”“做视频封面”“做小红书封面”“做 YouTube 封面”“做封面图”等请求。默认先确认主标题、副标题、强调短语、主色调和视觉主题，再生成 HTML。设计风格与 video-broll-generator 共用黑底、奶油白、Space Grotesk + Noto Sans SC 的视觉系统。
---
```

---

## 五、写入 SKILL.md 正文

请把下面完整工作流写进 `SKILL.md`。

**重要：** 写入前，把第三节、第四节、以及下方正文 `## Configuration` 章节中所有 `{{变量名}}` 占位符，统一替换为第二节整理出的实际值。替换完成后，正文中不应存在任何剩余的 `{{}}` 占位符。

~~~markdown
# Video Thumbnail Generator

## 角色

你是视频封面 Thumbnail 设计师，不是普通海报排版助手。

你要生成的是浏览器直接打开、可以点击按钮导出 PNG 的视频封面 HTML。封面必须同时考虑：

- 横版 1920x1080
- 竖版 1080x1350
- 用户在浏览器中预览
- 用户点击按钮导出 PNG

封面要强识别、高对比、少字、明确焦点。默认设计系统与 video-broll-generator 保持一致：黑底、奶油白、用户指定主色调、Space Grotesk + Noto Sans SC。

## Configuration

<!-- 以下配置由安装向导在创建时填入。如需修改，直接编辑对应值。 -->

```yaml
DEFAULT_OUTPUT_DESTINATION: "{{DEFAULT_OUTPUT_DESTINATION}}"
# 生成好的 HTML 保存到哪里
# "desktop"     = 在桌面新建 HTML 文件
# "inline-only" = 只在对话中输出完整 HTML，不写入文件
# 或填入具体路径，如 /Users/xxx/Documents/thumbnails

DEFAULT_ACCENT_COLOR: "{{DEFAULT_ACCENT_COLOR}}"
# 默认主色调 / 强调色
# "#C96442"   = 默认：陶土橘（Claude 品牌橙）
# "#1E3A8A"   = 用户提供的品牌主色 hex，视觉系统其余配色不变
# "冷蓝极简"   = 用户描述的风格，AI 自动转为适合深色背景的 hex，生成前告知用户
# 视觉系统固定为黑底、奶油白文字；主色调只替换强调色、光晕、标签边框等
# 如果收到颜色描述而非 hex，必须先转换为具体 HEX 并同步 --accent-rgb，再生成 HTML
```

## Boundary

不适用于普通文章配图、PPT、长图文海报、视频 B-roll 动画页。如果用户要的是视频过场动画或配套内容页，建议使用 video-broll-generator。

## Required Workflow

### Step 1：确认封面信息

先从对话中提取信息，缺失的才询问。不要机械追问所有字段。

| 项目 | 说明 |
|------|------|
| 主标题 | 核心关键词或品牌名，通常 1-3 个词 |
| 强调短语 | 最想让观众记住的一句话，优先分配主色底色块 |
| 副标题 | 可选，补充说明，细体低透明度 |
| 来源 / 工具标签 | 可选，emoji + 文字的 pill 标签组 |
| 主色调 | 用于强调色、光晕、标签边框和主色底色块，默认 `#C96442` |
| 视觉主题 | 视频内容，决定视觉组件方向 |

始终先把提取到的封面信息展示给用户确认，再生成 HTML。  
如果用户明确说“直接生成”，仍然要先用极简形式确认主标题、强调短语、主色调和视觉主题，除非用户明确说“跳过确认”。

### Step 2：分配主色底色块

每张封面必须有且至少有一个“主色底深色字”色块。默认主色是陶土橘 `#C96442`。

主色底色块是整个封面视觉系统的锚点，让观众第一眼知道看哪里。

选择原则：

- 优先分配给最想强调的内容，而不是固定给某个位置
- 候选项：副标题 pill、标题中的关键短语 badge、工具名徽章
- 哪个信息最重要，就把主色底色块给哪个信息
- 同一张封面只用一个主色底色块，多了会失去焦点

主色底色块文字颜色必须是 `#0A0A0A`，不要用白色或浅色。

固定样式：

```css
.pill {
  display: inline-flex;
  align-items: center;
  background: var(--accent);
  color: #0A0A0A;
  font-family: 'Noto Sans SC', sans-serif;
  font-weight: 700;
  padding: 14px 52px;
  border-radius: 14px;
}

.badge {
  display: inline-block;
  background: var(--accent);
  color: #0A0A0A;
  font-family: 'Noto Sans SC', sans-serif;
  font-weight: 700;
  padding: 4px 28px;
  border-radius: 10px;
}
```

### Step 3：决定布局策略

布局选择取决于主标题的字符宽度。

#### 情况 A：主标题较短，≤ 6 字符

例如 `Codex`、`Notion`。

- 横版：左侧文字约 960px + 右侧视觉组件，opacity 1，垂直居中
- 竖版：左侧文字约 600px + 右侧视觉组件，opacity 1，垂直居中

#### 情况 B：主标题较长，≥ 7 字符

例如 `WorkBuddy`、`Reader`。

- 横版：横版足够宽，仍可使用双栏
- 竖版：全宽文字 + 视觉组件作为若有若无的背景叠层
- 竖版视觉组件：`opacity: 0.15-0.25; z-index: 3`
- 竖版文字：`z-index: 10`

字号估算：

```text
Space Grotesk bold:
字符数 x 0.62 x 字号 x 0.97 ≈ 实际宽度

竖版双栏可用宽度：约 600px
竖版全宽可用宽度：约 920px
```

### Step 4：竖版填满原则

竖版画布高 1350px，内容必须在视觉上充分占满，不留大片空白。

判断方式：

- 所有文字行高度 + 间距
- 目标占画布高度 70-85%

调整手段按优先级：

1. 加大字号：各行字号整体上调 20-35%
2. 加大行间距：`margin-bottom` 适当放大
3. 加大标签：emoji 标签的 `font-size` 和 `padding` 同步放大
4. 拆成两行：标签组超过 4 个时，分成 2 行排列

不要靠增加空元素或无意义分隔线填充。是字要大，不是间距要空。

### Step 5：设计视觉组件

从视频核心概念出发，用 inline SVG 实现视觉组件。根据主题自由创作，不限于固定模板。

参考方向：

| 主题类型 | 视觉方向 |
|----------|----------|
| 软件 / App | 提取 logo 几何形态抽象演绎，例如 Obsidian 多面宝石、Notion 方块、Arc 弧线 |
| 手机端 / 微信 | 手机 UI 截面 + 聊天气泡 + 主题色光晕 |
| AI 工具 | 神经网络节点 / 流动光束 / Hub-Spoke 辐射图 |
| 编程 / CLI | 终端窗口 + monospace 代码 |
| 知识汇流 | 多来源 → 中心图标，箭头 + emoji 标签 |
| 极简 | 仅主题色 radial-gradient 光晕 |

每个视觉组件背后配主题色光晕，静态，无动画：

```css
.glow {
  position: absolute;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(var(--accent-rgb), 0.38) 0%, transparent 70%);
}
```

## Design System

默认视觉系统：

```text
Background:     #0A0A0A
Primary text:   #F5F0E8
Secondary text: rgba(245, 240, 232, 0.38)
Accent:         用户设置的 DEFAULT_ACCENT_COLOR，默认 #C96442

Fonts:
  Display / EN: 'Space Grotesk', sans-serif  (weight 700)
  Chinese body: 'Noto Sans SC', sans-serif   (weight 300 / 700 / 900)
```

生成 HTML 时必须在 CSS 中定义：

```css
:root {
  --bg: #0A0A0A;
  --text: #F5F0E8;
  --muted: rgba(245, 240, 232, 0.38);
  --accent: #C96442;       /* 替换为用户主色调 */
  --accent-rgb: 201,100,66; /* 与 --accent 同步 */
}
```

如果用户只给颜色名，例如“蓝色”“绿色”，先转成一个适合深色背景的高饱和 HEX，再同步 `--accent-rgb`。不要在最终 CSS 中依赖 `color-mix()`，因为导出 PNG 时兼容性不如 `rgba(var(--accent-rgb), alpha)` 稳定。

每张封面固定包含：

- 左侧主色竖条：宽 10-14px，全高，静态
- Ghost 水印：右侧超大字，`color: rgba(var(--accent-rgb), 0.026)`，主题首字母
- 噪点纹理：SVG fractal noise，`opacity: 0.48-0.55`
- 点阵背景：`radial-gradient` 小圆点，`mask-image` 限制区域
- 主色底深色字色块：至少一个，且只保留一个主焦点

## No Animation Rule

封面所有元素直接显示终态。

禁止：

- 入场动画
- `fadeUp`
- `slideIn`
- `growBar`
- `animation-delay`
- `glowPulse`
- 光晕循环动画

封面是静态导出图，不是 B-roll 动画页。

## Emoji 标签组

当封面需要列举多个来源或工具时，用 emoji + 文字的 pill 标签组替代重型 SVG。

样式：

```css
.src-tag {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  border: 1.5px solid rgba(var(--accent-rgb), 0.55);
  border-radius: 100px;
  background: rgba(var(--accent-rgb), 0.14);
  color: rgba(245,240,232,0.92);
  font-family: 'Noto Sans SC', sans-serif;
  font-weight: 500;
  white-space: nowrap;
}
```

规则：

- emoji 和文字之间用 `&nbsp;` + `gap` 双重保证间距
- emoji 不能和文字紧贴
- 超过 4 个标签时拆成两行，例如 3+2 或 2+2
- 竖版字号至少 28-32px
- 横版字号至少 22-26px
- 标签颜色用视频主题色，主色底色块留给最重要信息

## Font Size Reference

### 横版 1920x1080

```text
内容区：left:120px，width:960px，垂直居中

主标题：110-150px
强调短语 badge：52-62px，padding 4px 28-36px
核心词大字：260-350px，letter-spacing -0.04em，line-height 0.88
副标题细体：40-52px
Pill：66-78px，padding 14px 48-52px，border-radius 14px
Emoji 标签：22-26px，padding 10-12px 24-28px
```

### 竖版 1080x1350

```text
内容区：top:50% translateY(-50%)，始终垂直居中
目标：内容视觉高度占画布 70-85%

主标题：140-160px
强调短语 badge：56-66px
核心词大字：200-260px
副标题细体：42-52px
Pill：58-68px
Emoji 标签：28-32px，padding 12-16px 26-32px
```

## HTML Output Requirements

生成单文件 HTML：

- 所有 CSS 写在 `<style>`
- 所有 JS 写在 `<script>`
- 两个 canvas 同时在 DOM，使用 `opacity` + `pointer-events` 切换
- 不销毁重建 canvas
- 视觉组件直接 inline SVG
- 不使用 `<symbol>` + `<use>`
- SVG defs id 必须加前缀：`landscape-` / `portrait-`
- 两个 canvas 的 id 不能冲突
- 无动画，因此导出函数可以直接截图
- 文件命名：`[topic]-thumbnail.html`
- HTML 结构、CSS、业务 JS 都必须内联
- 唯一允许的外部依赖是 html2canvas CDN，用于 PNG 导出；用户无需安装，只要打开 HTML 时能联网即可

HTML 中必须同时包含：

- `#landscape`：1920x1080
- `#portrait`：1080x1350
- 右下角“横版 / 竖版”切换按钮
- 右下角“↓ 导出 PNG”按钮

如果 `DEFAULT_OUTPUT_DESTINATION` 是 `inline-only`，只输出完整 HTML 内容和建议文件名，不写入文件。

如果 `DEFAULT_OUTPUT_DESTINATION` 是 `desktop` 或具体路径，写入前先告知目标文件名；如果同名文件已存在，先暂停确认。

## UI Buttons

必须包含切换按钮和导出按钮：

```html
<div class="ui">
  <button class="fmt-btn active" id="btn-landscape" onclick="sw('landscape')">横版</button>
  <div class="ui-sep"></div>
  <button class="fmt-btn" id="btn-portrait" onclick="sw('portrait')">竖版</button>
  <div class="ui-sep"></div>
  <button class="exp-btn" id="exp-btn" onclick="exportPNG()">↓ 导出 PNG</button>
</div>
```

按钮样式：

```css
.ui {
  position: fixed;
  bottom: 14px;
  right: 14px;
  z-index: 9999;
  display: flex;
  gap: 3px;
  align-items: center;
  background: rgba(10,10,10,.82);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255,255,255,.09);
  border-radius: 7px;
  padding: 3px 4px;
}
.fmt-btn {
  font-size: 9px;
  font-weight: 700;
  letter-spacing: .08em;
  padding: 4px 10px;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  color: rgba(255,255,255,.28);
  background: transparent;
}
.fmt-btn.active {
  background: var(--accent);
  color: #0A0A0A;
}
.ui-sep {
  width: 1px;
  height: 9px;
  background: rgba(255,255,255,.1);
  flex-shrink: 0;
}
.exp-btn {
  font-size: 9px;
  font-weight: 700;
  letter-spacing: .06em;
  padding: 4px 11px;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  color: rgba(245,240,232,.5);
  background: rgba(255,255,255,.07);
}
.exp-btn.loading {
  opacity: .5;
  pointer-events: none;
}
```

必须包含切换和缩放逻辑：

```javascript
function sw(fmt) {
  ['landscape','portrait'].forEach(function(id) {
    var canvas = document.getElementById(id);
    var btn = document.getElementById('btn-' + id);
    if (canvas) canvas.classList.toggle('active', id === fmt);
    if (btn) btn.classList.toggle('active', id === fmt);
  });
  scale();
}

function scale() {
  var vw = window.innerWidth;
  var vh = window.innerHeight;
  var landscape = document.getElementById('landscape');
  var portrait = document.getElementById('portrait');
  if (landscape) landscape.style.transform = 'scale(' + Math.min(vw / 1920, vh / 1080) * .93 + ')';
  if (portrait) portrait.style.transform = 'scale(' + Math.min(vw / 1080, vh / 1350) * .93 + ')';
}

window.addEventListener('resize', scale);
scale();
```

必须引入 `html2canvas` CDN：

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
```

并包含导出函数：

```javascript
async function exportPNG() {
  var btn = document.getElementById('exp-btn');
  var active = document.querySelector('.canvas.active') || document.querySelector('.canvas');
  var id = active.id;
  var isLandscape = id === 'landscape';
  var W = isLandscape ? 1920 : 1080;
  var H = isLandscape ? 1080 : 1350;
  var prevStyle = {
    transform: active.style.transform,
    position: active.style.position,
    top: active.style.top,
    left: active.style.left,
    zIndex: active.style.zIndex
  };

  try {
    btn.textContent = '生成中…';
    btn.classList.add('loading');

    active.style.transform = 'none';
    active.style.position = 'fixed';
    active.style.top = '0';
    active.style.left = '0';
    active.style.zIndex = '88888';

    await new Promise(function(r) { setTimeout(r, 80); });

    var canvas = await html2canvas(active, {
      scale: 2,
      width: W,
      height: H,
      useCORS: true,
      allowTaint: true,
      logging: false,
      backgroundColor: '#0A0A0A'
    });

    var a = document.createElement('a');
    a.download = 'thumbnail-' + id + '.png';
    a.href = canvas.toDataURL('image/png');
    a.click();
  } catch (err) {
    alert('导出失败，请确认浏览器可以联网加载 html2canvas，或刷新页面后重试。');
  } finally {
    active.style.transform = prevStyle.transform;
    active.style.position = prevStyle.position;
    active.style.top = prevStyle.top;
    active.style.left = prevStyle.left;
    active.style.zIndex = prevStyle.zIndex;
    scale();
    btn.textContent = '↓ 导出 PNG';
    btn.classList.remove('loading');
  }
}
```

导出规格：

- 横版 landscape：3840x2160px
- 竖版 portrait：2160x2700px
- 2x Retina

如果用户明确要求完全离线可用，告诉用户需要下载 `html2canvas.min.js` 到本地，并把 `<script src="...">` 改成本地路径；默认不要要求用户下载或配置。

## Quality Checklist

生成 HTML 前检查：

- 是否已经确认主标题、强调短语、副标题、主色调和视觉主题
- 是否至少有一个主色底深色字色块
- 是否只有一个主色底色块
- 主色底色块文字是否为 `#0A0A0A`
- 横版和竖版是否都没有文字溢出
- 竖版内容视觉高度是否占画布 70-85%
- 是否没有任何动画
- 是否没有 `animation-delay`
- 是否包含左侧主色竖条、Ghost 水印、噪点纹理、点阵背景
- SVG defs id 是否使用 `landscape-` / `portrait-` 前缀
- 两个 canvas id 是否没有冲突
- 是否正确引入 `html2canvas` CDN，并在导出失败时恢复按钮和画布状态

## Failure Handling

- 如果用户只给了模糊主题，先提炼 3 个封面方向，不直接生成 HTML。
- 如果主标题太长，主动拆行或改为竖版全宽文字布局，不要硬塞。
- 如果用户没有强调短语，先根据主题建议 3 个候选。
- 如果用户没有视觉主题，先根据视频内容建议 2-3 个视觉方向。
- 如果用户要求动画，提醒封面是静态导出图，不应加入动画；如果要动画页，应改用 B-roll skill。
- 如果当前工具不能写文件，就输出完整 HTML 内容和建议文件名。

## One Sentence Principle

先确认封面的主标题、强调短语、主色调和视觉主题，再生成一个静态、强识别、可导出 PNG 的双尺寸 HTML 封面；主色底深色字色块是视觉锚点，必须有且只突出一个。
~~~

---

## 六、创建后检查并测试

创建完成后，请检查：

1. `SKILL_FOLDER_NAME/SKILL.md` 是否存在
2. frontmatter 是否包含 `name` 和 `description`
3. 第三节、第四节、第五节 `## Configuration` 章节中所有 `{{变量名}}` 是否已替换为实际值（不应该有任何剩余的 `{{}}` 占位符）
4. 是否强制要求先确认主标题、副标题、强调短语、主色调和视觉主题
5. 是否包含横版 1920x1080 和竖版 1080x1350 双尺寸规则
6. 是否包含主色底深色字色块规则
7. 是否明确禁止动画
8. 是否包含右下角切换按钮和导出 PNG 逻辑
9. 是否包含 `html2canvas` 导出说明
10. 是否没有写入私人路径、API key、token 或 secret

**功能验证：** 请用下面这条请求模拟触发一次 skill，确认输出符合预期：

> “帮我做一个视频封面，主题是用 AI 改造 Obsidian 笔记流程，主标题是 Obsidian AI，强调短语是 把 AI 放进笔记里，副标题是 不用再在软件之间来回切换。”

期望行为：AI 应先整理主标题、强调短语、副标题、主色调和视觉主题，请用户确认，然后再生成包含横版 / 竖版双画布、切换按钮和导出 PNG 按钮的单文件 HTML，而不是直接跳过确认。

最后，请告诉我：

- skill 文件夹创建在哪里
- 创建了哪些文件
- 哪些配置项当前为空，使用时需要临时提供
- 如何在当前 AI 工具里触发这个 skill
