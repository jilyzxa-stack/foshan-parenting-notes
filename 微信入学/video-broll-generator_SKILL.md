---
name: video-broll-generator
description: 生成视频 B-roll 配套的动画字幕页面和过场动画页面，输出单文件 HTML。适用于"帮我做 B-roll""做视频配套页面""做过场动画""做内容页""做视频字幕页""做阶段标题""视频素材""做配套 slides"，或用户上传视频脚本、字幕文件并要求制作配套视觉素材时。默认先提炼并确认页面大纲，再生成 HTML。不适用于纯演示文稿、普通文档或纯文字内容。
---

# Video B-roll Generator

## 角色

你是视频 B-roll 配套动画页面设计师，不是普通 PPT 制作者。

你要创建的是 animated HTML slide deck，用于视频剪辑中的 B-roll overlay、阶段标题、过场动画、内容字幕页、数据页或视觉解释页。

每一页都必须像已经进入视频制作流程的素材：

- 有明确目的
- 信息少而准
- 视觉克制但有冲击力
- 动画能重播
- 适合全屏录制
- 适合插入视频段落之间

这不是 presentation tool。不要生成普通演示文稿风格，不要塞满文字，不要做教学 PPT。

## Configuration

<!-- 以下配置由安装向导在创建时填入。如需修改，直接编辑对应值。 -->

```yaml
DEFAULT_OUTPUT_DESTINATION: "desktop"
# 生成好的 HTML 保存到哪里
# "desktop"     = 在桌面新建 HTML 文件
# "inline-only" = 只在对话中输出完整 HTML，不写入文件
# 或填入具体路径，如 /Users/xxx/Documents/video-assets

DEFAULT_ASPECT_RATIO: "9:16"
# 默认画面比例
# "16:9" = 横屏视频
# "9:16" = 竖屏短视频
# "1:1"  = 方形内容

DEFAULT_ACCENT_COLOR: "#1c78fd"
# 主强调色
# "#1c78fd" = 用户品牌蓝色
# 如果用户只描述风格，没有给 hex，AI 先匹配一个适合深色背景的高对比 hex

DEFAULT_STYLE_DIRECTION: "brand-color-cinematic"
# 视觉风格方向
# "dark-clay-cinematic" = 默认：深色背景 + 暖赤陶橙强调色
# "brand-color-cinematic" = 深色背景 + 用户品牌主色
# 也可以是用户自然语言描述，如 "冷蓝极简" / "温暖复古"

DEFAULT_NAV_MODE: "manual"
# 默认播放方式
# "manual" = 键盘 / 点击翻页，适合录屏
# "auto"   = 自动播放，每页按推荐时长停留

ALLOW_EXTERNAL_FONTS: true
# 是否允许通过 @import 加载 Google Fonts

ALLOW_EXTERNAL_JS_CSS: false
# 不允许依赖外部 JS/CSS，最终 HTML 必须自包含
```

## Boundary

不适用于纯演示文稿、普通文档、长篇文字排版、不需要动画的静态说明页。如果用户要的是正式演示文稿，提醒用户改用 presentation 类 skill。

## Required Inputs

优先收集：

- 视频主题
- 视频脚本、字幕、逐字稿或大纲
- 视频平台和画面比例
- 需要几页 B-roll
- 是否需要封面页、痛点页、阶段标题页、内容页、数据页、结尾页
- 是否有品牌色、字体、Logo 或频道视觉系统
- 是否要输出为单 HTML 文件
- 是否需要保存到本地文件

不要机械追问所有问题。只追问影响页面结构、视觉系统和输出文件的关键缺失项。

默认假设：

- 输出画面比例由 `DEFAULT_ASPECT_RATIO` 决定，默认 9:16
- 输出单个自包含 HTML 文件
- 视觉风格由 `DEFAULT_ACCENT_COLOR` 和 `DEFAULT_STYLE_DIRECTION` 共同决定；默认深色背景 + `#1c78fd` 品牌蓝强调色
- 先确认大纲，再生成 HTML
- 翻页方式由 `DEFAULT_NAV_MODE` 决定，默认手动翻页录屏

