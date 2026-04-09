---
name: content-to-html
invocation: user
description: "Content to HTML — 文章转 HTML 页面、slides、deck 或海报。6 种输出格式，8 种风格预设，1280x720 固定画布。"
version: "4.0"
last_updated: "2026-04-09"
---

# Content to HTML — 6 种格式，统一入口

## 核心架构（最重要，先读这里）

### 四格式四方法论

| 格式 | 方法论 | 一句话 |
|------|--------|-------|
| **page** | Visual Hierarchy | 不压缩，分层赋权重 |
| **slides** | Assertion-Evidence | 找断言+找证据，一页一概念 |
| **deck** | Narrative Curation | 跨源策展，编织故事 |
| **poster** | AIDA Screenshot | 找截图点，3秒理解 |

### 6 步提取 Pipeline

```
0.文章类型识别 → 1.IU切分 → 2.信号扫描(+评分) → 3.IU选择 → 4.再格式化 → 5.布局匹配 → 5.5.叙事重排 → 6.保真度检查
```

每步的完整定义见 `format-specs.md`。这里是速查：

- **Step 0**：翻译+Takeaways? 教程? 观点? 案例? → 决定提取变体
- **Step 1**：全文切分为信息单元（IU），每 IU = 1-3 句独立可理解的信息块
- **Step 2**：slides 用 5 信号（数字/对比/命名/引用/列表）扫描断言，poster 用 4 特征（信息差/数字冲击/可引用/有立场）扫描截图点
- **Step 3**：slides 选 40-55% IU，poster 选 4-10% IU
- **Step 4**：断言→slide 标题，证据→body 内容，数据→表格/KPI，引用→Quote
- **Step 5**：IU 数量和类型 → 选 slide type → 输出 Extraction Plan Table
- **Step 6**：对照保真度矩阵验证——slides 禁新增信息，poster 允许文案化

**这个 skill 的质量取决于 Pipeline 执行得多好。CSS/字体/动画都是装饰。**

---

## 格式总览

| 格式 | 画布 | 输入 | 输出 |
|------|------|------|------|
| **page** | 响应式滚动 | 一篇长文 | 精美可阅读 HTML 页面 |
| **slides** | 1280x720 (16:9) | 一篇长文 | 提炼要点翻页演示 |
| **deck** | 1280x720 (16:9) | 多篇碎片 | 策展叙事演示 |
| **poster-9x16** | 1080x1920 | 文章要点 | 竖屏海报/小红书 |
| **poster-1x1** | 1080x1080 | 核心观点 | 方形社交卡片 |
| **poster-16x9** | 1920x1080 | 文章摘要 | 横屏分享卡 |

## Commands

| Command | What |
|---------|------|
| `/content-to-html` | Interactive: choose format |
| `/content-to-html page <path>` | Article → HTML page |
| `/content-to-html slides <path>` | Article → 16:9 slide deck |
| `/content-to-html deck` | Multi-source → narrative deck |
| `/content-to-html poster <path>` | Article → poster (选尺寸) |

## NOT for

| Need | Use instead |
|------|-------------|
| 从零创建 slides（无原文） | `zara-frontend-slides` |
| 数据分析 → ECharts 可视化卡片 | `/data-slides` |
| AI 生图配图 | `/cover-gen` |
| 单个图表 | `/chart` |

## Bundled Assets（本 skill 目录下）

| 文件 | 用途 |
|------|------|
| `format-specs.md` | 6 种输出格式的提取策略+画布规范 |
| `slide-design-principles.md` | **PPT 排版设计原则（三分法/留白/图文/卡片/列表布局）** |
| `slide-types.md` | 15 种 slide 类型的精确 px 布局 |
| `STYLE_PRESETS.md` | 8 种风格预设（CSS 变量+字体+氛围） |
| `viewport-base.css` | 双模式 CSS（固定卡片 + 滚动页面） |
| `html-template.md` | slides/deck/poster HTML 架构 |
| `animation-patterns.md` | 动画分角色系统 |
| `slide-schema.md` | **JSON Schema for 15 slide types（代码即策略的数据契约）** |
| `slide-engine.html` | **JSON→HTML 渲染引擎（self-contained，8 预设 + 15 type + nav + 验证）** |
| `quality-gate.md` | 三层质量控制 |
| `mermaid-guide.md` | Mermaid 集成 + 7 坑指南 |

