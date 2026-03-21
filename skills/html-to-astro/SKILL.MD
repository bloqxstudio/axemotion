---
name: html-to-astro
description: >
  Converts HTML from any web source (Elementor, raw site exports, component previews, 
  live page saves) into clean, idiomatic Astro components. Use this skill whenever the 
  user uploads or pastes raw HTML and wants it converted to Astro, or says things like 
  "convert this to Astro", "implement this in Astro", "clean up this HTML into a 
  component", "I exported this from Elementor/WordPress", "turn this into a real 
  component", or "migrate this to my Astro project". Also trigger when the user uploads 
  a .html file and mentions Astro, components, or migration. This skill handles all 
  source types: Elementor page saves, Chrome "Save as HTML", Webflow exports, plain 
  HTML snippets, and Vue/React app shells.
---

# HTML → Astro Converter

Converts messy, bloated HTML (from Elementor, Webflow, Chrome saves, or any web source) 
into clean, maintainable Astro components with scoped styles, preserving all visual 
fidelity and interactions.

---

## Step 0 — Diagnose the input

Before writing any code, identify what type of HTML was provided:

| Type | Signals | What to do |
|---|---|---|
| **Elementor full page save** | `elementor-*` classes, `.e-widget-*`, `data-id`, `data-element_type` | Extract `.elementor-widget-container` subtrees |
| **Chrome "Save as HTML"** | `<!-- saved from url=... -->`, full `<html>` with `<link rel=stylesheet href=file://...>` | Extract the visual component from `<body>`, ignore shell |
| **Vue/React app shell** | `<div id="app">`, empty body, JS-rendered | ⚠️ HTML is empty — see "SPA Shell" section below |
| **Webflow export** | `.w-*` classes, `data-wf-*` attributes | Extract section/component subtrees |
| **Raw snippet / static HTML** | Just a component block with inline styles | Convert directly |
| **SuperElements preview** | `base.superelements.io` URL, Vue app with Tailwind | ⚠️ SPA Shell — see below |

---

## Step 1 — Handle SPA Shells (Elementor Preview / SuperElements / Vue apps)

If the HTML body contains `<div id="app"></div>` or similar empty containers, the 
component is **JavaScript-rendered** and the saved HTML doesn't contain visual markup.

**Tell the user:**

> "This HTML was saved from a JavaScript-rendered app — the component markup isn't in 
> the file itself (it's rendered at runtime by Vue/React). To convert this component, 
> I need the **rendered DOM**. Here's how to get it:
>
> 1. Open the page in Chrome/Firefox  
> 2. Right-click on the component → **Inspect**  
> 3. In DevTools, right-click the component's root element → **Copy → Copy outerHTML**  
> 4. Paste that HTML here
>
> Alternatively, if you can share a screenshot of the component, I can reconstruct it 
> from scratch in Astro."

Then **offer two paths**:
- A) Wait for rendered HTML → proceed to Step 2
- B) Reconstruct from screenshot or description → proceed to Step 3 (visual reconstruction)

---

## Step 2 — Extract the signal from the noise

From the HTML source, identify:

1. **The component boundary** — the outermost element that wraps just this component
2. **Visual properties to preserve:**
   - Layout (flex/grid/position)
   - Colors (extract as CSS custom properties if possible)
   - Typography (font-family, size, weight, line-height)
   - Spacing (padding, margin, gap)
   - Borders, shadows, border-radius
   - Background (color, gradient, image)
3. **Interactions to preserve:**
   - Hover states (from inline styles or `style` tags)
   - CSS transitions/animations
   - Any `data-*` attributes that drive JS behavior
