# Format Specs — 提取方法论 + 6 种格式规范

## 核心方法论（最重要，先读这里）

### 四格式四方法论

| 格式 | 方法论 | 一句话 | IU 选择率 |
|------|--------|-------|----------|
| **page** | Visual Hierarchy | 不压缩，分层赋权重 | 100% |
| **slides** | Assertion-Evidence | 找断言+找证据，一页一概念 | 40-55% |
| **deck** | Narrative Curation | 跨源策展，编织故事 | 20-30%/源 |
| **poster** | AIDA Screenshot | 找截图点，3秒理解 | 4-10% |

### 6 步提取 Pipeline（所有格式共享）

```
Step 0: 文章类型识别 → 选提取变体
Step 1: IU 切分 → 全文分解为信息单元
Step 2: 信号扫描 → 按格式信号给 IU 打分
Step 3: IU 选择 → 按选择率筛选
Step 4: 再格式化 → 适配目标布局
Step 5: 布局匹配 → 输出 Extraction Plan
Step 6: 保真度检查 → 验证不越权
```

---

### Step 0: 文章类型识别

| 文章类型 | 识别信号 | slides 提取变体 |
|---------|---------|---------------|
| **翻译+Takeaways** | frontmatter `doc_type: 翻译`, 有 Takeaways 段 | Takeaways 直接做断言，正文做证据 |
| **教程/How-to** | 编号步骤、代码块密集、"如何"/"步骤" | Step-by-Step: 每步=一页 |
| **观点/评论** | 明确立场、引用他人反驳 | Thesis-Evidence: 正题→反题→合题 |
| **案例/复盘** | 问题→方案→结果结构 | SCR: Situation→Complication→Resolution |
| **工具介绍** | 功能列表、对比表、截图 | Feature-Grid: 功能→cards，对比→table |
| **数据分析** | 表格、统计数字、图表描述 | Data-First: 数据→KPI，分析→断言 |

### Step 1: 信息单元（IU）切分

**IU 定义**：一个独立可理解的信息块，通常 1-3 句话。一篇 5000 字文章约 40-60 个 IU。

**切分规则**：
- 每个段落通常是 1-2 个 IU
- 列表的每一项是 1 个 IU
- 表格的每一行是 1 个 IU
- 引用块是 1 个 IU
- 代码块+说明是 1 个 IU

**IU 分类**：
- **断言 IU**：有明确立场或结论的陈述
- **证据 IU**：数据、例子、引用，支撑某个断言
- **过渡 IU**：连接段落的过渡句（"接下来..."）
- **背景 IU**：提供上下文但不含新论点
- **数据 IU**：包含精确数字或表格

### Step 2: 信号扫描

#### Slides 5 信号（标记断言候选）

| 信号 | 识别方式 | 优先级 |
|------|---------|--------|
| **数字** | 句中有精确数字（"40%"、"4100+"） | 最高——数字几乎总是好断言或好证据 |
| **对比** | "不是A而是B"、"从X到Y" | 高——天然有张力 |
| **命名** | 作者给概念起了名字（"指南针不是百科全书"） | 高——已精炼过的概念 |
| **引用** | 引用他人原话或定义 | 中——可做 Quote slide |
| **列表** | 原文有编号步骤或分类 | 中——直接映射为 cards 或 numbered items |

**简易评分**：每个 IU 按命中信号加分——数字 +5，对比 +4，命名 +4，引用 +3，列表 +2。7 天内热点话题 +3。取 top 40-55% 的 IU。

#### Poster 4 特征（标记截图点候选）

| 特征 | 定义 | 示例 |
|------|------|------|
| **信息差** | 读者大概率不知道 | "Meta 用 50+ 个 AI Agent" |
| **数字冲击** | 戏剧性的数字对比 | "从 2 天缩到 30 分钟" |
| **可引用** | 脱离上下文也能理解 | "过期的上下文比没有更危险" |
| **有立场** | 有明确判断 | "第一步不是部署 Agent 是让团队留痕" |

