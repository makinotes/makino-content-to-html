# Style Presets Reference — v4.2

8 refined presets for slides/deck mode. Each preset is a complete design system: CSS variables + font pair + background atmosphere + 4-level depth + dark/light.

**设计参考**：Apple Keynote, Stripe, Linear, Vercel/Next.js Conf, Figma Config, Material Design 暗色规范, Garr Reynolds (Presentation Zen), Cole Nussbaumer Knaflic (Storytelling with Data)

**v4.2 校准**：body font 等宽→无衬线（等宽仅用于代码/数据，通过 `--font-mono` 变量注入），暗色 accent 降饱和 30-40%（Material 规范），文字 opacity 三级体系

---

## Dark Themes

### 1. Midnight Editorial

**Vibe:** Elegant editorial, deep navy, serif sophistication
**Ref:** Apple Keynote (serif title + sans body), NYT editorial dark mode

**Display font:** Instrument Serif (700)
**Body font:** DM Sans (400/500)
**Font source:** Google Fonts

**Colors:**
```css
:root {
    --bg: #0f1729;
    --surface: #162040;
    --surface-elevated: #243362;
    --text: #e8e4d8;
    --text-dim: rgba(255,255,255,0.60);
    --accent: #c9a54e;
    --accent-dim: rgba(201,165,78,0.1);
    --border: rgba(200,180,140,0.08);
    --viewport-bg: #080d1a;
}
```

**Background atmosphere:** Radial glow (warm gold, top-center)

**Depth system:**
- Hero: accent-tinted surface + outer shadow
- Elevated: lighter surface
- Default: base surface
- Recessed: inset shadow

---

### 2. Terminal Mono

**Vibe:** Vercel-inspired precision, monospace titles, clean dark
**Ref:** Vercel/Next.js Conf (Geist 字体系统, 纯黑底+排版精度出科技感)

**Display font:** Geist Mono or JetBrains Mono (700)
**Body font:** Geist Sans or Inter (400/500)
**Font source:** Google Fonts (JetBrains Mono / Inter) or Fontshare (Geist)

**Colors:**
```css
:root {
    --bg: #0a0e14;
    --surface: #12161e;
    --surface-elevated: #1a1f2a;
    --text: rgba(255,255,255,0.87);
    --text-dim: rgba(255,255,255,0.50);
    --accent: #6ee7a0;
    --accent-dim: rgba(110,231,160,0.08);
    --border: rgba(110,231,160,0.06);
    --viewport-bg: #000000;
}
```

**Background atmosphere:** Subtle dot grid only (no scan lines — Tufte: minimize non-data ink)

**Depth system:**
- Hero: accent border-top + slight glow
- Elevated: lighter bg
- Default: base
- Recessed: inset shadow

---

### 3. Bold Signal

**Vibe:** High-impact, confident, orange focal card on dark
**Ref:** Stripe (渐变做情绪不做装饰), Apple (数字放大做视觉锤)

**Display font:** Archivo Black (900)
**Body font:** Space Grotesk (400/500)
**Font source:** Google Fonts

**Colors:**
```css
:root {
    --bg: #1a1a1a;
    --surface: #2d2d2d;
    --surface-elevated: #3a3a3a;
    --text: rgba(255,255,255,0.87);
    --text-dim: rgba(255,255,255,0.60);
    --accent: #e8744a;
    --accent-dim: rgba(232,116,74,0.1);
    --border: rgba(255,255,255,0.08);
    --viewport-bg: #0d0d0d;
}
```

**Background atmosphere:** Gradient mesh (135deg, dark)

**Depth system:**
- Hero: accent-colored card (full bg)
- Elevated: lighter surface
- Default: dark surface
- Recessed: darkest

**Signature:** Large section numbers (01, 02...) in 200px+, weight 200, opacity 0.06

---

### 4. Neon Cyber

**Vibe:** Futuristic, techy, confident, cyan+magenta neon
**Ref:** Stripe (渐变只用在 Hero 区域做视觉焦点, 数据区回归纯净)

**Display font:** Clash Display (700)
**Body font:** Satoshi (400/500)
**Font source:** Fontshare