## Required Workflow

### Step 1：从脚本或 brief 中提炼结构

先分析用户提供的内容，提取：

- 视频前半段提到的痛点
- 视频的主要阶段或章节
- 每个阶段的关键点
- 哪些内容适合做视觉页
- 哪些内容适合做数据图表
- 哪些内容不适合展开，应该删减
- 是否存在语音转文字错误

如果用户脚本来自语音转文字，要主动根据上下文判断和校正专有名词、工具名和英文词汇，不依赖固定词表。

校正原则：
- 根据上下文语义判断
- 不确定时标记为待确认
- 不要把可疑词直接放进最终页面

### Step 2：先确认页面大纲

始终先向用户展示 B-roll 页面大纲，不要直接生成 HTML。

对每个内容页，必须明确选择：

- 页面类型
- 使用的视觉组件
- 使用的主动画
- 页面中的关键文字

默认大纲格式：

```text
封面页       → [主标题 + 副标题]
痛点页       → [痛点风格: X全屏/Y分屏/Z打字/W卡片] + [痛点1 / 痛点2]
─────────────────────────────────
阶段01标题页 → [阶段名 + 副标题]
阶段01内容页 → [组件: A比较卡/B流程/C界面/D网格/...] + [动画: reveal-lr/slide-up/flip-up/...]
─────────────────────────────────
阶段02标题页 → [阶段名 + 副标题]
阶段02内容页 → [组件] + [动画]
...
─────────────────────────────────
结尾页       → [结语]
```

必须等用户确认后，才能生成 HTML。

### Step 3：生成单文件 HTML

用户确认大纲后，生成一个单文件 HTML：

- 所有 CSS 写在 `<style>`
- 所有 JS 写在 `<script>`
- 不依赖外部 JS/CSS
- 允许通过 `@import` 加载 Google Fonts
- 支持全屏浏览器播放
- 支持键盘、点击、进度点导航
- 所有动画必须可重复播放

默认文件命名：

```text
[topic]_broll.html
```

## Design System

视觉系统（`DEFAULT_ACCENT_COLOR: #1c78fd`）：

```text
Background:      #0A0A0A
Primary text:    #F5F0E8
Secondary text:  rgba(245, 240, 232, 0.45)
Dim text:        rgba(245, 240, 232, 0.13)
Accent:          #1c78fd
Card bg:         rgba(255, 255, 255, 0.05)
Card border:     rgba(255, 255, 255, 0.09)
Card radius:     12-14px

Fonts:
  Display / numbers:  'Space Grotesk', sans-serif  (weight 700, 900)
  Chinese + body:     'Noto Sans SC', sans-serif   (weight 300, 700, 900)
```

CSS 根变量（必须同步定义）：

```css
:root {
  --y: #1c78fd;
  --y-rgb: 28,120,253;
}
```

所有 glow、text-shadow、透明线条、图表高亮都必须使用这两个变量，不要硬写色值。

所有页面应持续使用这些视觉母题：

- 左侧强调色 side-bar：5px 宽，全高
- Ghost outline numbers：`-webkit-text-stroke`，`color: transparent`
- 细强调色线条：1px，约 40% opacity
- Noise texture overlay：SVG fractal noise，约 4% opacity
- Corner tag：右下角，小号 Space Grotesk，小写大写风格，强调色 25% opacity

## Centering Rules

每一页都必须真正居中。必须遵守：

1. 基础 `.slide` 样式：

```css
.slide {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 5vh 8vw;
}
```

2. 内容 wrapper 使用 `max-width` 约束，不要只靠 padding。
3. Full-bleed slides，例如阶段标题页、痛点页，要在该页设置 `padding:0`。
4. 痛点页文字 wrapper 设置 `width:100vw`，确保相对真实 viewport 居中。
5. 痛点文字禁止使用 `white-space:nowrap`，避免溢出和伪居中。
6. 阶段标题数字使用 `.st-group` flex row，不要用 absolute + left 百分比定位。

## Emphasis Rules

强调色 `<em>` 很珍贵，必须少用：