**截图冲动指数**：同时满足越多特征分越高。Top 1 做标题，Top 2-4 做要点。

### Step 3: IU 选择率

| 格式 | 选择率 | 选择标准 |
|------|--------|---------|
| page | 100% | 全选，做结构分层 |
| slides 14页 | 40-55%（~20-28 IU） | 信号强度排序，按叙事弧线组织 |
| deck 12页 | 20-30%/源 | 与主题相关性 × 叙事推进力 |
| poster 9:16 | ~10%（4-5 IU） | 截图冲动指数 top 5 |
| poster 1:1 | ~4%（1-2 IU） | 截图冲动指数 top 1 |

### Step 5.5: 叙事弧线重排

提取后的 slides 不应该按原文顺序排列。重排为叙事弧线：

```
Title → Problem(为什么重要) → Scale(多大规模) → Solution(怎么做) → Evidence(效果数据) → Insight(核心洞察) → Action(怎么用) → Closing
```

**重排规则**：
- 对照弧线检查每张 slide 推进了哪一步
- 如果两张 slide 推进同一步，考虑合并
- 如果某步骤没有 slide 覆盖，检查是否遗漏了关键 IU
- Before-After 放在 Evidence 位置，Quote 放在 Insight 位置

**节奏控制规则**（from Duarte Resonate）：
- 每 5-7 张内容 slide 后必须插入 1 张节奏变化页（Section Divider / Hero Stat / Quote）
- 数据密集区（连续 3+ 张 Content/Dashboard/Table）后接 1 张"所以呢"总结页（Quote 或 Hero Stat）
- Deck 开头用视觉冲击最强的布局（Title），结尾前倒数第 2 张用 Quote/Hero Stat 做情感收尾
- 检查方法：数 type 序列，连续 5 张同类型 = 节奏单调，必须打断

### Step 6: 保真度矩阵

| 操作 | page | slides | deck | poster |
|------|------|--------|------|--------|
| **文字改写** | 禁止 | 允许提炼（不改意思） | 允许重组+补过渡 | 允许文案化 |
| **结构重排** | 只增强 | 允许重新排序 | 允许跨源重组 | 完全重构 |
| **新增内容** | 禁止 | 禁止 | 允许过渡句 | 允许引导语/CTA |
| **省略内容** | 禁止 | 允许（跳过非核心） | 允许（只选相关） | 必须（极度精简） |

生成后检查：对照保真度矩阵验证，slides 里有没有新增原文没有的信息？poster 里有没有需要上下文才能理解的术语？

### 内容金字塔（跨格式复用）

```
page (100% 全文)
  ↓ IU 选择 40-55%
slides (要点+论据, 10-20 页)
  ↓ 截图冲动指数 top 3-5
poster (精华, 1 张)
```

降级提取是"选择"不是"压缩"。poster 从 slides 的 Extraction Plan 里选 top IU，不从原文重新提取。

---

## 格式决定提取策略（具体参数）

每种格式的内容提取策略不同。这是决定输出质量的第一步。

### page 提取策略
- **保留全文**，零压缩
- 原文每一段、每个代码块、每张图片都保留
- 只增加结构（TOC、section 分隔、highlight），不删减内容
- 图片保留原始引用路径

### slides 提取策略（Assertion-Evidence 适配版）

**核心原则**（from Alley/Duarte/Minto）：
- 每张 slide = **assertion 标题**（结论句，非主题词）+ **visual evidence**（视觉证据）
- 标题必须是完整结论句（≤20 词），独立可理解（无演讲者场景）
- 关键数字必须出现在 slide 上，不能只在"旁白"中
- "和"测试：标题含"和"就拆两页