**Colors:**
```css
:root {
    --bg: #0a0f1c;
    --surface: #111827;
    --surface-elevated: #1e293b;
    --text: rgba(255,255,255,0.87);
    --text-dim: rgba(255,255,255,0.55);
    --accent: #5cf5d4;
    --accent-secondary: #f472b6;
    --accent-dim: rgba(92,245,212,0.08);
    --border: rgba(92,245,212,0.06);
    --viewport-bg: #050810;
}
```

**Background atmosphere:** Gradient mesh only (two radial: cyan top-right + magenta bottom-left). No grid overlay — keep clean.

**Depth system:**
- Hero: neon glow shadow
- Elevated: border glow
- Default: base
- Recessed: deep dark

---

## Light Themes

### 5. Warm Signal

**Vibe:** Cream paper, warm editorial, brick-red accent
**Ref:** Linear (精密排版+高信息密度但不拥挤), Garr Reynolds (留白是设计元素)

**Display font:** Plus Jakarta Sans (700/800)
**Body font:** Work Sans (400/500)
**Font source:** Google Fonts

**Colors:**
```css
:root {
    --bg: #faf6f0;
    --surface: #ffffff;
    --surface-elevated: #ffffff;
    --text: #2c2a25;
    --text-dim: #7c756a;
    --accent: #c2410c;
    --accent-dim: rgba(194,65,12,0.08);
    --border: rgba(0,0,0,0.06);
    --viewport-bg: #1a1a1a;
}
```

**Dark mode override:**
```css
.dark {
    --bg: #1c1917;
    --surface: #292524;
    --text: #e7e5e4;
    --accent: #ea580c;
}
```

**Background atmosphere:** Diagonal stripes (very subtle)

---

### 6. Swiss Clean

**Vibe:** Precise, Bauhaus-inspired, single blue accent
**Ref:** Linear (极简+微妙层级系统), Swiss International Style (Helvetica 传统)

**Display font:** DM Sans (700/800)
**Body font:** Inter (400/500)
**Font source:** Google Fonts

**Colors:**
```css
:root {
    --bg: #ffffff;
    --surface: #f8f8f8;
    --surface-elevated: #ffffff;
    --text: #111111;
    --text-dim: #666666;
    --accent: #0055ff;
    --accent-dim: rgba(0,85,255,0.06);
    --border: rgba(0,0,0,0.06);
    --viewport-bg: #1a1a1a;
}
```

**Dark mode override:**
```css
.dark {
    --bg: #111111;
    --surface: #1a1a1a;
    --text: #eeeeee;
    --accent: #4488ff;
}
```

**Background atmosphere:** None (Swiss = pure, exception to the "no flat bg" rule)

**Depth system:** Minimal shadows, rely on border and spacing

---

### 7. Dark Botanical

**Vibe:** Elegant, sophisticated, warm gold/pink on black
**Ref:** 高端品牌/奢侈品调性，适合年度总结、品牌故事类演示

**Display font:** Cormorant (400/600)
**Body font:** IBM Plex Sans (300/400)
**Font source:** Google Fonts

**Colors:**
```css
:root {
    --bg: #0f0f0f;
    --surface: #1a1a1a;
    --surface-elevated: #252525;
    --text: #e8e4df;
    --text-dim: #9a9590;
    --accent: #d4a574;
    --accent-secondary: #e8b4b8;
    --accent-dim: rgba(212,165,116,0.08);
    --border: rgba(255,255,255,0.06);
    --viewport-bg: #000000;
}
```

**Background atmosphere:** Abstract soft gradient circles (blurred, overlapping, warm colors)

**Signature:** Thin vertical accent lines, italic signature typography

---

### 8. Notebook Tabs

**Vibe:** Editorial, organized, tactile paper feeling
**Ref:** Moleskine/实体笔记本质感，Figma Config (形状驱动的视觉语言)

**Display font:** Bodoni Moda (400/700)
**Body font:** DM Sans (400/500)
**Font source:** Google Fonts

**Colors:**
```css
:root {
    --bg-outer: #2d2d2d;
    --bg-page: #f8f6f1;
    --text: #1a1a1a;
    --text-dim: #666666;
    --accent: #1a1a1a;
    --tab-mint: #98d4bb;
    --tab-lavender: #c7b8ea;
    --tab-pink: #f4b8c5;
    --tab-sky: #a8d8ea;
    --tab-cream: #ffe6a7;
    --viewport-bg: #2d2d2d;
}
```