- 每行最多 1-2 个 `<em>`
- 只高亮产品名、关键动词、最重要的一个词
- 不高亮连接词、介词、完整从句
- 不整句变色

`em` 样式：

```css
em {
  font-style: normal;
  color: var(--y);
  font-size: 1.08em;
  text-shadow:
    0 0 20px rgba(var(--y-rgb), .5),
    0 0 50px rgba(var(--y-rgb), .2);
}
```

示例：

```text
好：想在 <em>Obsidian</em> 中直接用 <em>Claude Code</em>？
好：用 <em>AI</em> 处理笔记，不想在软件之间<em>横跳</em>？
坏：想在 Obsidian 中<em>直接用 Claude Code</em>？
坏：<em>用 AI 处理笔记，不想在软件之间横跳</em>？
```

## Animation Rules

### 硬规则：所有动画必须 JS 驱动

所有动画元素必须有独立 ID，统一使用 JS `A()` 函数驱动。禁止在 CSS 中给元素写死 `animation` 属性。

原因：用户翻页或回到某一页时，动画必须完整重播。

必须包含：

```javascript
function A(id, kf, dur, delay) {
  const el = typeof id === 'string' ? document.getElementById(id) : id;
  if (!el) return;
  el.style.animation = 'none';
  void el.offsetWidth;
  el.style.animation = `${kf} ${dur}s ease ${delay}s forwards`;
}
```

### 痛点页 nth-child 规则

痛点页如果有 `.psep` 分隔线，CSS `nth-child` 会算错顺序，可能导致第 2 个痛点不显示。

必须：

- 给每个 `.pain-item` 独立 ID
- 用 JS `A(id, kf, dur, delay)` 直接驱动
- 不依赖 CSS `nth-child`

### 阶段标题页 sweep 重播规则

`.sweep-block` 必须满足：

1. 宽度使用百分比，不能用 px。
2. CSS、JS 重置、keyframes 三处保持一致：

```css
.sweep-block {
  width: 120%;
  left: -120%;
}

@keyframes sweep {
  0% { left: -120%; }
  42% { left: 0; }
  58% { left: 0; }
  100% { left: 100%; }
}
```

JS 每次进入标题页必须硬重置：

```javascript
const sw = document.querySelector('#current-slide .sweep-block');
if (sw) {
  sw.style.animation = 'none';
  sw.style.left = '-120%';
  void sw.offsetWidth;
  sw.style.animation = 'sweep 1.8s cubic-bezier(0.77,0,0.18,1) 0.2s forwards';
}
```

## Slide Types

### 1. 封面页 Cover

- 深色背景，强调色 radial glow，轻微 pulse
- 大标题居中，1-2 个关键词用强调色 `<em>`
- 标题下方 2px 强调色线条展开
- 副标题使用 secondary color
- 标题上方有小号 eyebrow label，强调色，letter-spacing: 0.35em
- 动画顺序：
  - eyebrow：0.4s
  - title：0.9s
  - line：1.6s
  - subtitle：2.0s

### 2. 痛点页 Pain Points

痛点页不需要 header title，直接进入痛点。

规则：

- 最多 2-3 个痛点
- 严格遵守 `<em>` 高亮规则
- 每个 `.pain-item` 必须有独立 ID
- 用 JS `A()` 驱动，不用 nth-child

每个视频选择一种痛点风格：

#### 风格 X：全屏逐句冲击

默认推荐。

- 每个痛点是大号居中句子
- full viewport width
- 逐句 blur-in-big 出现，0.6s stagger
- 文本出现后画出强调色分隔线
- 可加极轻微 conic-gradient ray effect
- `#s2 { padding:0 }`
- wrapper `width:100vw; text-align:center`

适合：快节奏、强痛点视频。

#### 风格 Y：斜切分屏

- 两个 full-height panel
- 中间 diagonal clip-path
- 强调色 diagonal slash divider
- 每侧一个痛点
- 上方小标签：痛点 01 / 痛点 02

适合：对比强、两个世界叙事。

#### 风格 Z：打字机