**assertion 标题规则**：
- ✅ "Meta 用 50+ Agent 把隐性知识提取效率提升 40%" — 结论句
- ❌ "Meta 的 AI Agent 方案" — 主题词，不是结论
- ❌ "背景介绍" — 更不行

**视觉证据层级**（决定 body 用什么元素）：
| 断言支撑类型 | 渲染为 | slide type |
|------------|--------|-----------|
| 数字/数据 | stat callout 或 chart | Hero Stat / Dashboard |
| 流程/序列 | flow diagram 或 timeline | Diagram / Timeline |
| 对比 | split panel 或 comparison table | Before-After / Split |
| 关系/结构 | labeled diagram | Diagram |
| 举例 | quote card + attribution | Quote |
| 纯解释（无自然视觉） | 结构化文本块（label/value 对），不用段落 | Content |

**字数预算**：
- assertion 标题区：≤20 词
- evidence 区文字：≤60 词（标签/callout/注释）
- 超过 60 词 → 切换到 Slidedoc 模式（上限 250 词，排版需更紧凑）

**提取密度**：每 150-200 词源文 → 1 张 assertion slide

**每条 bullet = 核心论断（22px Heading 粗体）+ 支撑细节（15px Small 说明）**
每张 slide 的内容必须能撑满 616px body，不够就回原文补
数据/数字从原文精确提取，不概括

**A-E 失败时的 fallback**：
- 定义/概念类内容 → 结构化定义卡（term + attributes + example），不强行画图
- 纯叙事上下文 → 砍到 notes 或用 Quote 卡
- 多断言堆积 → section divider + 拆为 2-3 张子卡

**叙事排序**（Minto 金字塔 + STAR moment）：
- 先找文章高潮/核心结论（STAR moment），倒推哪些断言是承重的
- Title → Problem → Scale → Solution → Evidence → Insight(STAR) → Action → Closing

### deck 提取策略
- **重度策展**（~80%），从多篇文章选最相关的碎片
- 每页 = 叙事弧线的一步推进，不是罗列观点
- 引用原文必须标注来源 `—— 《文章标题》`
- 叙事弧线：Opening → 背景 → 过程 → 结果 → 收获 → Closing

### poster 提取策略
- **极致压缩**（~95%），只留精华
- 9:16 竖屏：最多 5 个 section，每 section 最多 4 bullet，总计不超过 300 字
- 1:1 方形：1 个核心观点 + 最多 3 个支撑数据，不超过 100 字
- 16:9 横屏：左栏标题+3-5 bullet，右栏 1 个大元素，不超过 150 字

### 文章类型识别

（详见 Step 0 定义，此处不重复。）

### 内容再格式化（提取后，布局前）

提取出来的内容不能直接塞进布局。先按目标布局再格式化：

| 原文形式 | 目标形式 | 再格式化操作 |
|---------|---------|------------|
| 一段话 → | 3 条 bullet | 拆句 + 提炼论断 |
| 列表 → | Dashboard cards | 每项加标题 + 简化描述 |
| 数字 → | KPI 大字 | 值 + 单位 + 变化方向 |
| 引用 → | Quote slide | 截最有力一句（≤25字） |
| 表格 → | Table slide | 选最关键 6-8 行 |
| 流程描述 → | Pipeline | 提取步骤名 + 连接关系 |

### 内容金字塔（跨格式复用）

```
page (100% 全文)
  ↓ 提取 ~70%
slides (要点+论据, 10-20 页)
  ↓ 选 top 3 最有冲击力的
poster (精华, 1 张)
```

如果用户已经做了 slides，再做 poster 时直接从 slides 的 Extraction Plan 选 top 3 slide，不从原文重新提取。

### 提取后：内容决定布局

