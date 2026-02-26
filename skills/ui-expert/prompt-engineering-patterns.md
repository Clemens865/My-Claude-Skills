# UI-Agentic Engineering Prompt Patterns

Analysis of high-fidelity UI engineering prompts used to generate production-ready frontend code.

---

## The Core Pattern

Every prompt follows the same skeleton:

```
1. GLOBAL CONTEXT    → Tech stack, theme, fonts, design system
2. COMPONENT TREE    → Section-by-section, top to bottom
3. PIXEL SPEC        → Every value explicit: px, hex, opacity, duration
4. ASSET LINKS       → Direct CDN URLs for videos/images
5. RESPONSIVE NOTES  → Breakpoint behavior
```

The prompts read like a **design-to-code handoff spec** — not a creative brief. They leave zero ambiguity. The AI has nothing to interpret, only to execute.

---

## Pattern Breakdown

### 1. Global Context Block (Always First)

Every prompt opens by establishing the full environment before any component is described.

**What's always declared:**
- Tech stack (React, Tailwind, TypeScript, framer-motion, specific packages)
- Font family + import source (Google Fonts, Fontshare)
- Color system as CSS variables or hex values
- Theme mode (dark-only in every example)
- Background color (always explicit, usually `#000`)

**Example structure:**
```
Build a [type] for [brand/project].
Use [font] (from [source]).
The [scope] has a [color] background with [media element].
Tech: [React, Tailwind, TypeScript, specific packages].
```

**Key insight:** The global block constrains the entire generation. No decisions left for the AI about theme, stack, or base aesthetics.

---

### 2. Component Tree (Top-Down, Sequential)

Every prompt describes components in **visual reading order** — exactly as a user would scan the page:

```
Navbar → Hero → Supporting Sections → CTA → Footer
```

Each component gets its own labeled block with a clear heading (`Navbar:`, `Hero Section:`, `Slide 3 — Analytics Slide:`).

**Naming convention:** Either functional (`Navbar`, `Hero Content`) or semantic with file names (`CoverSlide.tsx`, `AnalyticsSlide.tsx`).

**Key insight:** The prompt IS the component tree. The AI maps each block to a component/section in the output code.

---

### 3. Spatial Precision (The Signature Move)

This is what separates these prompts from casual design descriptions. Every spatial relationship is numerically specified:

| Property | How It's Specified |
|----------|-------------------|
| Padding | `px-14 py-4`, `120px horizontal`, `padding 36px` |
| Margins | `mt 1.5%`, `top margin 162px`, `-my-24` |
| Gaps | `gap 60px`, `30px apart`, `24px gap` |
| Sizes | `187px wide × 25px tall`, `801px × 384px`, `6px circles` |
| Font size | `22px`, `clamp(2rem, 5vw, 68px)`, `14px` |
| Font weight | `medium weight`, `font-medium`, `semibold`, `weight 450` |
| Line height | `22px`, `1.28`, `1.05` |
| Letter spacing | `-2px`, `-0.02em`, `tracking -0.02em` |
| Border radius | `9999px`, `8px`, `40px`, `rounded-full` |
| Border | `0.6px solid white`, `1px #d4d4d4`, `1.5px transparent` |
| Opacity | `opacity-80`, `white at 70%`, `rgba(255,255,255,0.08)` |
| Blur | `blur(24px)`, `blur(77.5px)`, `backdrop-blur 10px` |
| Z-index | `lowest z-index`, `z-index: 1`, `z-index: 2` |

**Key insight:** Nothing is described as "large," "small," "subtle," or "prominent." Everything has a number. The prompt author has already made every design decision.

---

### 4. Color Specification

Colors are always exact — never named loosely.

**Formats used:**
- Hex: `#000000`, `#A6A4FF`, `#7b39fc`, `#2b2344`
- RGBA: `rgba(255,255,255,0.08)`, `rgba(23,23,23,0.04)`
- HSL (CSS variables): `0 0% 0%`, `0 0% 100%`
- Tailwind shorthand: `bg-black/50`, `white/90`, `white/30`

**Opacity is always explicit:** Not "slightly transparent" but `opacity-90`, `60% opacity`, `rgba(255,255,255,0.12)`.

**Gradients are mathematically specified:** `linear-gradient at ~144.5 degrees from solid white (at ~28%) to transparent black (at ~115%)`.

---

### 5. Typography System

Fonts are declared with implementation-level detail:

```
Font: [Name] (import from [Source]: [exact URL])
Weights used: 400, 500, 700
```

Every text element specifies:
- Font family (when mixed fonts are used, e.g., Inter + Instrument Serif)
- Weight (numeric or keyword)
- Size (px or clamp)
- Line height
- Letter spacing
- Color/opacity
- Max width (for readability constraining)