4. **Assets:**
   - Image URLs (note whether they're absolute/relative/CDN)
   - Icon fonts (Font Awesome, Remixicon, etc.) — identify CDN links needed
   - Google Fonts or similar

**Strip ruthlessly:**
- All Elementor wrapper divs (`.elementor-section`, `.elementor-container`, `.elementor-column`, `.elementor-widget-wrap`)
- Grammarly integrations
- WordPress admin bar markup
- Preview mode overlays
- Empty `<style>` blocks and dead CSS
- `data-id`, `data-element_type`, `data-widget_type` attributes
- `class="elementor-*"` wrapper classes (keep only semantic/visual classes)

---

## Step 3 — Visual Reconstruction (when no rendered HTML available)

If working from a screenshot or description:

1. Identify the component type (card, feature block, hero, icon box, etc.)
2. Note layout direction, alignment, and spacing
3. Extract colors from the screenshot or ask the user for the design tokens
4. Build clean markup that matches the visual exactly

---

## Step 4 — Write the Astro Component

### File structure
```
src/components/
└── ComponentName.astro   (or .tsx if using React islands)
```

### Astro component template

```astro
---
// Props with TypeScript interface
interface Props {
  title?: string;
  description?: string;
  icon?: string;
  href?: string;
}

const { title = "Default Title", description = "", icon, href } = Astro.props;
---

<!-- Clean, semantic HTML — no framework wrappers -->
<div class="icon-box">
  {icon && <div class="icon-box__icon" set:html={icon} />}
  <div class="icon-box__content">
    {title && <h3 class="icon-box__title">{title}</h3>}
    {description && <p class="icon-box__description">{description}</p>}
    {href && <a href={href} class="icon-box__link">Learn more →</a>}
  </div>
</div>

<style>
  /* Scoped styles — no class conflicts, no !important */
  .icon-box {
    display: flex;
    gap: 1rem;
    padding: 1.5rem;
    border-radius: 12px;
    background: var(--color-surface, #fff);
    border: 1px solid var(--color-border, #e4e4e7);
    transition: box-shadow 0.2s ease, transform 0.2s ease;
  }

  .icon-box:hover {
    box-shadow: 0 8px 24px rgb(0 0 0 / 0.08);
    transform: translateY(-2px);
  }

  /* ... */
</style>
```

### Key rules for clean Astro output

1. **Scoped styles** — always use `<style>` (not `<style is:global>`) unless explicitly needed
2. **BEM-ish naming** — `block__element--modifier` keeps intent readable
3. **CSS custom properties** — extract colors/spacing as `var(--token)` for theming
4. **Props interface** — every dynamic piece of content becomes a prop
5. **No inline styles** — move all `style=""` attributes to the `<style>` block
6. **Semantic HTML** — replace `<div>` with `<article>`, `<section>`, `<nav>`, `<header>` where appropriate
7. **Preserve hover/focus states** — these matter for UX
8. **Accessibility** — add `aria-label` where needed, ensure heading hierarchy

### Handling icons

| Source | Astro approach |
|---|---|
| Font Awesome classes | Keep class-based icons, add FA CDN to layout |
| Remixicon | Keep class-based, add Remixicon CDN |
| Inline SVG | Extract as slot or prop: `<slot name="icon" />` |
| `<img>` icons | Convert to `<Image>` from `astro:assets` if local, or keep `<img>` if CDN |

---

## Step 5 — Generate usage example

Always provide a usage example after the component:

```astro
---
// src/pages/index.astro
import IconBox from '../components/IconBox.astro';
---

<IconBox 
  title="Fast Delivery"
  description="Orders delivered in under 30 minutes"
  icon='<svg>...</svg>'
  href="/delivery"
/>

<!-- Or as a grid -->
<div class="grid-3col">
  <IconBox title="Speed" description="..." />
  <IconBox title="Quality" description="..." />
  <IconBox title="Support" description="..." />
</div>
```

---

## Step 6 — Output checklist

Before presenting the final component, verify:

- [ ] No Elementor/framework wrapper divs remain
- [ ] All colors extracted to CSS custom properties or hardcoded values (not undefined variables)
- [ ] Hover states preserved
- [ ] Fonts referenced (CDN link noted if needed)
- [ ] Images have `alt` attributes
- [ ] Component accepts props for all dynamic content
- [ ] Usage example provided
- [ ] TypeScript interface defined (even if simple)

---

## Common Elementor patterns and their clean Astro equivalents

### Elementor Icon Box widget
```html
<!-- Elementor bloat -->
<div class="elementor-widget-wrap elementor-element-populated">
  <div class="elementor-element elementor-widget elementor-widget-icon-box" data-id="abc123" data-element_type="widget" data-widget_type="icon-box.default">
    <div class="elementor-widget-container">
      <div class="elementor-icon-box-wrapper">
        <div class="elementor-icon-box-icon">
          <span class="elementor-icon"><i class="fa fa-star"></i></span>
        </div>
        <div class="elementor-icon-box-content">
          <h3 class="elementor-icon-box-title"><span>Feature Title</span></h3>
          <p class="elementor-icon-box-description">Description text here.</p>
        </div>
      </div>
    </div>
  </div>
</div>
```

```astro
<!-- Clean Astro -->
<div class="icon-box">
  <div class="icon-box__icon">
    <i class="fa fa-star" aria-hidden="true"></i>
  </div>
  <div class="icon-box__content">
    <h3 class="icon-box__title">{title}</h3>
    <p class="icon-box__description">{description}</p>
  </div>
</div>
```

---

## Animating in Astro

If the original had entrance animations (Elementor Animate.css, AOS, custom), convert to:

```astro
<style>
  .icon-box {
    /* Modern CSS entrance animation */
    animation: fadeInUp 0.4s ease both;
    animation-delay: var(--delay, 0ms);
  }

  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(12px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
</style>
```

For scroll-triggered animations, suggest:
```astro
<!-- Native Intersection Observer via Astro script -->
<script>
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(e => e.target.classList.toggle('in-view', e.isIntersecting));
  });
  document.querySelectorAll('.animate-on-scroll').forEach(el => observer.observe(el));
</script>
```

---

## Notes on SuperElements specifically

SuperElements preview pages (`base.superelements.io`) are Vue 3 SPAs. The preview 
HTML saved from the browser will **always** be an empty app shell. To convert a 
SuperElements component to Astro:

1. Open the component preview in the browser
2. Use DevTools to copy the rendered component HTML (right-click → Copy outerHTML on the component root)
3. Also note any custom CSS variables from the `:root` style in DevTools
4. Paste both here — this skill will extract and convert them cleanly

The actual components use Tailwind utility classes — these can either be:
- **Kept as-is** (if the Astro project uses Tailwind)
- **Converted to semantic CSS** (if the project uses scoped styles)

Ask the user which approach they prefer.