| 提取出来的内容 | slides/deck 用什么布局 |
|-------------|---------------------|
| 3-5 条论断+细节 | Content (bullets) |
| 4-6 个并列概念 | Dashboard (cards) |
| 1 个核心大数字 | Hero Stat (居中大字) |
| A vs B 对比 | Split (双栏) |
| 改造前后/效果对比 | Before-After (灰+绿面板) |
| 3 个方案/选项 | Three-Column (三栏对比) |
| 数据表格 4+ 行 | Table |
| 时间线/分阶段 | Timeline (水平轴) |
| 二维分类 | 2x2 Matrix (四象限) |
| 流程/架构 | Diagram / Pipeline |
| 有力引用 | Quote |
| 3 个分级 | Content (tier cards) |
| 大图+说明 | Split 或 Full-Bleed |

**空白检测**：如果一页提取的内容不到 5 行纯文本 → 内容太少，回原文补或合并到相邻页。

**内容关系判断**（决定纵排 vs 横排）：
- **并列等权**（5 个独立问题点、5 个子系统，无先后无优先级）→ Dashboard(cards) 或 icon grid **横排**
- **递进有序**（第 1 步到第 5 步、优先级排序）→ Content(numbered list) **纵排**
- **分层分级**（L1/L2/L3 难度递增）→ Three-Column 或 tier cards
编号列表暗示"从上到下越来越不重要"，并列内容用编号会读成流水账。

---

## Format 1: page — 响应式滚动文章

画布：无固定尺寸，响应式。max-width 720px 正文区。

### 整体结构

```
┌─ browser viewport (any size) ─────────────────────┐
│ progress-bar: fixed top, 3px, accent color        │
│ util-menu: fixed top-right (theme toggle + print) │
│                                                    │
│ ┌─ hero ─────────────────────────────────────┐    │
│ │ padding: clamp(4rem,10vw,7rem) 24px        │    │
│ │ max-width: 720px, margin: 0 auto           │    │
│ │                                             │    │
│ │ [tags row]  9px pills                       │    │
│ │ [title]  clamp(1.75rem,5vw,3rem) / 800     │    │
│ │ [subtitle]  clamp(1rem,2vw,1.1875rem) dim  │    │
│ │ [meta]  small-size, dim                    │    │
│ └─────────────────────────────────────────────┘    │
│                                                    │
│ ┌─ page-wrapper (flex @1200px+) ─────────────┐    │
│ │ ┌─ toc ──────┐ ┌─ article ──────────────┐  │    │
│ │ │ sticky     │ │ max-width: 720px       │  │    │
│ │ │ top: 80px  │ │ padding: 0 24px        │  │    │
│ │ │ width:220px│ │                         │  │    │
│ │ │ font: 13px │ │ [sections...]          │  │    │
│ │ │            │ │                         │  │    │
│ │ └────────────┘ └─────────────────────────┘  │    │
│ └─────────────────────────────────────────────┘    │
│                                                    │
│ ┌─ footer ───────────────────────────────────┐    │
│ │ max-width: 720px, border-top, centered     │    │
│ └─────────────────────────────────────────────┘    │
│                                                    │
│ back-to-top: fixed bottom-right, >300px show      │
│ lightbox: fixed overlay for image zoom             │
└────────────────────────────────────────────────────┘
```

### Typography (clamp)

| 元素 | 字号 | 字重 | 行高 |
|------|------|------|------|
| Title | clamp(1.75rem, 5vw, 3rem) | 800 | 1.2 |
| h2 | clamp(1.375rem, 3vw, 1.75rem) | 800 | 1.3 |
| h3 | clamp(1.1875rem, 2.5vw, 1.375rem) | 700 | 1.4 |
| Body p | clamp(0.9375rem, 1.5vw, 1.0625rem) | 400 | 1.8 |
| Small | clamp(0.8125rem, 1.2vw, 0.9375rem) | 400 | 1.5 |
| Code | clamp(0.8125rem, 1.2vw, 0.875rem) | 400 | 1.6 |

### Content-Type Rendering