---

# Phase 0: 定位文章（共享，必做）

1. 用户指定路径，或给关键词搜索
2. Read 文章全文
3. Glob 同目录 `images/` 扫描图片
4. 逐个 Read 图片评估可用性
5. 提取 metadata：title, date, tags, description

**输出**：文章字数 + 图片数量 + 结构概览

---

# Phase 1: 选择格式（共享，必做）

用户命令已指定格式则跳过。否则 AskUserQuestion：

| 选项 | 说明 |
|------|------|
| page | 完整文章 → 可阅读网页 |
| slides | 文章要点 → 16:9 翻页演示 |
| deck | 多篇碎片 → 叙事演示 |
| poster | 文章要点 → 固定尺寸海报（再选 9:16 / 1:1 / 16:9） |

---

# MODE: page

Read `format-specs.md` Format 1 获取完整规范。

## Phase 2p: 分析结构
从文章提取：标题+副标题、一句话摘要、目录结构、特殊内容块、阅读时间。

## Phase 3p: 选风格
6 个 page 预设（Editorial / Notebook / Terminal / Ink / E-Ink Paper / Magazine Grid）。
生成 3 个 hero 预览到 `/tmp/content-html-previews/`。

## Phase 4p: 生成
1. Read `quality-gate.md` Layer 1 — State Your Approach + Slop Test
2. Read `format-specs.md` Format 1 — 布局/字号/间距
3. 按 Content-Type Routing 渲染各内容块
4. 必须包含 6 种交互 + OG tags + Print CSS

保存：`{文章目录}/html/{slug}.html`

---

# MODE: slides

Read `format-specs.md` Format 2 + `slide-types.md` 获取完整规范。

## Phase 2s: 结构提取（两步，核心）

### Step 1: 内容提取 — Assertion-Evidence 适配

**核心**：每张 slide = assertion 标题（结论句）+ visual evidence（视觉证据）。
标题必须是完整结论句（≤20 词），不是主题词。无演讲者场景，标题要独立可理解。

| 标题写法 | 例子 | 判断 |
|---------|------|------|
| 结论句 | "Meta 用 50+ Agent 把上下文覆盖从 5% 提到 100%" | ✅ 断言 |
| 主题词 | "Meta 的 AI Agent 方案" | ❌ 不是结论 |
| "和"测试 | "Agent 提升了覆盖率和减少了工具调用" | ❌ 含"和"，拆两页 |

**visual evidence 层级**（决定 body 元素）：
- 数字/数据 → stat callout / chart → Hero Stat / Dashboard
- 对比 → split panel → Before-After / Split
- 流程 → flow diagram → Diagram / Timeline
- 概念定义（无自然视觉） → 结构化文本块 → Content（不强行画图）

**字数预算**：标题 ≤20 词，evidence 区 ≤60 词，超过 → 紧凑 Slidedoc 模式（上限 250 词）

**提取规则**：
- Content type: 每条 bullet = **核心论断（22px Heading 粗体）+ 支撑细节（15px Small 说明）**，每条占 2-3 行
- Dashboard type: 每张卡片 = **标题 + 数据/描述**，不能只有标题
- Table type: 每行 = 完整数据，不省略
- Quote type: 选原文中最有力的一句话，不自己编
- Hero Stat: 只用于单个冲击性**数字**（"42%"/"$1.2B"），概念用 Quote

### Step 2: Type 选择 — 匹配内容量

根据提取到的内容量和性质选 slide type，不是先选 type 再填内容。