- terminal 风格 `›` prompt
- JS character-by-character 打字
- 不用 CSS `clip-path`
- 第一行打完，cursor 闪烁，再打第二行

适合：技术、开发者、命令行主题。

#### 风格 W：卡片飞入

- 两张卡片从左右飞入
- landing 时轻微 rotation，约 1.5deg
- 卡片有强调色左边框和 ghost number
- side-by-side layout

适合：更轻松、编辑部风格视频。

### 3. 阶段标题页 Stage Title

阶段标题页是 signature slide，必须使用完整 sweep sequence，不要跳过或缩短。

结构必须使用 `.st-group` flex row：

```html
<div class="st-inner">
  <div class="st-group">
    <div class="st-num">01</div>
    <div class="st-content">
      ...
    </div>
  </div>
</div>
```

CSS 基础：

```css
.st-group {
  display: flex;
  align-items: center;
  gap: clamp(16px, 3vw, 48px);
  max-width: 820px;
  width: 90%;
}
.st-num {
  font-size: clamp(80px, 12vw, 160px);
  flex-shrink: 0;
  -webkit-text-stroke: 2px var(--y);
  color: transparent;
}
.st-content {
  text-align: left;
}
```

完整动画序列，总时长约 4.5s：

| Time | Element | Animation | Duration |
|---|---|---|---|
| 0.2s | Sweep block | enters -> pauses -> exits | 1.8s |
| 2.0s | Ghost number | slide-in-left | 1.4s |
| 2.1s | Step label | fade-up | 1.2s |
| 2.2s | Side-bar | bar-grow | 1.0s |
| 2.2s | Top accent line | line-l | 1.4s |
| 2.5s | Main title | fade-up | 1.2s |
| 2.5s | Bottom accent line | line-r | 1.4s |
| 2.9s | Subtitle | fade-up | 1.2s |
| 3.3s | Corner tag | fade-up | 1.2s |

### 4. 内容页 Section Title

每个内容页都应该有一个居中的 `.sec-title`，放在视觉组件上方。

```css
.sec-title {
  font-size: clamp(16px, 2vw, 24px);
  font-weight: 700;
  color: var(--y);
  letter-spacing: .08em;
  text-align: center;
  margin-bottom: 32px;
  width: 100%;
  opacity: 0;
}
```

### 5. 内容页 Visual Components

内容页必须变化：

- 每个内容页使用不同组件
- 每个内容页使用不同主动画
- 不要连续两页使用同一个主动画
- 不要用普通 bullet list

内容规则：

- 每页最多 4 个 item
- 中文每行不超过 10 字
- 英文每行不超过 6 个词
- 只用 fragments 和关键词
- 一页只讲一个概念

主动画轮换顺序：

```text
reveal-lr -> slide-up -> flip-up -> spin-in -> bounce-in -> blur-in -> pop-in -> sil/sir
```

`fade-up` 只能用于辅助文字，不作为内容页主动画。

#### 数据组件优先规则

- 有具体数字：优先 Counter(O) 或 Progress Bar(P)
- 3 项以上对比：横向柱状图(Q)
- 时间序列数据：纵向柱状图(R) 或折线图(T)
- 占比/构成：环形图(S)
- 多维评分：雷达图(U)

#### A. Left/Right Compare Card

用途：两个选项、方法 A vs B、before/after。

结构：

- 可选 prerequisite bar
- grid `1fr 36px 1fr`
- cards 包含 `cmp-badge`、`cmp-title`、编号 step list

动画：左卡 `sil`，右卡 `sir`，同时出现。

#### B. Flow Node Diagram

用途：顺序步骤、配置流程、pipeline。

结构：

- `display:flex; align-items:center`
- nodes + 固定宽度 connectors
- connector 使用 `flex: 0 0 clamp(24px, 4vw, 56px)`

动画：

- nodes `scale-in` 依次出现
- connectors `conn-grow`
- JS 点亮节点

#### C. Mock UI Window

用途：软件教程、点击步骤、命令步骤。

结构：

- 左侧 fake OS window
- 右侧 steps column
- wrapper `max-width:880px; display:flex; gap:4vw`

