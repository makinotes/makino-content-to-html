# Animation Patterns Reference

## 1. Animation-by-Role System

| Content Type | Animation Name | Duration | Easing |
|---|---|---|---|
| Cards / bullets | fadeUp | 0.4s | ease-out |
| KPI numbers / badges | fadeScale | 0.35s | ease-out |
| SVG connections | drawIn | 0.8s | ease-in-out |
| Slide entrance | slideReveal | 0.6s | cubic-bezier(0.16,1,0.3,1) |
| Number counters | countUp | 1.2s | ease-out |
| Code blocks | fadeIn (no motion) | 0.3s | ease |

## 2. CSS Implementations

**fadeUp** — cards, bullets, list items

```css
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: translateY(0); }
}
.fade-up {
  animation: fadeUp 0.4s ease-out both;
  animation-delay: calc(var(--i, 0) * 0.05s);
}
```

**fadeScale** — KPI numbers, badges

```css
@keyframes fadeScale {
  from { opacity: 0; transform: scale(0.92); }
  to   { opacity: 1; transform: scale(1); }
}
.fade-scale {
  animation: fadeScale 0.35s ease-out both;
  animation-delay: calc(var(--i, 0) * 0.06s);
}
```

**drawIn** — SVG connections, lines, paths

```css
@keyframes drawIn {
  from { stroke-dashoffset: var(--path-length); }
  to   { stroke-dashoffset: 0; }
}
.draw-in {
  stroke-dasharray: var(--path-length);
  stroke-dashoffset: var(--path-length);
  animation: drawIn 0.8s ease-in-out both;
}
```

```javascript
// Set --path-length via JS after DOM ready
document.querySelectorAll('.draw-in').forEach(el => {
  const len = el.getTotalLength();
  el.style.setProperty('--path-length', len);
});
```

**countUp** — animated number counters

```css
/* CSS @property approach (Chrome/Edge) */
@property --count {
  syntax: '<integer>';
  initial-value: 0;
  inherits: false;
}
@keyframes countUp {
  from { --count: 0; }
  to   { --count: var(--count-target); }
}
.count-up {
  animation: countUp 1.2s ease-out both;
  counter-reset: count var(--count);
}
.count-up::after {
  content: counter(count) attr(data-suffix);
}
```

```javascript
// JS fallback for Safari — requestAnimationFrame with cubic ease-out
document.querySelectorAll('[data-count]').forEach(el => {
  const target = parseInt(el.dataset.count, 10);
  const suffix = el.dataset.suffix || '';
  const duration = 1200;
  const start = performance.now();
  const easeOut = t => 1 - Math.pow(1 - t, 3);
  function tick(now) {
    const t = Math.min((now - start) / duration, 1);
    el.textContent = Math.round(easeOut(t) * target) + suffix;
    if (t < 1) requestAnimationFrame(tick);
  }
  requestAnimationFrame(tick);
});
```

**fadeIn (no motion)** — code blocks, verbatim content

```css
@keyframes fadeIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}
.fade-in {
  animation: fadeIn 0.3s ease both;
}
```

## 3. Slide Transition (fixed-card mode)

Slides 使用固定卡片架构（非 Reveal.js）。页面切换通过 JS 控制 `.active` class，无需 fragment 动画。

如需页内元素渐入效果，用 CSS animation + JS 在 slide 切换时触发：

```css
.slide.active .animate-in {
  animation: fadeUp 0.4s ease-out both;
}
```

不使用 Reveal.js 的 `class="fragment"` 或 `data-auto-animate`。

## 4. Slide Entrance (page mode, IntersectionObserver)

```css
.slide-section {
  opacity: 0;
  transform: translateY(40px) scale(0.98);
  transition: opacity 0.6s cubic-bezier(0.16,1,0.3,1),
              transform 0.6s cubic-bezier(0.16,1,0.3,1);
}
.slide-section.visible {
  opacity: 1;
  transform: translateY(0) scale(1);
}

/* Stagger children */
.slide-section.visible > *:nth-child(1) { transition-delay: 0.0s; }
.slide-section.visible > *:nth-child(2) { transition-delay: 0.1s; }
.slide-section.visible > *:nth-child(3) { transition-delay: 0.2s; }
.slide-section.visible > *:nth-child(4) { transition-delay: 0.3s; }
.slide-section.visible > *:nth-child(5) { transition-delay: 0.4s; }
.slide-section.visible > *:nth-child(6) { transition-delay: 0.5s; }
```

```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.15 });

document.querySelectorAll('.slide-section').forEach(el => observer.observe(el));
```

## 5. Background Atmosphere Animations

Radial glow pulse — applied to `::before` pseudo-element only:

```css
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(ellipse at 50% 50%, rgba(99,102,241,0.4) 0%, transparent 60%);
  animation: glowPulse 8s ease-in-out infinite;
  pointer-events: none;
}
@keyframes glowPulse {
  0%, 100% { opacity: 0.6; }
  50%       { opacity: 1; }
}
```

## 6. Forbidden Animations

Do NOT use any of the following:

- `@keyframes` box-shadow glow loops on interactive elements
- Breathing / pulsing on static content (text, icons, cards at rest)
- Any continuously running animation after page load completes — except explicit progress bars
- `scale` transforms on card hover (use `box-shadow` elevation change only)
- Gradient text animation on headings (`background-clip: text` + shifting gradient)
- Floating gradient orbs / ambient blobs with continuous motion
- Rainbow or animated gradient borders

## 7. Reduced Motion

Always include this block — no exceptions:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
  html {
    scroll-behavior: auto;
  }
}
```
