# Slide Types — Pixel-Perfect Layout Spec

Canvas: **1280x720px** fixed. All measurements in px, no approximation.

## Global: 5-Layer Structure

Every slide (except Title/Divider) follows this exact structure:

```
┌─────────────────────────────────────────────────┐
│ .hdr: padding 16px 40px 0              .page    │  40px
│ .sub: margin 4px 40px 0                         │  20px
│                                                  │
│ .body: padding 12px 40px 0                      │  ~628px
│   content fills via justify-content              │
│                                                  │
│ .ft: padding 8px 40px 12px                      │  32px
└─────────────────────────────────────────────────┘
  Total height: 720px (hdr 40 + sub 20 + body ~628 + ft 32 = 720 - 40 - 20 - 32 = 628)
```

### Spacing Rules (MANDATORY) — 4px 基线网格

**所有间距必须是 4 的倍数**：4/8/12/16/20/24/28/32/36/40px。禁止 6/10/14/18 等非 4 倍数值（from Slidev 4px grid system）。

```css
.slide .hdr       { padding: 16px 40px 0; display: flex; align-items: center; justify-content: space-between; flex-shrink: 0; }
.slide .sub       { margin: 4px 40px 0; flex-shrink: 0; }
.slide .body      { padding: 12px 40px 0; flex: 1; display: flex; flex-direction: column; min-height: 0; }
.slide .ft        { padding: 8px 40px 12px; flex-shrink: 0; }
```

### Body Height: 显式计算（from data-slides，最关键的规则）

**不靠 flex:1 猜高度**。每张 slide 精确计算 body 可用高度：

```
total = 720px
hdr = 40px (16px padding + ~24px content)
sub = 20px (4px margin + ~16px content)
ft  = 32px (8px + 12px padding + ~12px text)
───────────────
body_available = 720 - 40 - 20 - 32 = 628px
body_padding_top = 12px
body_usable = 628 - 12 = 616px

→ Set .body { height: 628px } 或用 flex:1（但必须验证不留白）
```

### Body Fill Strategy（v3.6 修正：容器不撑满，body 负责居中）

**核心原则**：容器（card/panel/grid）按内容自然高度渲染，body 用 `justify-content: center` 把容器居中。空白在 body 上下（呼吸空间），不在容器内部（空洞感）。

**禁止**：grid/card 容器 `height: 100%` 强撑满 body。这会导致内容少的卡片 60% 是白色空洞。

| 场景 | Strategy | 容器高度 | 原因 |
|------|----------|---------|------|
| **bullets/numbered items** | body `space-evenly` | 无容器，items 直接在 body | items 均匀撑满 body |
| **cards grid（Dashboard/Three-Column）** | body `center` | grid 自然高度，**不设 height:100%** | 卡片紧凑，grid 整体居中 |
| **双面板（Before-After/Split）** | body `center` | 面板自然高度 + padding 36px | 面板内 flex-start，不要 space-between 撕开内容 |
| **Pipeline 步骤** | body `center` | 步骤卡高度 ~120-160px，水平条带居中 | 流水线是矮宽横条，不是高瘦竖卡 |
| **单个大元素（图/表/Quote）** | body `center + align-items: center` | 元素自然尺寸 | 视觉居中 |
| **密集内容（6+ bullets / 大表格）** | body `flex-start` | 内容自然撑满 >85% | 内容够多就不需要居中 |

**body padding-top 最大 12px** — 紧凑是核心，不要给 body 太多顶部留白。

### Breathing Enlargement（呼吸放大，v3.9 — from Beautiful.ai + Gamma 反面教训）

**当内容量不足以填满 body 时，放大间距和收窄内容区，而不是留空白。**

#### 全局字号一致性铁律（from Gamma 反面教材）

Gamma 的 per-card 独立字号缩放导致同一 H1 在不同 slide 上大小不同，是其最大败笔。

**我们的规则**：同一 deck 内，相同层级的字号**跨页一致**。Breathing 只调间距/padding/margin，**不调字号**。

#### 密度系数公式（from Gamma 三元公式简化）

预估 body 填充率（内容自然高度 / 616px），按三档取密度系数 `D`：

| 填充率 | 档位 | 密度系数 D |
|--------|------|-----------|
| **>70%** | 饱满 | 1.0 |
| **40-70%** | 中等 | 1.25 |
| **<40%** | 稀疏 | 1.6 |

**所有间距统一乘 D**：

```
卡片 padding   = 24px × D    → 饱满 24px / 中等 28px / 稀疏 36px（取 4px 网格近似值）
grid gap       = 原值 × D    → 如 10px → 12px → 16px
side margin    = 40px × D    → 饱满 40px / 中等 48px / 稀疏 64px
body padding   = 12px × D    → 饱满 12px / 中等 16px / 稀疏 20px
```

