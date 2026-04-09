# HTML Template — Fixed Card Architecture

slides/deck/poster 共用的 HTML 架构。不使用 Reveal.js，纯 CSS + JS。

## Base HTML Structure (slides/deck)

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Presentation Title</title>
  <!-- Fonts: Google Fonts or Fontshare -->
  <link href="https://fonts.googleapis.com/css2?family=..." rel="stylesheet">
  <style>
    /* === RESET === */
    *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

    /* === BODY === */
    body {
      background: #1a1a2e;
      font-family: var(--font-body);
      display: flex; justify-content: center; align-items: center;
      min-height: 100vh; overflow: hidden;
    }

    /* === SLIDE CARD === */
    .slide {
      width: 1280px; height: 720px;
      position: absolute; top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      background: var(--bg);
      color: var(--text);
      box-shadow: 0 8px 32px rgba(0,0,0,.25);
      border-radius: 6px;
      overflow: hidden;
      display: none; flex-direction: column;
    }
    .slide.active { display: flex; }

    /* === 5-LAYER STRUCTURE === */
    .slide .hdr  { padding: 16px 40px 0; display: flex; align-items: center; justify-content: space-between; flex-shrink: 0; }
    .slide .sub  { margin: 4px 40px 0; flex-shrink: 0; }
    .slide .body { padding: 12px 40px 0; flex: 1; display: flex; flex-direction: column; min-height: 0; }
    .slide .ft   { padding: 8px 40px 12px; flex-shrink: 0; }

    /* === THEME VARIABLES === */
    :root { /* from STYLE_PRESETS.md */ }

    /* === ATMOSPHERE === */
    .slide::after { /* from preset */ }
    .slide > * { position: relative; z-index: 1; }

    /* === TYPOGRAPHY (PRESENTATION SCALE — see slide-types.md) === */
    .slide .hdr h2    { font-size: 36px; font-weight: 800; line-height: 1.2; }
    .slide .hdr .page { font-size: 12px; font-weight: 600; letter-spacing: 1px; }
    .slide .sub       { font-size: 11px; font-weight: 600; text-transform: uppercase; letter-spacing: 2px; }
    .slide .body li   { font-size: 18px; line-height: 1.8; margin-bottom: 10px; }
    .slide .body p    { font-size: 18px; line-height: 1.8; }
    .slide .ft        { font-size: 11px; letter-spacing: .3px; text-align: center; }

    /* === COMPONENTS (from slide-types.md) === */

    /* === TITLE SLIDE === */
    .title-slide { justify-content: center; align-items: center; text-align: center; padding: 36px; }

    /* === NAV === */
    #nav { position: fixed; bottom: 16px; left: 50%; transform: translateX(-50%); display: flex; align-items: center; gap: 10px; z-index: 100; }
    #nav button { width: 30px; height: 30px; border-radius: 6px; border: 1px solid rgba(255,255,255,0.06); background: rgba(255,255,255,0.03); color: #555; cursor: pointer; font-size: 13px; }
    #counter { font-size: 10px; min-width: 50px; text-align: center; }

    @media (prefers-reduced-motion: reduce) { *, *::before, *::after { animation-duration: 0.01ms !important; } }
    @media print { #nav { display: none; } .slide { position: relative; display: flex !important; page-break-after: always; transform: none; top: auto; left: auto; margin: 20px auto; } }
  </style>
</head>
<body>
  <div class="slide title-slide active">...</div>
  <div class="slide">
    <div class="hdr"><h2>Title</h2><span class="page">02</span></div>
    <div class="sub">// LABEL</div>
    <div class="body">...</div>
    <div class="ft">Source · Date</div>
  </div>

  <div id="nav">
    <button id="prev">&larr;</button>
    <span id="counter">01 / 14</span>
    <button id="next">&rarr;</button>
  </div>

  <script>
    var slides = document.querySelectorAll('.slide');
    var current = 0, total = slides.length;
    function show(i) {
      if (i < 0 || i >= total) return;
      slides[current].classList.remove('active');
      current = i;
      slides[current].classList.add('active');
      document.getElementById('counter').textContent =
        String(current+1).padStart(2,'0') + ' / ' + String(total).padStart(2,'0');
    }
    document.getElementById('prev').onclick = function(){ show(current-1); };
    document.getElementById('next').onclick = function(){ show(current+1); };
    document.addEventListener('keydown', function(e){
      if(e.key==='ArrowRight'||e.key===' '||e.key==='ArrowDown'||e.key==='PageDown'){e.preventDefault();show(current+1);}
      else if(e.key==='ArrowLeft'||e.key==='ArrowUp'||e.key==='PageUp'){e.preventDefault();show(current-1);}
      else if(e.key==='Home'){e.preventDefault();show(0);}
      else if(e.key==='End'){e.preventDefault();show(total-1);}
    });
    var ty;
    document.addEventListener('touchstart',function(e){ty=e.touches[0].clientY;},{passive:true});
    document.addEventListener('touchend',function(e){var d=ty-e.changedTouches[0].clientY;if(Math.abs(d)>50){d>0?show(current+1):show(current-1);}});
  </script>
</body>
</html>
```

## Canvas Sizes by Format

| Format | Width | Height |
|--------|-------|--------|
| slides/deck | 1280px | 720px |
| poster-9x16 | 1080px | 1920px |
| poster-1x1 | 1080px | 1080px |
| poster-16x9 | 1920px | 1080px |

Change `.slide { width: Xpx; height: Ypx; }` per format.

## Poster (single card, no nav)

```html
<body>
  <div class="poster active">
    <div class="poster-hdr">...</div>
    <div class="poster-title">...</div>
    <div class="poster-body">...</div>
    <div class="poster-ft">...</div>
  </div>
</body>
```

No `#nav`, no slide controller JS. See `format-specs.md` for poster padding/typography.

## Section Label (NO emoji)

```css
.sub::before { content: ''; width: 6px; height: 6px; border-radius: 50%; background: currentColor; }
```

## Image Paths

HTML in `presentation/` or `poster/` or `html/` → images at `../images/xxx.png`

## Constraint Levels（from Beautiful.ai 约束式设计调研）

每个 Type 的属性分两级——**锁死**（sub-agent 不能改）和**内容响应**（随内容自动调整）：

| 属性 | 锁死 | 内容响应 |
|------|------|---------|
| 画布尺寸 | 1280x720px | - |
| side padding | 40px | - |
| hdr/sub/ft 高度 | 固定值 | - |
| 字号 | 6 级表里的值 | - |
| 字体 | preset 定义的 2 个 | - |
| 卡片高度下限 | min-height（80/100/120px） | - |
| 卡片高度 | - | 按内容 + padding 自然撑开 |
| grid 行列数 | 按内容量决定（4 卡 = 2x2，6 卡 = 2x3） | - |
| grid 高度 | - | 按卡片自然高度，body 居中 |
| body 策略 | 按 Type 定义选（center/space-evenly） | - |
| accent 使用 | 只用于规定的 5 种场景 | - |

**原则**：锁死的属性越多，sub-agent 犯错空间越小，输出越一致。

## HTML Constraints（from Manus 泄露 prompt 逆向，v3.9）

生成 HTML 时必须遵守的硬约束（违反会导致截图验证失败或导出异常）：

| 约束 | 规则 | 原因 |
|------|------|------|
| **内容不溢出** | body 内容总高度 ≤ 720px（scrollHeight ≤ clientHeight） | 固定画布不能滚动 |
| **底部边距** | .ft 区域不被压缩，始终可见 | 来源标注是可信度基础 |
| **背景色/边框** | 只在 `<div>` 上设，不在 `<p>/<li>/<h2>` 上加 | 语义标签不承载视觉装饰 |
| **禁止 CSS 渐变** | 不用 `linear-gradient`/`radial-gradient` 做内容元素背景（preset 氛围层除外） | 截图/导出一致性 |
| **颜色格式** | 优先 CSS 变量，直接写色值时用 `#RRGGBB`，不用 rgba 在内容元素上 | 可读性 + 导出兼容 |
| **显式声明** | 每个文本元素显式声明 font-size + color（不依赖继承链） | sub-agent 覆盖父级时不会意外改子级 |
| **文本宽度** | 任何文本容器 min-width: 100px | 防截断 |