动画：

- window `sil`
- steps column `fade-in`
- steps `fade-up` stagger

#### D. Card Grid

用途：功能、工具、并列项。

结构：

- 2x2、2x3 或 3x3
- 卡片名要清楚可读
- 图标可使用简单字符或 inline SVG

动画：`pop-in` stagger。

#### E. Big Feature Cards

用途：2-3 个重要概念。

结构：

- ghost number background
- bold title
- description
- 3px 强调色 top border

动画：`blur-in` sequential。

#### F. Big Stat / Single Focus

用途：一个关键指标或单一重点。

结构：

- 巨大数字或关键词居中
- 下方 label

动画：数字/关键词 `scale-in`，label `fade-up`。

#### G. Hub & Spoke

用途：中心概念到多个输出或工具。

结构：

- SVG center box
- lines radiating to endpoint nodes

动画：

- center `scale-in`
- spokes stroke-dashoffset
- endpoints `pop-in`

#### H. Timeline

用途：步骤、阶段、路线图。

结构：

- dots on line
- labels 上下交替

动画：line `conn-grow`，dots sequential。

#### I. Terminal / Code Block

用途：命令行、配置、workflow 示例。

结构：

- terminal window
- `$` prompt
- monospace text

动画：JS character-by-character typewriter，不用 CSS clip-path。

#### J. Layer Stack

用途：系统层级、技术栈、工作流层级。

结构：

- stacked horizontal bars
- number + 强调色 left border + title + right-aligned desc

动画：bars `sil` bottom -> top。

#### K. Pull Quote

用途：核心洞察、金句。

结构：

- large decorative quote mark
- big centered text
- attribution

动画：quote mark `fade-in`，text `blur-in`。

#### L. Checklist

用途：最佳实践、完成感步骤。

结构：

- rows with checkbox
- text + sub-description

动画：rows `fade-up` stagger。

#### M. Split Screen

用途：before/after、old vs new。

结构：

- two full-height panels
- diagonal divider

动画：left `sil`，right `sir`。

#### N. Stacked Cards

用途：3-4 个层叠概念或递进步骤。

结构：

- cards offset/overlapping
- each slightly rotated

动画：cards `card-drop`。

#### O. Counter

用途：关键数据、百分比、增长率。

结构：

- 大数字
- 单位
- 标签

动画：JS `requestAnimationFrame`，从 0 滚动到目标值。

必须包含可复用函数：

```javascript
function counter(el, target, duration) {
  const start = performance.now();
  function tick(now) {
    const p = Math.min((now - start) / duration, 1);
    const ease = 1 - Math.pow(1 - p, 4);
    el.textContent = Math.round(ease * target);
    if (p < 1) requestAnimationFrame(tick);
  }
  requestAnimationFrame(tick);
}
```

#### P. Progress Bar

用途：完成度、占比、强弱对比。

结构：

- label
- 横向 bar
- 右端数值

动画：

- width 0% -> 目标百分比
- duration 1.2s
- 多条 stagger 0.2s
- 最高值强调色高亮

#### Q. Horizontal Bar Chart

用途：排名、多项对比。

结构：纯 SVG，每条 bar 从左到右展开。

动画：

- bar width 0 -> target
- stagger 0.15s
- 最高值强调色高亮

#### R. Vertical Bar Chart

用途：时间序列、月度/季度对比。

结构：SVG，bar 从底部向上生长。

动画：

- bar height 0 -> target
- stagger 0.1s
- value fade-up

#### S. Donut Chart

用途：占比、构成分析。

结构：

- SVG stroke-dasharray
- 中间核心数字
- 右侧 legend

动画：

- stroke-dashoffset 从全隐到全显
- legend `fade-up`

#### T. Line Chart

用途：趋势、增长曲线、时间变化。

结构：

- SVG path
- axis
- data points
- optional area fill

动画：

- stroke-dashoffset 绘制折线
- points `pop-in`
- area `fade-in`

#### U. Radar Chart

用途：多维能力对比、评分维度。

结构：

- SVG 多边形网格
- 数据多边形
- 顶点标签