4px 网格近似：乘完后 round 到最近的 4 的倍数。

**字号不变**：Display 48px / Title 36px / Heading 22px / Body 18px / Small 15px / Label 11px 全 deck 统一。

#### 填充率预估方法（生成前心算）

```
每条 bullet (heading+detail) ≈ 74px
每张 card (kpi-val+label+padding) ≈ 120px
每个 timeline node ≈ 200px（水平方向，不算高度）
表格每行 ≈ 36px
Quote 块 ≈ 280px
→ 总高度 / 616 = 填充率
```

#### 规则

- Pipeline/Timeline 等水平布局按宽度方向判断，不按高度
- 密度系数影响范围：padding / gap / margin。不影响：字号 / line-height / 卡片 min-height
- 同一 deck 内**所有页使用相同密度系数 D**（取全 deck 平均填充率对应的档位），不 per-slide 独立调
- 例外：如果某页 >85% 填充，该页回落到 D=1.0（防止饱满页被间距撑爆）

---

## Global: Typography Scale (6 levels) — PRESENTATION SCALE

**关键设计决策**：slides 是 presentation 媒介，不是 web 阅读。字号必须是 web 的 ~1.7x。
1280x720 画布上 13px 正文只能填满 30% body — 这是之前所有空白问题的根因。

参照真实 PPT 标准（Google Slides/Keynote 默认）：

| Level | Font size | Weight | Line-height | Use for | 对比 web 版 |
|-------|-----------|--------|-------------|---------|-----------|
| **Display** | 48px | 800 | 1.15 | Title slide heading | web 30px |
| **Title** | 36px | 800 | 1.2 | Slide heading (h2 in .hdr) | web 22px |
| **Heading** | 22px | 700 | 1.4 | Card h3, .ni strong | web 14px |
| **Body** | 18px | 400 | 1.8 | List items, paragraphs | web 13px |
| **Small** | 15px | 400 | 1.6 | .ni detail, card desc, tier li | web 11px |
| **Label** | 11px | 600 | 1.3 | Section label, page number, tags, ft | web 9px |

```css
.slide .hdr h2    { font-size: 36px; font-weight: 800; line-height: 1.2; }
.slide .hdr .page { font-size: 12px; font-weight: 600; letter-spacing: 1px; }
.slide .sub       { font-size: 11px; font-weight: 600; text-transform: uppercase; letter-spacing: 2px; }
.slide .body li   { font-size: 18px; line-height: 1.8; margin-bottom: 10px; }
.slide .body p    { font-size: 18px; line-height: 1.8; }
.slide .ft        { font-size: 11px; letter-spacing: .3px; text-align: center; }
```

### 填充验证（新字号下）

| Slide 类型 | 元素 | 每元素高度 | 数量 | 总高度 | 占 body % | 策略 |
|-----------|------|----------|------|--------|----------|------|
| Content 5 bullets | heading 22px + body 18px×1.8 + margin 10px | ~74px | 5 | ~370px | 60% | space-evenly 撑满 |
| Dashboard 6 cards | kpi-val 48px + label 11px + padding 48px | ~120px | 6(2行) | ~250px | 40% | center（grid 居中） |
| Quote | bq 28px×1.5×3lines + attr 15px + before 100px | - | 1 | ~280px | 45% | center（天然居中） |
| Table 6 rows | th 15px+pad 24px + td(16px+pad 20px)×6 | ~36px/row | 7 | ~252px | 40% | center |
| Split 2 cols | heading 22px + 4 bullets(18px×1.8+10px) | ~220px | 2列 | ~220px | 35% | center，需饱满内容 |
| Before-After | stat 36px + label 11px + 4 bullets | ~250px | 2面板 | ~500px | 80% | space-between 面板内 |
| Three-Column | heading 22px + 4 bullets + stat 36px | ~300px | 3列 | ~300px | 48% | body center（grid 自然高度） |
| Pipeline 7 steps | num 28px + title 20px + count 15px + pad | ~200px | 7 横 | - | stretch | align-items: stretch |

**关键教训**：填充率 <70% 的类型，空白会聚集在底部，必须用 `space-evenly/space-between` 分散。`flex-start` 只适合内容自然撑满 >85% 的场景（密集表格、代码块）。

---

## Global: Component Specs

### Card Sizing 原则（from Gamma + Beautiful.ai 调研 v3.8）

**卡片用 min-height + padding 控制，不用 aspect-ratio 也不用固定 height**。aspect-ratio 在内容不足时会创造空洞（和 height:100% 同样的问题）。卡片高度 = 内容高度 + padding，由内容决定。

