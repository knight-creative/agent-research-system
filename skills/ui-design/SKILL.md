---
name: ui-design
description: Reference guide for designing production-grade user interfaces across websites, portals, and apps. Covers typography, color, spacing, motion, visual depth, component patterns, accessibility, performance, and current CSS capabilities. Invoke when making UI decisions, starting a new design, or reviewing interface quality.
allowed-tools:
  - Read
---

# /ui-design

Production-grade UI design standards. Apply these across every interface you build: websites, portals, apps, anything the user touches.

**Baseline:** Apple.com. Every element earns its place. No decoration without purpose.

---

## Typography

### Font Selection

Max 2 typefaces: one display, one body. Variable fonts where available.

**Display options** (headlines, hero text, section titles):
- Geist: clean, modern, developer aesthetic
- Bricolage Grotesque: editorial, distinctive, high personality
- Syne: geometric, bold, tech-forward
- Plus Jakarta Sans: versatile, warm geometric
- Bebas Neue: condensed, high-impact, uppercase-only
- Cal Sans: friendly, rounded, approachable

**Body options** (paragraphs, UI labels, captions):
- Inter: neutral, readable, universal
- DM Sans: slightly warmer than Inter, great for SaaS
- Outfit: modern, clean, slightly geometric

### Type Scale

Define all sizes as tokens. Never one-off values.

```css
--text-xs:   0.75rem;   /* 12px */
--text-sm:   0.875rem;  /* 14px */
--text-base: 1rem;      /* 16px */
--text-lg:   1.125rem;  /* 18px */
--text-xl:   1.5rem;    /* 24px */
--text-2xl:  2rem;      /* 32px */
--text-3xl:  3rem;      /* 48px */
--text-4xl:  4rem;      /* 64px */
--text-5xl:  5rem;      /* 80px */
--text-6xl:  6rem;      /* 96px */
```

For fluid type that scales with viewport:
```css
--text-display: clamp(3rem, 6vw + 1rem, 6rem);
```

### Line Height and Spacing

- Headlines: `line-height: 1.0` to `1.15`: tight
- Subheadings: `line-height: 1.2` to `1.35`
- Body text: `line-height: 1.6` to `1.75`
- Headlines: `letter-spacing: -0.02em` to `-0.04em`: slightly tighter than default
- Body: `letter-spacing: 0`: never track out body text

### Text Wrapping

```css
h1, h2, h3 { text-wrap: balance; }
p, li       { text-wrap: pretty; }
```

`balance` prevents awkward single-word orphans on headings. `pretty` avoids lonely words on the last line of paragraphs.

### Font Loading

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```

Always use `display=swap` to prevent invisible text during load. Define `size-adjust`, `ascent-override`, `descent-override` on the fallback font to minimize layout shift.

---

## Color

### Architecture

Every project gets:
- 1 brand accent (the primary action color)
- 9-step neutral scale (near-black to near-white, never pure #000 or #fff)
- Semantic set: success, warning, error, info
- Dark mode variants for every color

### Neutral Scale (HSL)

```css
--neutral-950: hsl(220 15% 5%);    /* near-black background */
--neutral-900: hsl(220 13% 9%);
--neutral-800: hsl(220 10% 14%);
--neutral-700: hsl(220 8% 22%);
--neutral-600: hsl(220 6% 35%);
--neutral-500: hsl(220 5% 50%);    /* mid-gray */
--neutral-400: hsl(220 5% 65%);
--neutral-300: hsl(220 6% 78%);
--neutral-200: hsl(220 8% 88%);
--neutral-100: hsl(220 10% 94%);
--neutral-50:  hsl(220 13% 97%);   /* near-white background */
```

### Color Rules

- Never `#000000` or `#ffffff`: near-blacks and off-whites only
- One accent color per interface. Don't mix brand accents on the same surface.
- WCAG AA minimum: 4.5:1 for body text, 3:1 for large text and UI components
- Dark mode: define in `@media (prefers-color-scheme: dark)` OR via `[data-theme="dark"]` attribute

---

## Spacing

8px base grid. All spacing values are multiples of 4 or 8.

```css
--space-1:  0.25rem;   /* 4px  */
--space-2:  0.5rem;    /* 8px  */
--space-3:  0.75rem;   /* 12px */
--space-4:  1rem;      /* 16px */
--space-6:  1.5rem;    /* 24px */
--space-8:  2rem;      /* 32px */
--space-12: 3rem;      /* 48px */
--space-16: 4rem;      /* 64px */
--space-24: 6rem;      /* 96px */
```

Page containers: `max-width: 1280px`, centered, with `padding-inline: clamp(1rem, 4vw, 2rem)`.