| 内容特征 | 选什么 type |
|---------|------------|
| 3-5 条论断+细节，每条 2-3 行 | **Content**（bullet list） |
| 4-6 个并列概念/模块，每个有标题+描述 | **Dashboard**（卡片 grid） |
| 1 个核心数字是全页信息 | **Hero Stat**（大数字居中） |
| 2 组对比（A vs B） | **Split**（双栏） |
| 改造前后/效果对比 | **Before-After**（灰+绿双面板） |
| 3 个方案/选项对比 | **Three-Column**（三栏对比） |
| 有数据表格（4+ 行） | **Table** |
| 时间线/分阶段进展 | **Timeline**（水平轴+节点） |
| 二维分类/优先级矩阵 | **2x2 Matrix**（四象限） |
| 有架构图/流程图 | **Split**（图+说明）或 **Diagram** |
| 一句有力引用 | **Quote** |
| 3 个分级/分层概念 | **Content**（tier cards） |

**空白检测规则**：如果一页提取出来的内容不到 5 行纯文本，说明内容太少，要么回原文补充细节，要么合并到相邻页。

Read `slide-types.md` Content Density Limits — 超过上限拆页，低于下限补充或换 type。

### Extraction Plan Table（必做，用户确认后才生成）

提取完内容后，输出一张表给用户审阅：

```
| 原文位置 | 提取内容 | Slide # | 布局类型 | 内容量 | 预估填充率 | 密度策略 |
|---------|---------|---------|---------|--------|-----------|---------|
| P1-3 | 问题: 4仓库+逻辑错+静默出错 | S2 | Content | 5条x2行 | ~60% ✅ | D=1.0 标准 |
| P4 | 6个子系统 | S3 | Dashboard | 6卡片 | ~40% ⚠️ | D=1.25 中等 |
| P5-6 | 50+ Agent + 架构图 | S4 | Split | 图+3行 | ~35% ⚠️ | 降级→Content |
| P7 | 五问框架 | S5 | Content | 5条x2行 | ~60% ✅ | D=1.0 标准 |
```

检查项：
- 每页预估填充率标注（✅ >60% / ⚠️ 40-60% / ❌ <40%）
- 填充率 <40% 的页必须标注密度策略（Breathing 档位或降级方案）
- 全 deck 平均填充率决定统一密度系数 D（字号不变，间距×D）
- 布局类型是否匹配内容特征
- 原文关键论据是否遗漏
- 叙事顺序是否合理

用户确认后才进入生成阶段。这是从 data-slides 学到的核心模式：**提取过程可见、可审**。

### 叙事顺序模板（from data-slides）

Slides 不是按原文顺序罗列，要有叙事弧线：

```
Title → Problem(为什么重要) → Scale(多大规模) → Solution(怎么做) → Detail(关键细节) → Results(效果数据) → Insight(核心洞察) → Action(怎么用) → Closing
```

每页推进一步。用户可以在大纲阶段调整顺序。

## Phase 3s: 选风格
Read `STYLE_PRESETS.md` — 8 个预设。推荐 3 个，生成 title slide 预览。

## Phase 4s: 生成
1. Read `quality-gate.md` Layer 1
2. Read `format-specs.md` Format 2 — 1280x720 固定卡片架构
3. Read `slide-types.md` — 每种 type 的精确 HTML + CSS + 间距
4. Read `animation-patterns.md` — 动画参考
5. 派发 sub-agent 生成，prompt 包含：大纲 + 预设 CSS + slide-types 规范

### 两条生成路径（v4.0）

| 路径 | 适用场景 | sub-agent 输出 | 设计规则位置 |
|------|---------|---------------|-------------|
| **JSON→Engine** | 标准 slides/deck（推荐） | slide-schema.md 定义的 JSON | 锁在 slide-engine.html 渲染引擎里 |
| **直接 HTML** | page 模式、poster 模式、需要自定义布局时 | 完整 HTML+CSS | 散在 slide-types.md 各 type 定义里 |