```css
/* Dashboard KPI 卡片：宽矮横卡 */
.card-kpi { min-height: 80px; padding: 24px 20px; }
/* 通用信息卡 */
.card-info { min-height: 100px; padding: 24px 20px; }
/* 窄高列卡（Three-Column） */
.card-col { min-height: 120px; padding: 28px 20px; }
```

**禁止**：`height: 100%`、`height: 300px`、`aspect-ratio`（文本卡片上）。
**aspect-ratio 唯一合法场景**：图片容器（配合 `object-fit: cover`）。
**Pipeline 步骤卡**：`height: 120-160px`（内容恒定，不需要响应式）。

### Card

```css
.card {
  background: var(--sf);
  border: 1px solid var(--bdr);
  border-radius: 10px;
  padding: 24px 20px;           /* PRESENTATION scale padding */
}
.card h3 { font-size: 22px; font-weight: 700; margin-bottom: 6px; }
.card p  { font-size: 15px; color: var(--dim); margin: 0; line-height: 1.6; }
.card-accent { border-top: 3px solid var(--accent); }
```

### Tier Card (colored left border)

```css
.tier {
  background: var(--sf);
  border-radius: 10px;
  padding: 20px 24px;
  margin-bottom: 12px;
  border-left: 4px solid var(--accent);
}
.tier .tl { font-size: 11px; font-weight: 800; letter-spacing: 1px; margin-bottom: 8px; }
.tier li  { font-size: 15px; line-height: 1.6; margin-bottom: 5px; }
```

### Table

```css
table { width: 100%; border-collapse: collapse; }
th { font-size: 15px; font-weight: 700; padding: 12px 18px; border-bottom: 3px solid var(--accent); }
td { font-size: 16px; padding: 10px 18px; border-bottom: 1px solid var(--bdr); }
td.hl { font-weight: 800; }  /* accent color from preset */
```

### Numbered Item

```css
.ni { display: flex; gap: 16px; margin-bottom: 16px; }
.ni .n { font-size: 28px; font-weight: 800; min-width: 36px; opacity: 0.3; line-height: 1.1; }
.ni .t strong { font-size: 22px; font-weight: 700; }
.ni .t p { font-size: 15px; margin: 4px 0 0; line-height: 1.6; }
```

### Insight Bar

```css
.bar { padding: 18px 24px; border-radius: 10px; margin-bottom: 10px; font-size: 18px; line-height: 1.6; }
.bar.on  { font-weight: 700; }  /* accent bg + border from preset */
.bar.off { opacity: 0.35; }
```

### Stats Bar

```css
.stats { display: flex; gap: 14px; font-size: 12px; flex-wrap: wrap; padding: 14px 0 0; border-top: 1px solid var(--bdr); margin-top: 16px; }
```

### Pills

```css
.pills { display: flex; gap: 8px; margin-top: 14px; }
.pill { font-size: 12px; padding: 6px 14px; border-radius: 16px; }
```

### Quote

```css
.bq { font-size: 28px; font-weight: 500; line-height: 1.5; text-align: center; max-width: 900px; }
.bq::before { content: '\201C'; font-size: 100px; opacity: 0.1; display: block; line-height: 0.3; margin-bottom: 16px; }
.attr { font-size: 15px; margin-top: 20px; text-align: center; line-height: 1.6; }
```

---

## Content Density Limits (HARD CAPS)

**全局上限**：单张 slide 总视觉元素 ≤6 个（heading/card/bullet/stat/diagram-node 各算 1 个）。超过 6 个认知负荷激增（研究数据：500%），必须拆页。

| Type | Max Content | Exceeds → |
|------|------------|-----------|
| Title | 1 heading + 1 subtitle + tags | N/A |
| Divider | 1 large number + 1 heading | N/A |
| Content | 1 heading + 5-6 bullets (each max 2 lines) | Split page |
| Split | Left: heading + 3-4 points / Right: image or card | Split page |
| Dashboard | 1 heading + max 6 KPI cards | Split page |
| Table | 1 heading + max 8 rows x 4 cols | Split page |
| Code | 1 heading + max 10 lines | Split page |
| Quote | 1 quote (~25 words) + 1 attribution | Shorten |
| Diagram | 1 heading + max 10 nodes | Split to overview + detail |
| Full-Bleed | background image + heading + 1-2 lines | N/A |
| Hero Stat | 1 number (80-120px) + 1 context sentence + optional delta | N/A |
| Timeline | max 8 nodes, each: year + title + 1-2 line desc | Split page at 9+ |
| Before-After | 2 panels, each: label + stat + 3-4 bullets | N/A |
| 2x2 Matrix | 4 quadrants, max 4 items per quadrant | Simplify items |
| Three-Column | 3 columns, each: icon + heading + 3-4 bullets + optional stat | Split page |