Section padding: `padding-block: clamp(4rem, 8vw, 8rem)` for hero/feature sections.

---

## Motion

### Timing Functions

```css
--ease-out-expo:   cubic-bezier(0.16, 1, 0.3, 1);   /* entrance: snappy start, floats to rest */
--ease-in-expo:    cubic-bezier(0.7, 0, 0.84, 0);    /* exit: stays, then accelerates away */
--ease-in-out:     cubic-bezier(0.4, 0, 0.2, 1);     /* state changes */
--spring:          cubic-bezier(0.22, 1, 0.36, 1);   /* interactive elements */
```

### Duration Guidelines

- Hover / micro-interactions: 120ms to 200ms
- State changes (color, opacity): 200ms to 300ms
- Entrance animations: 400ms to 700ms
- Page transitions: 300ms to 500ms
- Never animate longer than 700ms unless there is a deliberate dramatic reason

### Stagger

When animating a list or grid of items, stagger each item by 50ms to 80ms. Items should share the same animation but start at slightly different times.

### What to Animate (safe)

Always animate these properties: they use the GPU and don't cause layout reflow:
- `opacity`
- `transform` (translate, scale, rotate)
- `filter` (blur, brightness)
- `clip-path`

Never animate (causes reflow, janky):
- `width`, `height`
- `top`, `left`, `right`, `bottom`
- `padding`, `margin`
- `font-size`

### Framer Motion (React)

Standard entrance:
```jsx
initial={{ opacity: 0, y: 20 }}
animate={{ opacity: 1, y: 0 }}
transition={{ duration: 0.5, ease: [0.22, 1, 0.36, 1] }}
```

Stagger children:
```jsx
// Parent
transition={{ staggerChildren: 0.06 }}

// Each child
variants={{ hidden: { opacity: 0, y: 16 }, visible: { opacity: 1, y: 0 } }}
```

### Reduced Motion

Always respect user preference:
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

In Framer Motion: use `useReducedMotion()` hook and set duration to 0 when true.

---

## Visual Depth

### Elevation Model

Elements at higher elevation should feel lifted: but subtly.

```css
--shadow-sm: 0 1px 2px hsl(0 0% 0% / 0.06);
--shadow-md: 0 4px 12px hsl(0 0% 0% / 0.08), 0 1px 3px hsl(0 0% 0% / 0.05);
--shadow-lg: 0 12px 32px hsl(0 0% 0% / 0.10), 0 2px 6px hsl(0 0% 0% / 0.06);
--shadow-xl: 0 24px 64px hsl(0 0% 0% / 0.14), 0 4px 12px hsl(0 0% 0% / 0.08);
```

**Use shadows for:** modals, dropdowns, floating elements, cards that need lift.
**Do not use shadows for:** decorative effect, adding "depth" everywhere, replacing borders.

### Border Radius

Consistent scale: pick one and apply it everywhere:

```css
--radius-sm:  4px;
--radius-md:  8px;
--radius-lg:  12px;
--radius-xl:  16px;
--radius-2xl: 24px;
--radius-full: 9999px;  /* pills, tags */
```

Cards typically use `--radius-xl` or `--radius-2xl`. Buttons match their size (sm button = sm radius).

### Layering (z-index)

```css
--z-base:     0;
--z-raised:   10;    /* sticky headers, floating buttons */
--z-dropdown: 20;    /* dropdowns, popovers */
--z-sticky:   30;    /* sticky sidebars */
--z-overlay:  40;    /* modal backdrops */
--z-modal:    50;    /* modal dialogs */
--z-toast:    60;    /* notifications */
--z-tooltip:  70;    /* tooltips: always on top */
```

---

## Component Patterns

### Buttons

Every button needs: default, hover, active (pressed), disabled, and loading states.

```css
/* Base: always visible focus ring */
button:focus-visible {
  outline: 2px solid var(--color-brand);
  outline-offset: 2px;
}
```

Sizes: sm (32px height), md (40px), lg (48px). Padding scales with height.

Never use `cursor: pointer` on disabled buttons. Never disable buttons without a reason visible to the user.

### Forms

- Label above the input, always: never floating labels (accessibility and readability)
- Error state: red border + red error text below the input: not just color alone
- Success state: green border or checkmark: never silent
- Required fields: asterisk (*) next to the label with a legend explaining it
- Placeholder: hint text only, never the label: it disappears when the user types

### Cards

Cards are contained units. Every card needs:
- Clear visual boundary (border or background differentiation)
- Consistent padding
- Semantic heading hierarchy (h2, h3: never skip levels)
- A primary action (button, link, or the entire card is clickable)

If the whole card is clickable: use a single `<a>` wrapping the card, not nested clickable elements.

