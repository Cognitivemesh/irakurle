# Visual Designer Skill


### Link Styling

Never rely on browser default link colors. The default blue (#0000EE) has poor contrast on dark backgrounds. Style links with color: var(--accent) and keep underlines for discoverability. On light backgrounds, use deeper tones (#0891b2, #059669, #d97706).

## Section / Card Components

The fundamental building block. A colored card representing a system component, pipeline step, or data entity.

**IMPORTANT: Never use `.node` as a CSS class name.** Mermaid.js internally uses `.node` on its SVG `<g>` elements with `transform: translate(x, y)` for positioning. Any page-level `.node` styles (hover transforms, box-shadows, transitions) will leak into Mermaid diagrams and break their layout. Use `.ve-card` instead (namespaced to avoid collisions with CSS frameworks like Bootstrap/Tailwind that also use `.card`).

```css
.ve-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 16px 20px;
  position: relative;
}

/* Colored accent border (left or top) */
.ve-card--accent-a {
  border-left: 3px solid var(--node-a);
}

/* --- Depth tiers: vary card depth to signal importance --- */

/* Elevated: KPIs, key sections, anything that should pop */
.ve-card--elevated {
  background: var(--surface-elevated);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08), 0 1px 2px rgba(0, 0, 0, 0.04);
}

/* Recessed: code blocks, secondary content, detail panels */
.ve-card--recessed {
  background: color-mix(in srgb, var(--bg) 70%, var(--surface) 30%);
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.06);
  border-color: var(--border);
}

/* Hero: executive summaries, focal elements — demands attention */
.ve-card--hero {
  background: color-mix(in srgb, var(--surface) 92%, var(--accent) 8%);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08), 0 1px 3px rgba(0, 0, 0, 0.04);
  border-color: color-mix(in srgb, var(--border) 50%, var(--accent) 50%);
}

/* Glass: special-occasion overlay effect (use sparingly) */
.ve-card--glass {
  background: color-mix(in srgb, var(--surface) 60%, transparent 40%);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-color: rgba(255, 255, 255, 0.1);
}

/* Section label (monospace, uppercase, small) */
.ve-card__label {
  font-family: var(--font-mono);
  font-size: 10px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1.5px;
  color: var(--node-a);
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  gap: 8px;
}

/* Colored dot indicator */
.ve-card__label::before {
  content: '';
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: currentColor;
}
```

## Code Blocks

Code blocks need explicit whitespace preservation and a max-height constraint. Without these, code runs together and long files overwhelm the page.

### Basic Pattern

```css
.code-block {
  font-family: var(--font-mono);
  font-size: 13px;
  line-height: 1.5;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 16px;
  overflow-x: auto;
  /* CRITICAL: preserve line breaks and indentation */
  white-space: pre-wrap;
  word-break: break-word;
}

/* Constrain height for long code */
.code-block--scroll {
  max-height: 400px;
  overflow-y: auto;
}
```

```html
<pre class="code-block code-block--scroll"><code>// Your code here
function example() {
  return true;
}</code></pre>
```

### With File Header

```css
.code-file {
  border: 1px solid var(--border);
  border-radius: 8px;
  overflow: hidden;
}

.code-file__header {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 16px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--text-dim);
}

.code-file__body {
  font-family: var(--font-mono);
  font-size: 13px;
  line-height: 1.5;
  padding: 16px;
  background: var(--surface-elevated);
  white-space: pre-wrap;
  word-break: break-word;
  max-height: 500px;
  overflow: auto;
}
```

```html
<div class="code-file">
  <div class="code-file__header">
    <span>src/extension.ts</span>
  </div>
  <pre class="code-file__body"><code>export function activate() {
  // ...
}</code></pre>
</div>
```