### Sparse Content Degradation Matrix（稀疏降级，from Tome/Beautiful.ai，v3.8）

**当选定布局的预期填充率 <40% 时，不硬撑——降级为更紧凑的布局。**

| 原布局 | 稀疏信号 | 降级为 | 原因 |
|--------|---------|--------|------|
| Dashboard 3 列 card grid | 每卡仅 1 个数字+标签 | **横向 Stat Bar**（数字+标签内联，一条水平带） | 3 个小卡片在 1200px 里太散 |
| Dashboard 6 卡 2x3 grid | 每卡仅标题+1 行描述 | **缩小卡片 + Breathing 中等档** | 内容不够撑满 6 个方卡 |
| Three-Column 3 列 | 每列仅 heading+1 行 | **横向 pill 标签条**或 **Content tier cards** | 3 列竖卡 90% 是空白 |
| Split 双栏 | 双方各仅 2-3 行 | **Before-After 大数字模式** | 用大数字占空间 |
| 2x2 Matrix | 每象限仅 1 项 | **Content 4 条 bullet** | 4 个大象限放 4 条文字太浪费 |
| Content 3 条 bullet | 每条仅 1 行无 detail | **Hero Stat**（如果有数字）或 **Quote**（如果有引用） | 3 条短句填不满 body |

**降级判断时机**：在 Extraction Plan Table 阶段预估。如果预估填充率 <40%，先尝试 R1 呼吸放大；放大后仍 <40% 则执行降级。

---

## Type 1: Title

```
┌──────────────────────────────────────────────────┐
│                                                   │
│              [prompt motif]  11px                 │
│              [tags row]  11px pills               │
│                                                   │
│    Title 48px weight 800, centered, max 1000px    │
│                                                   │
│         Subtitle 15px, dim color                  │
│         Meta line 11px, dimmest                   │
│                                                   │
└──────────────────────────────────────────────────┘
```

Layout: `justify-content: center; align-items: center; text-align: center; padding: 36px;`
NO hdr/sub/body/ft structure — custom centered layout.

```html
<div class="slide title-slide active">
  <div class="prompt">cat file.deck<span class="cursor"></span></div>
  <div class="tags"><span>Tag1</span><span>Tag2</span></div>
  <h1>Slide Title</h1>
  <p class="subtitle">Subtitle text</p>
  <p class="meta">Date · Source · N slides</p>
</div>
```

```css
.title-slide { justify-content: center; align-items: center; text-align: center; padding: 36px; }
.title-slide h1 { font-size: 48px; font-weight: 800; line-height: 1.15; max-width: 1000px; margin-bottom: 12px; }
.title-slide .subtitle { font-size: 15px; }
.title-slide .meta { font-size: 11px; margin-top: 16px; opacity: 0.4; }
.tags { display: flex; gap: 6px; justify-content: center; margin-bottom: 14px; }
.tags span { font-size: 11px; padding: 2px 8px; border-radius: 3px; }
```

---

## Type 2: Section Divider

Same centered layout as Title, but with large decorative number:

```html
<div class="slide title-slide">
  <div class="section-num">02</div>
  <div class="sub" style="justify-content: center;">// SECTION NAME</div>
  <h1 style="font-size: 36px;">Section Title</h1>
</div>
```

```css
.section-num { font-size: 160px; font-weight: 200; opacity: 0.04; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -60%); }
```

---

## Type 3: Content (Bullet List)

```
┌─ hdr ──────────────────────────────── page ─┐  16px pad
│ Title 36px/800                          03   │
├─ sub ────────────────────────────────────────┤  4px margin
│ ● // SECTION LABEL  11px                     │
├─ body ───────────────────────────────────────┤  12px pad
│ • Bullet 1  18px/400, margin-bottom 10px     │
│ • Bullet 2                                   │
│ • Bullet 3                                   │
│ • Bullet 4                                   │
│ • Bullet 5 (max)                             │
│                                              │
├─ ft ─────────────────────────────────────────┤  8px pad
│ Source · Date                     11px       │
└──────────────────────────────────────────────┘
```

Body strategy: `justify-content: space-evenly` — items 均匀撑满 616px body usable，禁止 flex-start（会导致底部 40% 空白）。