**Background atmosphere:** Paper container with subtle shadow on dark outer bg

**Signature:** Colorful section tabs on right edge (vertical text), binder holes on left

---

## Shared Rules

### Text Opacity 三级体系（from Material Design 暗色规范）

暗色主题中文字靠 opacity 区分层级，不用独立 hex 值：

```css
--text: rgba(255,255,255,0.87);       /* 主文字 — 标题、正文 */
--text-dim: rgba(255,255,255,0.60);   /* 次文字 — 说明、标签 */
--text-disabled: rgba(255,255,255,0.38); /* 禁用/最弱 — footer、占位 */
```

亮色主题同理：`rgba(0,0,0, 0.87/0.60/0.38)`。

**为什么不用 hex**：opacity 体系自动适配任何底色，换主题不用改文字色。Linear 和 Material Design 都用这套。

### Accent 色降饱和规则（from Material Design + Stripe）

暗色背景上高饱和色会"震动"（视觉疲劳）。accent 色必须降饱和：

```css
/* 原始品牌色 → 暗色适配色 */
#FF5722 → #e8744a  /* Bold Signal: 橙 */
#50fa7b → #6ee7a0  /* Terminal: 绿 */
#00ffcc → #5cf5d4  /* Neon: 青 */
#ff00aa → #f472b6  /* Neon: 粉 */
```

**方法**：HSL 中 S 降 30-40%，L 提 10-15%。或用 `color-mix(in srgb, 品牌色 60%, white 40%)`。

### Card Anti-Float 背景差异（from 专业 PPT 工具调研 v3.8）

卡片/组件不能"漂浮"在空白中。用微妙背景色差锚定视觉位置：

```css
/* Card anti-float: 卡片背景比 slide 背景略浅/深 */
.card, .tier, .ba-panel, .col-card, .matrix-q {
  background: color-mix(in srgb, var(--bg) 92%, var(--text) 8%);
}
```

**4 种锚定手段**（至少用 1 种）：
1. **背景差异**：`color-mix(in srgb, var(--bg) 92%, var(--text) 8%)`（2-8% 色差）
2. **单侧 accent**：左边框 4px / 顶部 accent bar（不用全边框）
3. **区域分隔线**：标题区和内容区之间 1px, 10% opacity 分隔线
4. **格式塔邻近性**：卡片内 padding < 卡片间 gap（内紧外松）

**Dark theme**：`color-mix(in srgb, var(--bg) 92%, var(--text) 8%)` 在深色背景上自动偏亮。
**Light theme**：同规则在浅色背景上自动偏深。不需要 dark/light 两套。

### Accent 色战略性使用（from Knaflic/Williams）

accent 色是稀缺资源，不是装饰。严格限制使用场景：
- **关键数字**：Hero Stat 的大数字、Dashboard 的 KPI 值
- **推荐选项**：Three-Column 的 `.recommended` 边框、Before-After 的 AFTER 面板
- **当前步骤**：Pipeline 的当前/高亮步骤、Timeline 的 active node
- **高亮行**：Table 的 `td.hl`、Tier card 的左侧边条

**禁止用 accent 的地方**：正文 body text、非关键数据、所有 card 同时高亮（= 没有高亮）。
**非关键数据一律灰色**（`var(--dim)` 或 `opacity: 0.5`），让 accent 在视觉上"跳出来"。

### Font Pairing Pool (13 pairs, rotate across generations)

**规则**：Body font 必须是无衬线/衬线正文体。等宽字体仅用于代码块和数据表格（`--font-mono`），不做正文。