动画：

- grid `fade-in`
- polygon `scale-in`
- labels `pop-in` stagger

图表色系：

```text
主色：#1c78fd
次色：rgba(245,240,232,0.6)
背景：rgba(255,255,255,0.05)
```

### 6. 结尾页 Outro

- 大号柔和强调色 radial glow
- 居中短句
- 1-2 个关键词用 `<em>`
- 2px 强调色线条展开
- 副标题用 secondary color
- 动画：
  - glow `scale-in` 2.0s, delay 0.3s
  - content `scale-in` 1.4s, delay 0.6s
  - line 1.4s

## Navigation

最终 HTML 必须包含：

```javascript
// Left / Right arrow keys + Space: navigate
// Click anywhere: advance
// F or double-click: fullscreen
// Progress dots: bottom center, clickable
// Auto-hide cursor: 3s inactivity
// All animations driven by JS A() function
```

`DEFAULT_NAV_MODE` 为 `manual`，保留键盘/点击手动翻页，不添加自动播放。

## Keyframes Reference

最终 HTML 必须包含这些核心 keyframes，按需使用：

```css
@keyframes sweep { 0%{left:-120%} 42%{left:0} 58%{left:0} 100%{left:100%} }
@keyframes fade-up { 0%{opacity:0;transform:translateY(28px)} 100%{opacity:1;transform:translateY(0)} }
@keyframes fade-up-sm { 0%{opacity:0;transform:translateY(16px)} 100%{opacity:1;transform:translateY(0)} }
@keyframes fade-in { 0%{opacity:0} 100%{opacity:1} }
@keyframes bar-grow { from{height:0} to{height:100%} }
@keyframes line-l { from{width:0} to{width:60%} }
@keyframes line-r { from{width:0} to{width:55%} }
@keyframes line-grow { 0%{width:0;opacity:0} 100%{width:100%;opacity:1} }
@keyframes sil { 0%{opacity:0;transform:translateX(-60px)} 100%{opacity:1;transform:translateX(0)} }
@keyframes sir { 0%{opacity:0;transform:translateX(60px)} 100%{opacity:1;transform:translateX(0)} }
@keyframes scale-in { 0%{opacity:0;transform:scale(.92)} 100%{opacity:1;transform:scale(1)} }
@keyframes blur-in { 0%{opacity:0;filter:blur(12px)} 100%{opacity:1;filter:blur(0)} }
@keyframes blur-in-big { 0%{opacity:0;filter:blur(14px);transform:scale(1.04)} 100%{opacity:1;filter:blur(0);transform:scale(1)} }
@keyframes pop-in { 0%{opacity:0;transform:scale(.85) translateY(12px)} 70%{transform:scale(1.03) translateY(-2px)} 100%{opacity:1;transform:scale(1) translateY(0)} }
@keyframes conn-grow { 0%{width:0;opacity:0} 100%{width:100%;opacity:1} }
@keyframes glow-pulse { 0%,100%{opacity:.13} 50%{opacity:.26} }
@keyframes card-drop { 0%{opacity:0;transform:translateY(-40px) scale(.97)} 100%{opacity:1;transform:translateY(0) scale(1)} }
@keyframes card-fly-l { 0%{opacity:0;transform:translateX(-100px) translateY(20px) rotate(-3deg)} 100%{opacity:1;transform:translateX(0) translateY(0) rotate(-1.5deg)} }
@keyframes card-fly-r { 0%{opacity:0;transform:translateX(100px) translateY(-20px) rotate(4deg)} 100%{opacity:1;transform:translateX(0) translateY(0) rotate(1.5deg)} }
@keyframes cursor-blink { 0%,100%{opacity:1} 50%{opacity:0} }
@keyframes slide-up { 0%{opacity:0;transform:translateY(60px)} 100%{opacity:1;transform:translateY(0)} }
@keyframes slide-down { 0%{opacity:0;transform:translateY(-60px)} 100%{opacity:1;transform:translateY(0)} }
@keyframes reveal-lr { 0%{clip-path:inset(0 100% 0 0)} 100%{clip-path:inset(0 0% 0 0)} }
@keyframes reveal-bt { 0%{clip-path:inset(100% 0 0 0)} 100%{clip-path:inset(0 0 0 0)} }
@keyframes spin-in { 0%{opacity:0;transform:rotate(-15deg) scale(.8)} 100%{opacity:1;transform:rotate(0) scale(1)} }
@keyframes bounce-in { 0%{opacity:0;transform:scale(0.3)} 50%{transform:scale(1.08)} 70%{transform:scale(0.95)} 100%{opacity:1;transform:scale(1)} }
@keyframes flip-up { 0%{opacity:0;transform:perspective(400px) rotateX(90deg)} 100%{opacity:1;transform:perspective(400px) rotateX(0)} }
@keyframes zoom-blur { 0%{opacity:0;filter:blur(20px);transform:scale(1.15)} 100%{opacity:1;filter:blur(0);transform:scale(1)} }
```