**Anti-bullet-list 规则**（Tufte/Reynolds/Alley 三人共识）：
禁止生成纯 bullet list（光秃秃一行文字+圆点）。每条 bullet 必须是 **heading+detail 二行模式**：
- heading（22px Bold）= 核心论断，独立可理解
- detail（15px Small）= 支撑说明，1 行
如果内容只有光秃秃的短句列表，优先转为 Dashboard(cards) 或 tier cards，不要用 Content 硬凑。

**交互：Click-to-Reveal（v4.1）**：item 有 `expand` 字段时，点击展开额外文本。heading+detail 始终可见（assertion 不隐藏），expand 是补充 evidence。export 模式自动全部展开。

```html
<div class="slide">
  <div class="hdr"><h2>Title</h2><span class="page">03</span></div>
  <div class="sub">// LABEL</div>
  <div class="body">
    <ul>
      <li>Point 1</li>
      <li>Point 2 with <strong class="accent">highlight</strong></li>
    </ul>
  </div>
  <div class="ft">Source · Date</div>
</div>
```

---

## Type 4: Split (Two-Column)

```
┌─ hdr ──────────────────────────────── page ─┐
│ Title                                   04   │
├─ sub ────────────────────────────────────────┤
│ ● // LABEL                                  │
├─ body (grid 1fr 1fr, gap 20px) ─────────────┤
│ ┌─ left ─────────┐  ┌─ right ────────────┐  │
│ │ card or text    │  │ card, image,       │  │
│ │                 │  │ or diagram         │  │
│ └─────────────────┘  └────────────────────┘  │
├─ ft ─────────────────────────────────────────┤
│ Source · Date                                │
└──────────────────────────────────────────────┘
```

```css
.g1x2 { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
```

Body strategy: `justify-content: center` — vertically center the grid. Grid 自然高度，body center，列内内容少时加 padding 或 `align-content: center`。

---

## Type 5: Dashboard (KPI Cards)

```
┌─ hdr ──────────────────────────────── page ─┐
│ Title                                   05   │
├─ sub ────────────────────────────────────────┤
│ ● // LABEL                                  │
├─ body (grid 3col or 2col, gap 10px) ────────┤
│ ┌─ card ──┐ ┌─ card ──┐ ┌─ card ──┐         │
│ │ value   │ │ value   │ │ value   │         │
│ │ 48px/800│ │ 48px/800│ │ 48px/800│         │
│ │ label   │ │ label   │ │ label   │         │
│ │ 11px    │ │ 11px    │ │ 11px    │         │
│ └─────────┘ └─────────┘ └─────────┘         │
│ ┌─ card ──┐ ┌─ card ──┐ ┌─ card ──┐         │
│ └─────────┘ └─────────┘ └─────────┘         │
├─ ft ─────────────────────────────────────────┤
└──────────────────────────────────────────────┘
```

```css
.g2x3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; }
.card-accent { border-top: 2px solid var(--accent); }
.kpi-val { font-size: 48px; font-weight: 800; }
.kpi-label { font-size: 11px; margin-top: 4px; }
```

Body strategy: `justify-content: center`.
Max 6 cards. KPI value max 6 characters.

**交互：Number Counter Animation（v4.1）**：KPI 数字在 slide 出现时从 0 滚动到目标值（0.8s ease-out）。自动检测数字格式，非数字值跳过。`animate: false` 显式禁用。Hero Stat 同理。

---

## Type 6: Table

```
┌─ hdr ──────────────────────────────── page ─┐
│ Title                                   06   │
├─ sub ────────────────────────────────────────┤
│ ● // LABEL                                  │
├─ body ───────────────────────────────────────┤
│ ┌─ th ──────┬──────────┬──────────┐          │
│ │ Col1 15px │ Col2     │ Col3     │ 2px accent border-bottom
│ ├───────────┼──────────┼──────────┤          │
│ │ td 16px   │          │ hl       │ 1px border-bottom
│ │           │          │          │          │
│ │ (max 8 rows)                    │          │
│ └───────────┴──────────┴──────────┘          │
├─ ft ─────────────────────────────────────────┤
└──────────────────────────────────────────────┘
```

Body strategy: `justify-content: center`.
Max 8 rows, max 4 columns. `td.hl` for accent-highlighted values.

**Table + Callout 变体**（from 咨询公司风格）：
表格数据量大时（6+ 行），在右侧 20% 区域加 Callout Box 提炼 1-2 个核心结论。读者先看 Callout 得到结论，再看表格验证。
```css
.table-with-callout { display: grid; grid-template-columns: 75% 25%; gap: 16px; }
.callout-box { background: var(--accent-dim); border-left: 3px solid var(--accent); border-radius: 8px; padding: 20px 16px; font-size: 16px; font-weight: 700; }
```

---

## Type 7: Code