| # | Display | Body | Code (optional) | Mood |
|---|---------|------|-----------------|------|
| 1 | Instrument Serif | DM Sans | JetBrains Mono | Editorial dark |
| 2 | Geist Mono | Inter | Geist Mono | Vercel precision |
| 3 | Archivo Black | Space Grotesk | — | Bold impact |
| 4 | Clash Display | Satoshi | — | Futuristic |
| 5 | Plus Jakarta Sans | Work Sans | Azeret Mono | Warm editorial |
| 6 | DM Sans | Inter | Fira Code | Swiss precise |
| 7 | Cormorant | IBM Plex Sans | — | Elegant serif |
| 8 | Bodoni Moda | DM Sans | — | Notebook classic |
| 9 | Fraunces | Work Sans | — | Vintage editorial |
| 10 | Syne | Space Grotesk | Space Mono | Creative voltage |
| 11 | Manrope | Manrope | — | Clean professional |
| 12 | Outfit | Outfit | — | Playful modern |
| 13 | Cormorant Garamond | Source Serif 4 | — | Literary |

**Display 用等宽的例外**：#2 (Geist Mono) 和 #10 (Syne+Space Mono) 的标题用等宽是有意为之（科技/创意场景），但 body 仍然是无衬线。

### Background Atmosphere (4 patterns)

**Radial glow:**
```css
background: radial-gradient(ellipse 80% 50% at 50% -10%, rgba(212,167,58,0.15) 0%, transparent 70%);
```

**Dot grid:**
```css
background-image: radial-gradient(circle, rgba(255,255,255,0.06) 1px, transparent 1px);
background-size: 24px 24px;
```

**Diagonal stripes:**
```css
background-image: repeating-linear-gradient(
    45deg,
    transparent,
    transparent 40px,
    rgba(0,0,0,0.015) 40px,
    rgba(0,0,0,0.015) 41px
);
```

**Gradient mesh:**
```css
background:
    radial-gradient(ellipse 60% 60% at 80% 20%, rgba(0,255,204,0.12) 0%, transparent 60%),
    radial-gradient(ellipse 60% 60% at 20% 80%, rgba(255,0,170,0.10) 0%, transparent 60%);
```

### Depth System (4 levels)

```css
/* Hero — accent-tinted + outer shadow */
.depth-hero {
    background: color-mix(in srgb, var(--surface) 85%, var(--accent) 15%);
    box-shadow: 0 20px 60px rgba(0,0,0,0.4), 0 0 0 1px var(--border);
}

/* Elevated — lighter surface + small shadow */
.depth-elevated {
    background: var(--surface-elevated);
    box-shadow: 0 8px 24px rgba(0,0,0,0.2), 0 0 0 1px var(--border);
}

/* Default — base surface + border */
.depth-default {
    background: var(--surface);
    border: 1px solid var(--border);
}

/* Recessed — inset shadow */
.depth-recessed {
    background: var(--bg);
    box-shadow: inset 0 2px 8px rgba(0,0,0,0.2);
}
```

---

## DO NOT USE

**Fonts:** Inter, Roboto, Arial, system fonts as display. 等宽字体做正文 body（仅标题可用等宽）

**Colors:** `#6366f1` (generic indigo), purple gradients on white, 暗底上纯白 `#FFFFFF` 做正文（用 `rgba(255,255,255,0.87)` 替代, Material Design 规范）, 高饱和 accent 未降饱和直接用于暗底

**Layouts:** Everything centered uniform, identical card grids, 一页塞 3+ 个信息点（Garr Reynolds: 一页一想法）

**Decorations:** Emoji icons, gratuitous glassmorphism, glow animations, scan lines / decorative grids（Tufte: minimize non-data ink）

---

## CSS Gotchas

**Negating CSS functions — WRONG (silently ignored by browsers):**
```css
right: -clamp(28px, 3.5vw, 44px);
margin-left: -min(10vw, 100px);
```

**CORRECT — wrap in `calc()`:**
```css
right: calc(-1 * clamp(28px, 3.5vw, 44px));
margin-left: calc(-1 * min(10vw, 100px));
```

CSS does not allow a leading `-` before function names. The browser silently discards the entire declaration — no error, the element just appears in the wrong position. Always use `calc(-1 * ...)` to negate CSS function values.

**Color scheme theming — WRONG:**
```css
@media (prefers-color-scheme: dark) {
    :root { --bg: #111; }
}
```

**CORRECT — use class-based theming:**
```css
.dark { --bg: #111; }
```

Never use bare `@media (prefers-color-scheme)` for variables — use class-based theming for reliable control.