| 内容 | HTML | CSS 关键值 |
|------|------|-----------|
| 段落 | `<p>` | margin-bottom: 1.5em |
| 代码块 | `<pre><code>` + copy btn | padding: 20px, border-radius: 8px, inset shadow |
| 表格 | `<table>` in `.table-scroll` | th padding: 10px 14px, td padding: 10px 14px |
| 引用 | `<blockquote>` | border-left: 4px accent, padding: 12px 20px, border-radius: 0 8px 8px 0 |
| 图片 | `<figure>` + `<figcaption>` | border-radius: 8px, cursor: zoom-in |
| 列表 | `<ul>/<ol>` | padding-left: 1.5em, li margin-bottom: 0.5em |
| h2 分割 | border-bottom | padding-bottom: 0.3em, margin-top: 2.5em |

### Interactivity (必须全有)

1. Progress bar — scroll 百分比
2. TOC highlight — IntersectionObserver, rootMargin '-20% 0px -60% 0px'
3. Back to top — >300px 显示
4. Dark/Light toggle — localStorage
5. Image lightbox — click zoom + Escape close
6. Code copy button

### OG Tags (必须)

```html
<meta property="og:title" content="{title}">
<meta property="og:description" content="{一句话摘要}">
<meta property="og:type" content="article">
```

---

## Format 2: slides — 单篇文章提炼演示

画布：**1280x720px** 固定 (16:9)。

### 画布结构

```css
body { background: #1a1a2e; display: flex; justify-content: center; align-items: center; min-height: 100vh; overflow: hidden; }
.slide { width: 1280px; height: 720px; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); box-shadow: 0 8px 32px rgba(0,0,0,.25); border-radius: 6px; overflow: hidden; }
.slide.active { display: flex; }
```

### 内部结构

每张 slide 用 slide-types.md 定义的 15 种 type 之一。全局 5 层结构：
- hdr: 16px 40px 0 (~40px)
- sub: 4px 40px 0 (~20px)
- body: 12px 40px 0 (flex:1, ~628px)
- ft: 8px 40px 12px (~32px)

### 导航

```html
<div id="nav">
  <button id="prev">&larr;</button>
  <span id="counter">01 / 14</span>
  <button id="next">&rarr;</button>
</div>
```

固定 bottom: 16px, 居中。键盘：方向键/空格/Home/End。触摸：swipe >50px。

### 典型页数

| 文章长度 | slides 数量 |
|---------|------------|
| <2000 字 | 6-8 页 |
| 2000-5000 字 | 10-14 页 |
| >5000 字 | 14-20 页 |

第一页必须是 Title type，最后一页必须是 Title type (Thanks)。

---

## Format 3: deck — 多源叙事演示

画布：同 slides，**1280x720px**。

与 slides 的区别：

| | slides | deck |
|---|--------|------|
| 来源 | 单篇文章 | 多篇文章/碎片 |
| 内容 | 提炼原文要点 | 策展+重组 |
| 叙事 | 按原文结构 | 自定义弧线 |
| 标注 | 不需要 | 引用必须标注来源 `—— 《文章标题》` |

### 叙事弧线模板

```
Opening (1页)  → 背景 (1-2页) → 过程 (N页, 主体) → 结果 (1-2页) → 收获 (1页) → Closing (1页)
```

每页推进一步。不罗列观点。

---

## Format 4: poster-9x16 — 竖屏海报/小红书

画布：**1080x1920px** 固定 (9:16)。

### 结构

```
┌───────────── 1080px ──────────────┐
│ .poster-hdr: padding 44px 48px 0  │  ~80px
│   [logo/brand]  [tag pills]       │
│                                    │
│ .poster-title: padding 20px 48px  │  ~120px
│   Title 40-48px / 800             │
│   Subtitle 16px / dim             │
│                                    │
│ .poster-body: padding 16px 48px   │  ~1500px
│   content cards / bullet lists    │
│   / key stats / quotes            │
│   justify-content: space-between  │
│                                    │
│ .poster-ft: padding 16px 48px 36px│  ~80px
│   [source] [QR code] [brand]      │
└────────────────────────────────────┘
  Total: 1920px
```