```
┌─ hdr ──────────────────────────────── page ─┐
│ Title                                   07   │
├─ sub ────────────────────────────────────────┤
├─ body (align-items: center) ────────────────┤
│         ┌─ .code-block ────────────┐        │
│         │ .filename   config.py    │ 11px   │
│         │                          │        │
│         │   code line 1  14px mono │        │
│         │   code line 2            │        │
│         │   (max 10 lines)         │        │
│         │                          │        │
│         └──────────────────────────┘        │
├─ ft ─────────────────────────────────────────┤
└──────────────────────────────────────────────┘
```

```css
.code-block { max-width: 90%; border-radius: 10px; padding: 20px 24px; position: relative; }
.code-block .filename { position: absolute; top: 8px; right: 12px; font-size: 11px; opacity: 0.4; }
.code-block pre { font-size: 14px; line-height: 1.5; }
```

Body strategy: `justify-content: center; align-items: center`.

---

## Type 8: Quote

```
┌──────────────────────────────────────────────┐
│                                              │
│                                              │
│               "  (100px, accent, 0.1 opacity)│
│                                              │
│     Quote text 28px italic, centered         │
│     max ~25 words, max-width 900px           │
│                                              │
│     — Attribution  15px dim                  │
│                                              │
│                                              │
└──────────────────────────────────────────────┘
```

Uses title-slide layout (full centered, no 5-layer).

```css
.bq { font-size: 28px; font-weight: 500; line-height: 1.5; text-align: center; max-width: 900px; }
.bq::before { content: '\201C'; font-size: 100px; opacity: 0.1; display: block; line-height: 0.3; margin-bottom: 16px; }
.attr { font-size: 15px; margin-top: 20px; text-align: center; line-height: 1.6; }
```

---

## Type 9: Diagram

Two sub-types based on complexity:

### CSS Pipeline (<7 nodes, no branching)

```css
.pipeline { display: flex; gap: 6px; align-items: stretch; }
.pipeline-step { flex: 1; border-top: 2px solid var(--accent); border-radius: 8px; padding: 14px 12px; }
.pipeline-step .step-num { font-size: 20px; font-weight: 800; opacity: 0.2; margin-bottom: 6px; }
.pipeline-step h3 { font-size: 15px; font-weight: 700; margin-bottom: 4px; }
.pipeline-step p { font-size: 12px; }
.pipeline-arrow { display: flex; align-items: center; font-size: 16px; flex-shrink: 0; }
```

Body strategy: `justify-content: center; align-items: stretch`。步骤卡高度 ~120-160px（矮宽水平条带），不要 height:100% 拉成高瘦竖卡。Pipeline 整体在 body 垂直居中。

### Mermaid (8-15 nodes)

See `mermaid-guide.md`. Must use zoom controls wrapper. Body strategy: `justify-content: center; align-items: center`.

---

## Type 10: Full-Bleed Image

```html
<div class="slide" style="padding: 0; position: relative;">
  <img src="../images/hero.png" style="width: 100%; height: 100%; object-fit: cover; position: absolute; inset: 0; opacity: 0.3;">
  <div style="position: absolute; bottom: 40px; left: 40px; right: 40px; z-index: 1;">
    <h2 style="font-size: 36px;">Title over image</h2>
    <p style="font-size: 18px;">Brief description</p>
  </div>
</div>
```

No 5-layer structure. Image covers entire 1280x720. Content at bottom with scrim.

---

## Type 11: Hero Stat (Single KPI)

When one **number** IS the message. Must be a specific, quantifiable metric — not a concept or metaphor.

**Use**: "42%" / "$1.2B" / "4100+" / "30 minutes"
**Don't use**: "复利" / "Less is More" / "AI Native" (these are Quote or Full-Bleed)

```
┌─ hdr ──────────────────────────────── page ─┐
│ Title                                   11   │
├─ sub ────────────────────────────────────────┤
│ ● // LABEL                                  │
├─ body (center both axes) ───────────────────┤
│                                              │
│          80-120px  bold  accent              │
│              42%                             │
│                                              │
│          Context 20px, dim                   │
│     "vs 23% last quarter"                    │
│                                              │
├─ ft ─────────────────────────────────────────┤
│ Source · Date                                │
└──────────────────────────────────────────────┘
```

```css
.hero-stat { justify-content: center; align-items: center; text-align: center; }
.hero-stat .big { font-size: 96px; font-weight: 800; line-height: 1.1; }
.hero-stat .ctx { font-size: 20px; margin-top: 24px; line-height: 1.6; max-width: 600px; }
.hero-stat .delta { font-size: 18px; margin-top: 12px; opacity: 0.6; }
```

