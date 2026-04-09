# Quality Gate — Three-Layer Defense

## Layer 1: Pre-Generation (before writing any HTML)

### State Your Approach (mandatory)
Before writing HTML, declare design intent in 1-2 sentences. Example:
"This is a technical deep-dive article, reader is a data engineer peer. Using Terminal Mono preset, scan lines for atmosphere, code blocks get inset shadow treatment, TOC on left for navigation."

If you can't state intent, you haven't thought enough.

### 7-Item Slop Test (hit 2+ = redo from scratch)
1. Inter or Roboto + purple/violet gradient
2. Every heading uses `background-clip: text` gradient
3. Every section prefixed with emoji icon (use SVG dot label instead)
4. Cards have animated glowing box-shadows
5. Cyan-magenta-pink color scheme + dark background
6. Completely uniform card grid, no visual hierarchy
7. Code blocks have decorative three-dot chrome (red/yellow/green circles)

### Font Blacklist
NEVER use as display font: Inter, Roboto, Arial, Helvetica, system-ui alone

### Color Blacklist
NEVER use: #6366f1 / #7c3aed / #a78bfa (Tailwind indigo/violet), #d946ef (fuchsia), purple gradients on white

## Layer 2: During Generation (structural rules)

### JSON→Engine 路径的质量保障

当使用 slide-engine.html 渲染引擎时：
- **Layer 1 仍需执行**（State Your Approach + Slop Test 在内容提取阶段做）
- **Layer 2 由引擎保障**（字号/间距/密度系数/卡片样式全部锁定，sub-agent 无法违反）
- **Layer 3 仍需执行**（提取质量测试 + Squint/Swap Test 在最终输出上做）
- 引擎内置 JSON 验证：type 合法性、items 数量、accent 唯一性，失败 console.warn + fallback

### Content Density Limits (slides/deck only)
See `slide-types.md` for per-type hard caps. Core rule: exceeds limit → split page, never compress, never scroll.

### Content-Type Routing (page mode)
| Content Type | Rendering |
|-------------|-----------|
| Paragraphs | `<p>` with line-height 1.8 |
| Code blocks | `<pre><code>` + CSS class coloring + copy button |
| Tables (4+ rows or 3+ cols) | `<table>` + horizontal scroll wrapper |
| Quotes | `<blockquote>` + accent border |
| Images | `<figure>` + `<figcaption>` + lightbox |
| Data/stats | Card grid or highlight box |
| Steps/process | Timeline or numbered cards |
| Comparison | Two-column grid |

### Content Routing Decision Tree (diagrams)
- <7 nodes, no branching → CSS Pipeline (Mermaid renders too small)
- 8-15 nodes → Mermaid (set fontSize: 18px+)
- >15 nodes → Mermaid overview (5-8 nodes) + CSS Grid detail cards

### Depth System (4 levels, never all same)
Every page/slide must use at least 2 depth levels:
- **Hero**: `background: color-mix(in srgb, var(--surface) 92%, var(--accent) 8%); box-shadow: 0 4px 20px rgba(0,0,0,0.08);`
- **Elevated**: `background: var(--surface-elevated); box-shadow: 0 2px 8px rgba(0,0,0,0.08);`
- **Default**: `background: var(--surface); border: 1px solid var(--border);`
- **Recessed**: `background: color-mix(in srgb, var(--bg) 70%, var(--surface) 30%); box-shadow: inset 0 1px 3px rgba(0,0,0,0.06);`

### Background Atmosphere (mandatory, pick 1)
Every output must have a non-flat background. Choose from:
1. Radial glow: `radial-gradient(ellipse at 50% 0%, var(--accent-dim) 0%, transparent 60%)`
2. Dot grid: `radial-gradient(circle, var(--border) 1px, transparent 1px); background-size: 24px 24px`
3. Diagonal stripes: `repeating-linear-gradient(-45deg, transparent, transparent 40px, var(--border) 40px, var(--border) 41px)`
4. Gradient mesh: two overlapping radial-gradients with accent colors
Exception: Swiss Clean preset explicitly uses flat white (the one permitted exception).

### Animation by Role (don't mix)
- Cards/bullets: fadeUp (0.4s)
- KPI numbers: fadeScale (0.35s)
- SVG lines: drawIn (0.8s)
- Slide entrance: slideReveal (0.6s)

### Anti-Patterns (explicit prohibitions)

Content（from Tufte/Reynolds/Alley/咨询公司研究）:
- **标题写主题词**而非结论句（"销售数据" → "Q3 销售额同比增长 22%"）
- **一张 slide 两个结论**（拆成两张）
- **纯 bullet list**（每条必须 heading+detail 二行模式）
- **视觉元素超过 6 个**（认知负荷 +500%，拆页）
- **正文字体 <18px**（presentation scale 最小值）
- **表格超 6 列在正文中**（移至 Appendix 或拆列）
- **连续 5+ 张同类型 slide**（节奏单调，插入变化页）
- **数字不标来源**（可信度崩塌）
- **非关键数据用 accent 色**（全灰基底 + 1 accent 指向关键数据）

Design:
- Emoji as icons (use SVG or CSS dot labels)
- Animated glow shadows
- Floating gradient orbs
- Rainbow/gradient borders
- Gradient text on headings
- Scale transforms on card hover (shadow elevation only)
- All content centered + uniform padding
- Identical cards with no visual hierarchy