### Typography

| 元素 | 字号 | 字重 |
|------|------|------|
| Title | 40-48px | 800 |
| Section heading | 20-24px | 700 |
| Body | 16-18px | 400 |
| Label | 12-14px | 600 |
| Footer | 12px | 400 |

### 内容限制

- 最多 5 个 section
- 每 section 最多 4 bullet
- 总文字不超过 300 字
- 1-2 个 highlight stat/quote

### CSS

```css
body { background: #1a1a2e; display: flex; justify-content: center; align-items: center; min-height: 100vh; }
.poster { width: 1080px; height: 1920px; overflow: hidden; box-shadow: 0 8px 32px rgba(0,0,0,.25); border-radius: 8px; display: flex; flex-direction: column; }
```

---

## Format 5: poster-1x1 — 方形社交卡片

画布：**1080x1080px** 固定 (1:1)。

### 结构

```
┌───────────── 1080px ──────────────┐
│ .poster-hdr: padding 36px 48px 0  │  ~60px
│                                    │
│ .poster-title: padding 16px 48px  │  ~100px
│   Title 32-36px / 800             │
│                                    │
│ .poster-body: padding 12px 48px   │  ~820px
│   1 key point or quote            │
│   + supporting data / visual      │
│                                    │
│ .poster-ft: padding 12px 48px 28px│  ~60px
└────────────────────────────────────┘
  Total: 1080px height
```

### 内容限制

- 1 个核心观点或引用
- 最多 3 个支撑数据
- 总文字不超过 100 字
- 大字号为主，留白多

---

## Format 6: poster-16x9 — 横屏分享卡

画布：**1920x1080px** 固定 (16:9)。

### 结构

```
┌──────────────────── 1920px ────────────────────┐
│ .poster-hdr: padding 36px 60px 0               │  ~60px
│                                                  │
│ .poster-body: padding 16px 60px                 │  ~920px
│   左右分栏 grid 1fr 1fr, gap 40px               │
│   左: title + bullets/stats                     │
│   右: image / chart / large quote               │
│                                                  │
│ .poster-ft: padding 12px 60px 24px              │  ~50px
└──────────────────────────────────────────────────┘
  Total: 1080px height
```

### Typography

| 元素 | 字号 | 字重 |
|------|------|------|
| Title | 36-44px | 800 |
| Body | 18-20px | 400 |
| Label | 12-14px | 600 |
| Stat value | 48-64px | 800 |

### 内容限制

- 左栏：标题 + 3-5 bullet 或 2-3 stat cards
- 右栏：1 个大元素（图片/图表/引用）
- 总文字不超过 150 字

---

## 格式选择指南

| 场景 | 推荐格式 |
|------|---------|
| 文章分享到网站/链接 | page |
| 会议/分享/微信群演示 | slides |
| 多篇文章经验总结分享 | deck |
| 小红书/朋友圈/手机端 | poster-9x16 |
| 微信群/即刻/社交卡片 | poster-1x1 |
| 公众号头图/横屏分享 | poster-16x9 |

---

## 通用规则（所有格式）

1. **背景不能纯平色** — 必须有氛围（径向发光/点阵/条纹/渐变网），Swiss Clean 例外
2. **深度层级** — 至少用 2 级（Hero + Default 或 Default + Recessed）
3. **字体** — 禁止 Inter/Roboto/Arial 单独做 display，必须从 STYLE_PRESETS.md 选
4. **图片** — 相对路径 `../images/xxx.png`
5. **颜色** — 全用 CSS 变量，换主题只改 `:root`
6. **side padding** — slides/deck 40px，poster 48-60px，page 24px
7. **print** — 隐藏导航/进度条
8. **prefers-reduced-motion** — 所有动画 0.01ms