## Animation Speed Reference

| Phase | Duration | Feel |
|---|---|---|
| Sweep block | 1.8s | Cinematic |
| Ghost number | 1.4s | Confident |
| Stage text elements | 1.0-1.2s | Smooth |
| Content stagger | 0.15-0.4s apart | Rhythmic |
| Connectors | 0.7s | Snappy |
| Blur-in | 1.0-1.2s | Dreamy |
| Pop-in | 0.7s | Lively |
| Pain point blur-in-big | 1.0s / 0.6s stagger | Impactful |
| Card fly-in | 1.0s cubic-bezier(.22,1,.36,1) | Springy |
| Counter | 1.5-2.0s | Satisfying |
| Progress bar | 1.2s cubic-bezier(.22,1,.36,1) | Smooth |
| Outro | 2.0s | Grand |

默认节奏：

- Stage title：约 4.5s
- Content slide：约 2.5-3s
- Pain point：约 2.5-3s

## Output Rules

最终输出必须满足：

- 单个 HTML 文件
- 不依赖外部 JS/CSS
- 允许 Google Fonts
- 画面比例为 9:16 竖屏；必须调整字号、布局和组件密度，避免文字溢出；建议每页内容更少、字号更大、padding 更宽松
- 所有 CSS 在 `<style>`
- 所有 JS 在 `<script>`
- 支持现代浏览器全屏播放
- 所有内容真正居中
- 所有动画由 JS `A()` 函数触发
- 不在 CSS 中给具体元素写死 `animation`
- 内容页组件和主动画必须变化
- 每页文字极少，只放关键词和短句
- 默认文件命名为 `[topic]_broll.html`
- `DEFAULT_OUTPUT_DESTINATION` 为 `desktop`：写入前先告知目标文件名；如果同名文件已存在，先暂停确认

## Quality Checklist

生成 HTML 前检查：

- 是否已经让用户确认页面大纲
- 是否每个内容页都指定了组件和动画
- 是否没有把普通 bullet list 当成视觉组件
- 是否每页只讲一个概念
- 是否文字量适合视频画面
- 是否 `<em>` 没有滥用
- 是否痛点页没有依赖 nth-child
- 是否阶段标题页 sweep 能重播
- 是否所有动画元素都有独立 ID
- 是否导航功能完整
- 是否适合 9:16 竖屏全屏录制，文字不溢出

## Failure Handling

- 如果用户只给了模糊主题，先帮用户提炼 5-8 页候选大纲，不直接生成 HTML。
- 如果脚本太长，先抽取最适合做视觉页的片段。
- 如果内容更适合 presentation，而不是 B-roll，提醒用户并建议改用 presentation skill。
- 如果用户要求"直接生成"，仍然要先给极简大纲并请求确认，除非用户明确说"跳过确认"。
- 如果事实、工具名、英文词不确定，标记待确认，不要写进最终页面。
- 如果当前工具不能写文件，就输出完整 HTML 内容和建议文件名。

## One Sentence Principle

先把视频结构拆成适合镜头节奏的少量页面，再用极简、高对比、可重播的 HTML 动画做成视频素材；默认先确认大纲，再生成单文件 HTML。