**Responsive font pattern:** `clamp(min, preferred, max)` is used consistently for responsive typography (e.g., `clamp(2rem, 5vw, 68px)`).

---

### 6. Media Elements

Videos are described with full playback configuration:

```
Auto-playing, looped, muted, playsInline video
URL: [direct CDN link]
Sizing: [w-full h-auto / object-cover / object-contain]
Styling: [border radius, negative margins, overlay treatment]
```

**Video overlay pattern:** Background video → semi-transparent overlay → content on top. Always with explicit z-index stacking.

**Advanced video:** HLS streaming with `hls.js` implementation details (enableWorker, MANIFEST_PARSED event, Safari fallback).

---

### 7. Animation Specifications

Animations are described with timing precision:

| Property | Example |
|----------|---------|
| Duration | `0.8s`, `500ms`, `20s` |
| Easing | `ease-in-out`, `linear` |
| Delay | `0.3s delay` |
| Type | `fade+slide down`, `translateX(0) to translateX(-50%)` |
| Library | `framer-motion` or CSS keyframes |
| Behavior | `infinite`, `auto-hiding after 3 seconds` |

**Marquee pattern:** Duplicate content, CSS translateX animation, `linear infinite`. Appears in multiple prompts.

---

### 8. Interactive Element Construction

Buttons are described as **layered constructions**, not simple elements:

```
Outer: pill shape, 0.6px white border
Inner: black-background pill
Text: white, 14px, font-medium
Effect: white glow streak along top edge (blurred gradient)
Padding: 29px horizontal, 11px vertical
```

This "construction" language signals that the element has depth — multiple DOM layers, not just styled text.

---

### 9. Layout Architecture

Layout is described using CSS-native language:

- `Flexbox row, space-between`
- `Three-column layout (flex 0 0 22% / flex 0 0 38% / flex 0 0 20%)`
- `Vertically centered, nudged up 3%`
- `Max width: 1440px, centered horizontally`
- `Absolute positioned, horizontally centered, top offset ~215px`

**Key insight:** The prompt author thinks in CSS. They describe layouts using the exact properties the AI will generate.

---

### 10. Glassmorphism / "Liquid Glass" Pattern

A recurring visual pattern described with precision:

```
backdrop-filter: blur(24px) saturate(1.4)
background: linear-gradient(rgba(255,255,255,0.08) to rgba(255,255,255,0.03))
border: 1px solid rgba(255,255,255,0.12)
radial specular highlight at top-left
```

This appears across multiple prompts as the default card/container treatment.

---

## Structural Template

Based on all examples, this is the prompt skeleton:

```markdown
Build a [component/page type] for [project/brand].
Use [font] (from [source]). Tech: [stack].
Background: [color]. Theme: [dark/light].

[Package installations if needed]

Global Design System:
- Colors: [exact values]
- Typography: [font, weights, sizes]
- Spacing: [system description]
- Effects: [blur, glass, shadows]

[Component 1 — Name]:
- Position/layout: [exact CSS description]
- Left/top: [child elements with full specs]
- Center: [child elements with full specs]
- Right/bottom: [child elements with full specs]
- Responsive: [breakpoint behavior]

[Component 2 — Name]:
...

[Component N — Name]:
...
```

---

## What Makes These Prompts Work

| Principle | Description |
|-----------|-------------|
| **Zero Ambiguity** | Every value is a number. No "make it look nice." |
| **Visual Reading Order** | Components described top→bottom, left→right, matching how code renders |
| **CSS-Native Language** | Uses Tailwind classes, CSS property names, and layout terminology directly |
| **Layered Descriptions** | Complex elements broken into stacking layers (background → overlay → content) |
| **Asset-Ready** | All media URLs, font imports, and package names provided upfront |
| **Implementation Hints** | File names (`CoverSlide.tsx`), component structure, and wiring instructions included |
| **Responsive Baked In** | Breakpoints and scaling (clamp, percentage-based) woven throughout, not bolted on |

---

## Anti-Patterns NOT Present

These prompts never:
- Use vague adjectives ("modern," "clean," "minimal")
- Reference other websites ("like Stripe's hero")
- Describe feelings ("should feel premium")
- Leave spacing to judgment ("add appropriate padding")
- Mix concerns (layout + content + animation described together)
- Omit units or specificity

---

## Summary

These prompts are **executable design specs disguised as natural language**. The author has already completed the design process — font selection, spacing system, color palette, animation timing, responsive strategy — and is using the prompt purely as a translation layer from visual intention to code.

The AI's job is assembly, not design. That's why they work.