Body strategy: `justify-content: center; align-items: center`.
Number fills ~40-50% of body height. Context sentence below.

---

## Type 12: Timeline (Horizontal)

4-8 events on a horizontal axis. Uses 5-layer structure.

```
┌─ hdr ──────────────────────────────── page ─┐
│ Title                                   12   │
├─ sub ────────────────────────────────────────┤
├─ body ───────────────────────────────────────┤
│                                              │
│  ●──────────●──────────●──────────●          │
│  2022       2023       2024       2025       │
│  Founded    Series A   Launch     IPO        │
│  desc 1     desc 2     desc 3     desc 4    │
│                                              │
├─ ft ─────────────────────────────────────────┤
└──────────────────────────────────────────────┘
```

```html
<div class="body">
  <div class="timeline">
    <div class="tl-line"></div>
    <div class="tl-node active">
      <div class="tl-dot"></div>
      <div class="tl-year">2022</div>
      <div class="tl-title">Founded</div>
      <div class="tl-desc">Description text</div>
    </div>
    <!-- repeat for each node -->
  </div>
</div>
```

```css
.timeline { position: relative; display: flex; justify-content: space-between; align-items: flex-start; padding: 80px 0 0; }
.tl-line { position: absolute; top: 88px; left: 0; right: 0; height: 3px; background: var(--bdr); }
.tl-node { flex: 1; text-align: center; position: relative; z-index: 1; }
.tl-dot { width: 16px; height: 16px; border-radius: 50%; background: var(--accent); margin: 0 auto 16px; }
.tl-node.active .tl-dot { width: 20px; height: 20px; box-shadow: 0 0 0 4px rgba(var(--accent-rgb), 0.2); }
.tl-year { font-size: 15px; font-weight: 700; margin-bottom: 4px; }
.tl-title { font-size: 18px; font-weight: 700; margin-bottom: 4px; }
.tl-desc { font-size: 15px; line-height: 1.5; max-width: 180px; margin: 0 auto; }
```

Body strategy: `justify-content: center`. Max 8 nodes. Active node uses accent color + glow.

---

## Type 13: Before-After (Dual Panel)

Color-coded comparison of two states. Uses 5-layer structure.

```
┌─ hdr ──────────────────────────────── page ─┐
│ Title                                   13   │
├─ sub ────────────────────────────────────────┤
├─ body (grid 1fr 1fr, gap 24px) ─────────────┤
│ ┌─ BEFORE (muted) ───┐ ┌─ AFTER (accent) ──┐│
│ │ label: BEFORE 11px  │ │ label: AFTER 11px ││
│ │                     │ │                    ││
│ │ stat: 2 days        │ │ stat: 30 min      ││
│ │ 36px bold           │ │ 36px bold accent   ││
│ │                     │ │                    ││
│ │ • detail 1 15px     │ │ • detail 1 15px   ││
│ │ • detail 2          │ │ • detail 2         ││
│ │ • detail 3          │ │ • detail 3         ││
│ └─────────────────────┘ └────────────────────┘│
├─ ft ─────────────────────────────────────────┤
└──────────────────────────────────────────────┘
```

```css
.ba-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 24px; }
.ba-panel { border-radius: 10px; padding: 32px 28px; display: flex; flex-direction: column; }
.ba-panel.before { background: var(--sf); border: 1px solid var(--bdr); }
.ba-panel.after { background: rgba(var(--accent-rgb), 0.08); border: 1px solid var(--accent); }
.ba-label { font-size: 11px; font-weight: 800; letter-spacing: 2px; text-transform: uppercase; margin-bottom: 16px; }
.ba-stat { font-size: 36px; font-weight: 800; margin-bottom: 16px; }
.ba-panel.after .ba-stat { color: var(--accent); }
.ba-panel li { font-size: 15px; line-height: 1.6; margin-bottom: 8px; }
```

Body strategy: `justify-content: center`。`.ba-grid` 不设 height:100%（按内容自然高度），body 居中。面板内部 `flex-start` + padding 36px 32px，不用 space-between（会撕开内容导致面板内大片空白）。

Two-dimensional categorization. Uses 5-layer structure.

```
┌─ hdr ──────────────────────────────── page ─┐
│ Title                                   14   │
├─ sub ────────────────────────────────────────┤
├─ body ───────────────────────────────────────┤
│         ← axis label X →                     │
│  ┌──────────────┬──────────────┐  ↑          │
│  │  Q1 heading  │  Q2 heading  │  axis       │
│  │  • item      │  • item      │  label      │
│  │  • item      │  • item      │  Y          │
│  ├──────────────┼──────────────┤  ↓          │
│  │  Q3 heading  │  Q4 heading  │             │
│  │  • item      │  • item      │             │
│  └──────────────┴──────────────┘             │
├─ ft ─────────────────────────────────────────┤
└──────────────────────────────────────────────┘
```