CSS:
- `-clamp(...)` (use `calc(-1 * clamp(...))`)
- `@media (prefers-color-scheme)` for defining theme variables (use class-based)
- Fixed px font sizes in page mode (use clamp)
- `display: flex` on `<li>` elements (use absolute position for markers)
- Missing `min-height: 0` on flex/grid children containing charts

## Layer 3: Post-Generation (before delivery)

### Screenshot Verification Loop（from Manus，v3.9）

生成 HTML 后，用 Playwright MCP 逐页截图验证（可选，推荐）：

```
生成 HTML → 打开浏览器 → 逐页截图 → 自检 5 项 → 不通过则修正 → 重新截图
```

**5 项自检清单**：

| # | 检查项 | 方法 | 不通过处理 |
|---|--------|------|-----------|
| 1 | 内容不溢出 | JS: `slide.scrollHeight <= slide.clientHeight` | 减少内容或拆页 |
| 2 | body 填充率 >60% | 目测截图中 body 区域空白占比 | 执行 Breathing 或降级 |
| 3 | 卡片无大面积空白 | 目测卡片内部有效内容占比 | 缩小卡片（min-height+padding） |
| 4 | 字号在 6 级表内 | Grep HTML 中所有 font-size 值 | 修正为标准值 |
| 5 | ft 区域可见 | 截图底部有来源标注 | 修正 body 高度 |

**何时跳过**：内容量大（>10 页）且时间紧迫时可跳过，但必须在 Delivery Checklist 中标注"未做截图验证"。

### 提取质量验证测试（按格式分）

#### Slides 3 测试

**遮盖测试（Cover Test）**
遮住每页的 body，只看 h2 标题。仅从标题能理解这页核心信息吗？
- "瓶颈不是模型能力，是隐性知识" ✅ 遮住 body 也懂
- "问题背景" ❌ 遮住 body 什么都不知道
→ 验证 Assertion-Evidence 的断言质量

**电梯测试（Elevator Test）**
把所有 slides 的 h2 标题串成一段话读出来。能组成连贯的 30 秒电梯演讲吗？有跳跃或重复吗？
→ 验证叙事弧线连贯性

**空白测试（Whitespace Test）**
每页 body 内容是否撑满 616px 的 60% 以上？
- <40%: 先执行 Breathing Enlargement（slide-types.md R1），放大后仍不足则执行 Sparse Degradation（R3 降级矩阵）
- <60%: 内容不够饱满，回原文补细节或换 type
- ~80%: 理想
- >100%: 溢出，拆页
→ 验证 IU 提取饱满度

#### Poster 4 测试

**拇指测试（Thumb Test）**
手机上缩到缩略图大小（~2cm 宽），还能看清标题和感知内容吗？
→ 验证视觉层次在小尺寸下是否成立

**截图冲动测试（Screenshot Impulse Test）**
如果在小红书刷到这张海报，会截图发给朋友吗？
→ 验证截图点的冲击力

**脱离上下文测试（Context-Free Test）**
给没读过原文的人看，5 秒内能理解吗？
→ 验证术语和表达的通俗性

**分享文案测试（Share Caption Test）**
别人截图分享会配什么文字？能想象出一句自然的分享文案吗？
→ 验证"社交货币"价值

#### 保真度检查（所有格式）

对照 format-specs.md 保真度矩阵：
- slides: 有没有新增原文没有的信息？有没有改变原文的意思？
- deck: 过渡句是否只起连接作用，没有偷偷塞入新论点？
- poster: 文案化改写后是否仍忠于原文精神？

### 视觉质量测试

#### Squint Test
Blur your eyes and look at the page. Can you still perceive visual hierarchy? Are title/body/code visually distinct? If everything blurs into one mass → depth is insufficient.

#### Swap Test
Replace your fonts with Arial. Replace your colors with gray. If nobody can tell the difference → your design choices aren't impactful enough. Go back and make a bolder decision.

### Delivery Checklist

#### All Modes
- [ ] Anti-Slop 7 items all clear
- [ ] Squint Test passed (hierarchy visible)
- [ ] Swap Test passed (design is distinctive)
- [ ] Dark + Light themes both work
- [ ] 375px viewport: no horizontal overflow
- [ ] `prefers-reduced-motion` respected
- [ ] Background atmosphere present (not flat)
- [ ] At least 2 depth levels used

#### Page Mode (+)
- [ ] All code blocks have copy button
- [ ] All images have lightbox
- [ ] TOC highlights follow scroll
- [ ] OG tags present (og:title, og:description)
- [ ] Article content complete, nothing omitted
- [ ] Print CSS hides nav/progress/TOC

#### Slides/Deck Mode (+)
- [ ] 16:9 canvas renders correctly (1280x720)
- [ ] Dark surround visible around slides
- [ ] Keyboard navigation works (arrows, space, Home, End)
- [ ] Touch swipe works
- [ ] Slide counter visible and accurate
- [ ] `?export` mode: all slides visible, no animations
- [ ] No slide content overflows (scrollHeight <= clientHeight)
- [ ] Narrative coherent, sources cited (deck only)
- [ ] Entry animations fire on slide transition (CSS, not Reveal.js fragments)

### Per-Generation Variety
Track what was used last time. This time MUST differ:
- Last time dark → suggest light this time
- Last time Serif title → suggest Sans or Mono
- Last time symmetric layout → try asymmetric