JSON 路径：sub-agent 只负责内容决策（选 type、填内容），设计规则零犯错空间。
直接 HTML 路径：灵活但依赖 sub-agent 遵守 slide-types.md 规范。

### 画布核心

```css
body { background: #1a1a2e; display: flex; justify-content: center; align-items: center; min-height: 100vh; overflow: hidden; }
.slide { width: 1280px; height: 720px; position: absolute; top: 50%; left: 50%; transform: translate(-50%,-50%); box-shadow: 0 8px 32px rgba(0,0,0,.25); border-radius: 6px; overflow: hidden; }
```

### 5 层结构（非 Title/Divider/Quote 页）

```
hdr  16px 40px 0  (~40px)
sub  4px 40px 0   (~20px)
body 12px 40px 0  (flex:1, ~628px)
ft   8px 40px 12px (~32px)
```

保存：`{文章目录}/presentation/index.html`

---

# MODE: deck

Read `format-specs.md` Format 3。画布同 slides（1280x720）。

## Phase 2d: 收集 + 提取（核心区别：多源策展）

### Step 1: 收集素材
1. 用户提供多篇文章路径或关键词
2. Read 每篇来源，标记与主题相关的段落
3. AskUserQuestion 确认主题、听众、时长

### Step 2: Extraction Plan Table（deck 版）

```
| 来源文章 | 提取内容 | Slide # | 布局类型 | 叙事作用 |
|---------|---------|---------|---------|---------|
| 《做了三版 Data Agent》 | 核心痛点: 隐性知识难获取 | S2 | Content | 背景-问题 |
| 《Meta 隐性知识》 | 50+ Agent 预计算方案 | S4 | Split(图) | 过程-解法 |
| 《OpenAI Data Agent》 | Less is More 原则 | S6 | Quote | 过程-佐证 |
| 用户口述 | 自己的实践经验 | S8 | Content | 结果-验证 |
```

检查项（同 slides）+ 额外：
- 每页标注了来源吗（`—— 《文章标题》`）
- 叙事弧线是否连贯（Opening → 背景 → 过程 → 结果 → 收获 → Closing）
- 多篇来源的内容没有重复/矛盾

### Step 3: Type 选择
同 slides 的内容→布局映射规则。

---

# MODE: poster

Read `format-specs.md` Format 4/5/6（按尺寸选）。

## Phase 2-poster: 选尺寸 + 提取

### Step 1: 选尺寸
AskUserQuestion：9:16（竖屏/小红书）/ 1:1（方形/社交）/ 16:9（横屏/封面）

### Step 2: 极致提取

海报是极致压缩。从整篇文章里只保留最有冲击力的内容。

| 尺寸 | 提取什么 | 上限 |
|------|---------|------|
| **9:16** | 1 个标题 + 最多 5 个要点（每个带 1 行说明）+ 1 个 highlight 数据/引用 | 300 字 |
| **1:1** | 1 个核心观点（大字）+ 最多 3 个支撑数据 | 100 字 |
| **16:9** | 左栏：标题 + 3-5 bullet / 右栏：1 个大元素（图/引用/数据） | 150 字 |

### Step 3: Extraction Plan（poster 版，单行即可）

```
| 提取内容 | 区域 | 字数检查 |
|---------|------|---------|
| 标题: "Meta 用 50+ Agent 提取隐性知识" | poster-title | 15字 ✅ |
| 要点1: 瓶颈是隐性知识不是模型 + 4仓库3语言 | body-section1 | 30字 ✅ |
| 要点2: 指南针不是百科全书 + 25行/文件 | body-section2 | 25字 ✅ |
| highlight: "工具调用减少 40%" | body-stat | 10字 ✅ |
| 总计 | | 120字 ✅ (上限300) |
```

## Phase 3-poster: 选风格
同 slides 预设。

## Phase 4-poster: 生成
单张固定尺寸 HTML 卡片。严格遵守 format-specs.md 的间距和字号规范。

保存：`{文章目录}/poster/{slug}-{ratio}.html`

---

