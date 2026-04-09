# Slide JSON Schema — v1.0

sub-agent 输出 JSON，渲染引擎输出 HTML。设计规则全锁在渲染引擎里，sub-agent 只负责内容决策。

## 顶层结构

```json
{
  "meta": {
    "title": "Presentation Title",
    "preset": "midnight-editorial",
    "density": 1.0,
    "fonts": {
      "display": "Instrument Serif",
      "body": "JetBrains Mono"
    }
  },
  "slides": [ /* SlideObject[] */ ]
}
```

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| meta.title | string | Y | deck 标题 |
| meta.preset | string | Y | STYLE_PRESETS.md 中的 8 选 1 |
| meta.density | number | Y | 密度系数 D（1.0 / 1.25 / 1.6） |
| meta.fonts | object | Y | display + body 字体 |
| slides | array | Y | SlideObject 数组 |

## SlideObject 通用字段

```json
{
  "type": "content",
  "page": "03",
  "title": "Slide Heading (assertion sentence)",
  "subtitle": "// SECTION LABEL",
  "footer": "Source · Date",
  "content": { /* 按 type 不同 */ }
}
```

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| type | enum | Y | 15 种之一 |
| page | string | N | 页码（"01"-"99"），title/divider 可省略 |
| title | string | Y* | hdr 区 h2（title/divider/quote 类型可省略） |
| subtitle | string | N | sub 区标签 |
| footer | string | N | ft 区来源标注 |
| content | object | Y | 按 type 定义，见下 |

## 15 种 Type 的 content 定义

### type: "title"

```json
{
  "heading": "Main Title Text",
  "subtitle": "Subtitle text",
  "tags": ["Tag1", "Tag2"],
  "meta": "Date · Source · N slides"
}
```

### type: "divider"

```json
{
  "number": "02",
  "section": "SECTION NAME",
  "heading": "Section Title"
}
```

### type: "content"

```json
{
  "items": [
    { "heading": "Core assertion", "detail": "Supporting detail 1 line" },
    { "heading": "Second point", "detail": "Detail text" }
  ]
}
```

items 长度 3-6。每条必须有 heading + detail（anti-bullet-list 规则）。

### type: "split"

```json
{
  "left": {
    "variant": "text",
    "heading": "Left heading",
    "items": [
      { "heading": "Point", "detail": "Detail" }
    ]
  },
  "right": {
    "variant": "text|image|card",
    "heading": "Right heading",
    "items": [],
    "image": "../images/xxx.png",
    "caption": "Figure caption"
  }
}
```

variant: "text" | "image" | "card"。image 时用 image+caption 字段。

### type: "dashboard"

```json
{
  "cards": [
    { "value": "42%", "label": "Conversion Rate", "accent": true },
    { "value": "$1.2B", "label": "Revenue", "accent": false },
    { "value": "4100+", "label": "Daily Users", "accent": false }
  ]
}
```

cards 长度 3-6。accent: true 的卡片用 accent 色高亮（最多 1 张）。

### type: "table"

```json
{
  "columns": ["Col1", "Col2", "Col3"],
  "rows": [
    ["Cell", "Cell", "Cell"],
    ["Cell", "**highlighted**", "Cell"]
  ],
  "callout": "Key insight text (optional)"
}
```

`**text**` 标记 = td.hl accent 高亮。callout 触发 Table+Callout 变体。

### type: "code"

```json
{
  "filename": "config.py",
  "language": "python",
  "code": "def hello():\n    return 'world'"
}
```

最多 10 行。

### type: "quote"

```json
{
  "text": "The quote text, max ~25 words",
  "attribution": "— Author Name, Title"
}
```

quote 类型不用 5-layer 结构，全居中。

### type: "diagram"

```json
{
  "variant": "pipeline",
  "steps": [
    { "number": "01", "title": "Step Name", "detail": "Brief desc" }
  ]
}
```

variant: "pipeline" (CSS) | "mermaid"。pipeline 最多 7 步。

### type: "full-bleed"

```json
{
  "image": "../images/hero.png",
  "heading": "Title over image",
  "description": "Brief description"
}
```

### type: "hero-stat"

```json
{
  "number": "42%",
  "context": "vs 23% last quarter",
  "delta": "+19pp YoY"
}
```

number 必须是具体数字，不是概念词。

### type: "timeline"

```json
{
  "nodes": [
    { "year": "2022", "title": "Founded", "detail": "Description", "active": false },
    { "year": "2024", "title": "Launch", "detail": "Description", "active": true }
  ]
}
```

nodes 长度 4-8。active: true 用 accent 色 + glow。

### type: "before-after"

```json
{
  "before": {
    "label": "BEFORE",
    "stat": "2 days",
    "items": ["Detail 1", "Detail 2", "Detail 3"]
  },
  "after": {
    "label": "AFTER",
    "stat": "30 min",
    "items": ["Detail 1", "Detail 2", "Detail 3"]
  }
}
```

### type: "matrix"

```json
{
  "xLabel": "X Axis Label",
  "yLabel": "Y Axis Label",
  "quadrants": [
    { "heading": "Q1", "items": ["Item1", "Item2"], "highlight": false },
    { "heading": "Q2", "items": ["Item1"], "highlight": true },
    { "heading": "Q3", "items": ["Item1", "Item2"], "highlight": false },
    { "heading": "Q4", "items": ["Item1"], "highlight": false }
  ]
}
```

highlight: true 的象限用 accent 背景。

### type: "three-column"

```json
{
  "columns": [
    {
      "heading": "Option A",
      "items": ["Point 1", "Point 2", "Point 3"],
      "stat": "85%",
      "recommended": false
    },
    {
      "heading": "Option B",
      "items": ["Point 1", "Point 2"],
      "stat": "92%",
      "recommended": true
    },
    {
      "heading": "Option C",
      "items": ["Point 1", "Point 2", "Point 3"],
      "stat": "78%",
      "recommended": false
    }
  ]
}
```

recommended: true 的列用 accent 边框。

---

## 验证规则（渲染引擎在渲染前检查）

| 规则 | 检查 |
|------|------|
| type 合法 | 15 种之一 |
| content 必填字段 | 按 type 检查 |
| items 长度 | content 3-6, dashboard 3-6, timeline 4-8 |
| 字符串非空 | title, heading 等不为 "" |
| number 是数字 | hero-stat.number 含数字字符 |
| accent 唯一 | dashboard.cards 最多 1 个 accent:true |

渲染引擎遇到验证失败：console.warn + 用 fallback 渲染（灰色占位块 + 错误信息）。
