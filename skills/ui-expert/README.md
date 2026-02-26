# ui-expert — Pixel-Spec Design Prompt Engineer

An [Agent Skill](https://agentskills.io) that acts as an interactive UI design consultant, guiding you through a structured design process and outputting production-ready pixel-spec prompts.

Also available as a Claude Code `/command` — see [`claude-code-commands/ui-expert.md`](../../claude-code-commands/ui-expert.md).

## What It Does

Instead of writing vague design descriptions and hoping for the best, `ui-expert` walks you through a 6-phase interactive design process — making every decision explicit, every value a number, and every output deterministic.

The result: a complete pixel-spec prompt file you can feed to any AI code generator (v0, Lovable, Bolt, Claude Artifacts) and get exactly the UI you designed.

## The 6-Phase Design Loop

| Phase | Name | What Happens |
|-------|------|-------------|
| 1 | **Discovery** | Scope, audience, mood, content status, optional reference image |
| 2 | **Design System** | Tech stack, fonts, colors (from presets, manual, or image extraction), spacing, effects |
| 3 | **Component Architecture** | Page structure mapped top-to-bottom |
| 4 | **Section Spec (The Loop)** | Each section specified pixel-by-pixel with approval gates and optional image references |
| 5 | **Assembly** | All sections combined into one prompt file |
| 6 | **Review & Refine** | Targeted revisions, loop back to any phase |

Every decision is presented as **selectable options** (arrow keys) — not free-text input. You pick from concrete choices with exact values shown.

## What Makes It Different

- **Zero vague adjectives.** The words "modern," "clean," "minimal," "subtle" are banned. Everything is a number: `px`, `rem`, `hex`, `rgba`, `ms`.
- **Interactive selections.** Every decision point presents 2-4 concrete options to pick from, not open-ended questions.
- **Image reference support.** Provide screenshots, mockups, or brand assets at any phase — the skill extracts colors, typography, layout, and effects as concrete values.
- **Section-by-section loop.** Each section gets its own approval gate. Approve and move on, or revise before continuing.
- **Anti-pattern detector.** If you say something vague, it won't accept it — it'll present concrete alternatives to choose from.
- **Based on proven methodology.** Built on analysis of production UI engineering prompts that consistently produce high-fidelity output.

## Image References

You can provide reference images at three key points:

| When | What It Does |
|------|-------------|
| **Phase 1 (Discovery)** | Analyzes a screenshot or mockup upfront — extracts colors, fonts, layout patterns, effects as a starting point for the entire design |
| **Phase 2 (Design System)** | Extracts a color palette from a brand asset or screenshot instead of picking from presets |
| **Phase 4 (Per-section)** | Analyzes a section-specific reference and drafts the pixel-spec directly from it |

The skill reads the image, identifies concrete values (hex colors, approximate spacing, font characteristics, border radius, effects), and presents them as selectable options for you to confirm, adjust, or ignore.

## Directory Structure (agentskills.io)

```
ui-expert/
  SKILL.md                                  # Skill definition
  references/
    prompt-engineering-patterns.md           # Knowledge base — methodology and patterns
  README.md                                 # This file
  ui-expert-summary.md                      # Presentation-ready summary
```

## Usage in Claude Code

If installed as a Claude Code command (`/ui-expert`):

```bash
# Full page design session
/ui-expert Landing page for a SaaS product

# Design from existing content
/ui-expert Create the homepage based on @content/homepage.md

# Single component
/ui-expert Design a glassmorphic testimonial card

# Multi-page site
/ui-expert Marketing site for an AI consulting business
```

## Example Flow

```
> /ui-expert Landing page for a developer tool

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Phase 1/6: Discovery
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

? Do you have a reference image to start from?
  > No reference       — Start from scratch — I'll make choices as we go
    Yes, I have one    — I'll provide a file path or drag an image in
    Multiple refs      — I have several images for different aspects

? What are we building?
  > Full page          — Complete single page (hero, sections, footer)
    Multi-page site    — Multiple connected pages with shared nav/footer
    Section            — One section or block within a page
    Component          — Single reusable component (card, form, modal)

? What's the mood direction?
    Dark + dramatic    — Black bg (#0a0a0a), glass effects, glows, high contrast
  > Light + airy       — White bg (#ffffff), soft shadows, muted colors
    Bold + saturated   — Vivid accent colors, strong contrast, punchy
    Neutral            — Gray tones (#f5f5f5/#1a1a1a), clean lines
```

## Output

The skill produces a complete pixel-spec prompt file:

```markdown
Build a landing page for [project].
Use Inter (from Google Fonts, weights 400/500/600/700). Tech: React, Tailwind, TypeScript, framer-motion.
Background: #050505. Theme: dark.

Global Design System:
- Colors: bg #050505, text #ffffff, accent #7c3aed, surface #0a0a0a, border rgba(255,255,255,0.08)
- Typography: Inter, sizes 14/16/18/24/32/48/64px
- Spacing: 4px base scale
- Effects: backdrop-blur(24px), border-radius 8/12/16px, transition 300ms ease-out

Navbar:
- Fixed top, max-width 1200px centered, h-16, px-6, flex row items-center justify-between
...
```

## Background

Based on the analysis in [`references/prompt-engineering-patterns.md`](references/prompt-engineering-patterns.md), which documents the patterns found in production UI engineering prompts that consistently generate pixel-perfect frontend code. The key insight: these prompts are **executable design specs disguised as natural language** — the AI's job is assembly, not design.

The `ui-expert` skill turns this methodology into an interactive, guided process accessible to anyone — not just people who already think in CSS.