# Shared: Error Warnings

| 错误 | 正确 |
|------|------|
| page 正文超 720px | max-width: 720px |
| slides 一页塞太多 | Read slide-types.md density limits |
| slides body 内容浮在中间 | 检查 justify-content 策略 |
| 字号不在 6 级表内 | 只用 Display/Title/Heading/Body/Small/Label |
| side padding 不统一 | slides 40px, poster 48-60px, page 24px |
| 图片用绝对路径 | 相对路径 ../images/ |
| 使用 Inter/Roboto/Arial | 禁止 |
| slides 用了 Reveal.js | 不用，固定 1280x720 卡片 |
| emoji 做图标 | SVG 或 CSS 圆点 label |
| CSS `-clamp(...)` | `calc(-1 * clamp(...))` |

# Shared: Quality Gate

Read `quality-gate.md`。交付前必过 Squint Test + Swap Test + 格式对应检查清单。

---

# Version

- **v4.0** — 2026-04-09 — 代码即策略架构落地（N6）：新增 slide-schema.md（15 type JSON 契约）+ slide-engine.html（渲染引擎，8 预设 × 15 type × density 系数 × 验证）。sub-agent 输出 JSON，设计规则全锁在引擎里
- **v3.9** — 2026-04-09 — Gamma/Manus/AnyGen 调研落地 5 项：密度系数公式化（间距=base×D，替代查表）、全局字号一致性铁律（字号跟层级走不跟密度走，修正 R1）、HTML 约束清单（from Manus：禁渐变/显式声明/背景色只加 div）、Extraction Plan 加密度预估列、截图验证闭环（Playwright 5 项自检）
- **v3.8** — 2026-04-09 — PPT agent 调研 R1-R4 落地：去掉 aspect-ratio 改 min-height+padding，稀疏内容布局降级矩阵，Card Anti-Float 背景差异
- **v3.7** — 2026-04-09 — 4px 网格间距，容器约束级别表
- **v3.6** — 2026-04-09 — Review 驱动修正：容器不撑满 body（禁 height:100%），body 负责居中；内容关系→布局映射（并列等权用 cards 横排 vs 递进有序用编号纵排）；Pipeline 矮宽横条不是高瘦竖卡
- **v3.5** — 2026-04-09 — 调研报告增量提炼 6 条规则：anti-bullet-list 共识、节奏控制（5-7 张插 1 张变化页）、视觉元素 ≤6 上限、Table+Callout 变体、Content anti-patterns checklist、accent 色战略性使用规则
- **v3.4** — 2026-04-09 — Body fill strategy 修正：Content/Tier/Pipeline 类型从 flex-start 改为 space-evenly，填充验证表增加策略列和新 type 行
- **v3.3** — 2026-04-09 — A-E 方法论整合（assertion 标题规则+visual evidence 层级+字数预算），新增 5 种 slide type（Hero Stat/Timeline/Before-After/2x2 Matrix/Three-Column），Pipeline 加 Step 5.5 叙事重排+信号评分，全量 typography 对齐 presentation scale
- **v3.1** — 2026-04-08 — 6 种输出格式（+poster 3 种），去掉 Reveal.js 改 1280x720 固定卡片，新增 format-specs.md，slide-types.md 精确到 px，slide 5 层结构铁律
- **v3.0** — 2026-04-08 — 开源级重构
- **v2.0** — 2026-04-08 — 合并 content-to-html + makino-article-to-slides
- **v1.0** — 2026-04-07 — Initial

# Credits

Page design: [ship-page-skill](https://github.com/Zooeyii/ship-page-skill), [visual-explainer](https://github.com/nicobailon/visual-explainer), [visualize](https://github.com/careerhackeralex/visualize)
Slides layout: [makino-data-slides](data-slides spacing rules), [revealjs-skill](https://github.com/ryanbbrown/revealjs-skill)
Style system: [ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill), [zara-frontend-slides](https://github.com/anthropics/zara-frontend-slides) (MIT)