### Navigation

- Mobile: hamburger or bottom nav. Test keyboard navigation.
- Always visible focus indicators
- Current page/section clearly indicated
- Skip-to-main-content link as the first element for keyboard users

---

## Current CSS Features (Use These)

### CSS Grid and Subgrid

```css
/* Main layout */
.layout {
  display: grid;
  grid-template-columns: 1fr min(65ch, 100%) 1fr;
}

/* Subgrid: align nested elements to parent grid */
.card-grid {
  display: grid;
  grid-template-rows: subgrid;
}
```

### CSS Custom Properties for Theming

```css
:root {
  --color-bg: var(--neutral-50);
  --color-fg: var(--neutral-950);
}

[data-theme="dark"] {
  --color-bg: var(--neutral-950);
  --color-fg: var(--neutral-50);
}
```

### @layer for Specificity

```css
@layer base, components, utilities;

@layer base { /* reset, defaults */ }
@layer components { /* reusable patterns */ }
@layer utilities { /* one-off overrides */ }
```

### Container Queries

```css
.card-container {
  container-type: inline-size;
}

@container (min-width: 400px) {
  .card { flex-direction: row; }
}
```

Use for component-level responsiveness: the component responds to its container, not the viewport.

### :has() Selector

```css
/* Style a form when it contains an error */
.field:has(.error-message) input {
  border-color: var(--color-error);
}

/* Style a nav when a dropdown is open */
nav:has([aria-expanded="true"]) { background: var(--color-bg-raised); }
```

### Fluid Spacing with clamp()

```css
/* Scales smoothly between 320px and 1280px viewport */
padding: clamp(1.5rem, 4vw, 3rem);
font-size: clamp(1rem, 2.5vw, 1.25rem);
```

### View Transitions API

```js
document.startViewTransition(() => {
  updateDOMFunction();
});
```

```css
::view-transition-old(root) {
  animation: fade-out 200ms ease-in;
}
::view-transition-new(root) {
  animation: fade-in 300ms ease-out;
}
```

For Next.js: use the `next-view-transitions` package or App Router's built-in navigation.

---

## Tailwind v4 Patterns

```css
/* @theme replaces tailwind.config.js in v4 */
@import "tailwindcss";

@theme {
  --color-brand: oklch(0.65 0.2 250);
  --font-display: "Bricolage Grotesque", sans-serif;
  --radius-card: 1rem;
}
```

Key v4 differences:
- Config is CSS-first, not JS
- Use `@theme` block to define custom values
- Custom variants with `@variant`
- `@apply` still works but prefer utility classes

---

## Accessibility

- **Semantic HTML first.** Use `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<button>`: not div soup.
- **ARIA only when semantic HTML isn't enough.** `role="button"` on a div is wrong: just use `<button>`.
- **Keyboard navigation:** every interactive element must be Tab-reachable and have a visible focus state.
- **Skip link:** first focusable element should be `<a href="#main">Skip to main content</a>`, visible on focus.
- **Modals:** trap focus inside when open. Return focus to the trigger when closed. Escape closes.
- **Images:** `alt` text on all meaningful images. Decorative images get `alt=""`.
- **Color alone:** never use color as the only indicator of state (error, success, disabled). Always pair with text or icon.
- **Minimum touch target:** 44x44px for anything tappable on mobile.

---

## Performance

- **Core Web Vitals targets:** LCP < 2.5s, INP < 200ms, CLS < 0.1
- **Images:** use `next/image` (auto-optimization, lazy load, srcset). Always set `width` and `height` to prevent layout shift.
- **Fonts:** preconnect + `display=swap`. Subset if using a large font. Self-host if performance-critical.
- **JavaScript:** code-split by route. Lazy-load components below the fold with `dynamic()` in Next.js.
- **Animation:** use `will-change: transform` only immediately before an animation: not permanently.
- **CSS:** no unused styles. Tailwind v4 purges automatically. Extract critical CSS for the above-the-fold content.

---

## What "Apple Baseline" Actually Means

1. **Generous whitespace.** Let elements breathe. Cramped layouts signal low quality.
2. **One focal point per section.** The eye should know exactly where to go.
3. **Perfect alignment.** Nothing feels off-center or visually uncomfortable.
4. **Every detail is intentional.** No border, shadow, or color that wasn't chosen deliberately.
5. **Smooth interactions.** Every button press, hover, and transition has a response. Nothing feels static.
6. **Copy is part of the design.** Font choice, size, and spacing for text is as considered as anything visual.
7. **Consistent rhythm.** Spacing, sizing, and motion feel like they belong to the same system.
8. **Never ship broken.** Nothing ships with a layout bug, a missing state, or a broken mobile view.