```css
.matrix-wrap { display: flex; flex-direction: column; align-items: center; justify-content: center; }
.matrix-axes { position: relative; width: 100%; }
.matrix-x-label { text-align: center; font-size: 11px; font-weight: 700; letter-spacing: 1px; margin-bottom: 8px; }
.matrix-y-label { position: absolute; left: -40px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 11px; font-weight: 700; letter-spacing: 1px; }
.matrix { display: grid; grid-template-columns: 1fr 1fr; grid-template-rows: 1fr 1fr; gap: 4px; }
.matrix-q { border-radius: 8px; padding: 20px; background: var(--sf); }
.matrix-q h4 { font-size: 15px; font-weight: 700; margin-bottom: 8px; }
.matrix-q li { font-size: 14px; line-height: 1.5; margin-bottom: 4px; }
.matrix-q.highlight { background: rgba(var(--accent-rgb), 0.1); border: 1px solid var(--accent); }
```

Max 4 items per quadrant. One quadrant can be highlighted with accent.

Body strategy: `justify-content: center`。`.matrix-wrap` 不设 height:100%，按内容自然高度居中。matrix grid 用固定行高（如 `grid-template-rows: 200px 200px`）控制象限大小，不靠 flex 撑满。

---

## Type 15: Three-Column Comparison

Three parallel options or concepts. Uses 5-layer structure.

```
┌─ hdr ──────────────────────────────── page ─┐
│ Title                                   15   │
├─ sub ────────────────────────────────────────┤
├─ body (grid 3 cols, gap 16px) ──────────────┤
│ ┌─ col 1 ─────┐ ┌─ col 2 ─────┐ ┌─ col 3 ─┐│
│ │ icon/emoji   │ │ icon/emoji   │ │ icon     ││
│ │ heading 22px │ │ heading 22px │ │ 22px     ││
│ │              │ │              │ │          ││
│ │ • point 1    │ │ • point 1    │ │ • pt 1   ││
│ │ • point 2    │ │ • point 2    │ │ • pt 2   ││
│ │ • point 3    │ │ • point 3    │ │ • pt 3   ││
│ │              │ │              │ │          ││
│ │ stat 36px    │ │ stat 36px    │ │ stat     ││
│ └──────────────┘ └──────────────┘ └──────────┘│
├─ ft ─────────────────────────────────────────┤
└──────────────────────────────────────────────┘
```

```css
.g1x3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
.col-card { border-radius: 10px; padding: 28px 20px; background: var(--sf); border: 1px solid var(--bdr); display: flex; flex-direction: column; }
.col-card.recommended { border-color: var(--accent); border-width: 2px; }
.col-card .col-icon { font-size: 32px; margin-bottom: 12px; }
.col-card h3 { font-size: 22px; font-weight: 700; margin-bottom: 12px; }
.col-card li { font-size: 15px; line-height: 1.6; margin-bottom: 6px; }
.col-card .col-stat { font-size: 36px; font-weight: 800; margin-top: auto; padding-top: 16px; border-top: 1px solid var(--bdr); }
```

One column can have `.recommended` class for accent border highlight.

Body strategy: `justify-content: center`。`.g1x3` 不设 height:100%（按内容自然高度），body 居中。`.col-card` 内部 `justify-content: flex-start`，底部 stat 用 `margin-top: auto` 固定底部。

---

## Grid Reference

| Grid | CSS | Use for |
|------|-----|---------|
| 2x3 | `repeat(3, 1fr); gap: 10px` | Dashboard 6 cards |
| 2x2 | `repeat(2, 1fr); gap: 10px` | Feature 4 cards, 2x2 Matrix |
| 1x2 | `1fr 1fr; gap: 20px` | Split, Before-After |
| 1x3 | `repeat(3, 1fr); gap: 16px` | Three-Column Comparison |

---

## Layout Debugging Checklist

Before delivery, check each slide:

- [ ] No content below .ft (nothing cut off at bottom)
- [ ] No large empty gaps in body (wrong justify-content)
- [ ] Card text not overflowing card boundaries
- [ ] Table fits within body (max 8 rows)
- [ ] Image not exceeding max-height (380px for normal, 100% for full-bleed)
- [ ] Font sizes match the 6-level scale (no arbitrary sizes)
- [ ] All slides use consistent side padding (40px)
- [ ] Page number visible in .hdr
- [ ] Section label present in .sub
- [ ] Footer present in .ft
