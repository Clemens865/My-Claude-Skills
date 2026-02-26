# /ui-expert — Summary

## The Problem

When people ask AI to build UI, they fall into two traps:

1. **Too vague.** "Build me a landing page" — the AI guesses everything, output is generic and unpredictable.
2. **Too manual.** Writing a 2,000-word pixel spec from scratch requires being a designer who already knows exactly what they want.

There's no guided middle path. No tool that helps you make design decisions step-by-step and translates them into a format AI can execute precisely.

## What /ui-expert Is

A Claude Code skill that acts as an interactive UI design consultant. It guides you through a structured 6-phase design process using selectable options (arrow keys, not free text) and outputs a production-ready pixel-spec prompt.

The output prompt can be fed directly into any AI code generator — v0, Lovable, Bolt, Claude Artifacts — and produces deterministic, high-fidelity results because every value is explicit.

## How It Works

### The 6-Phase Design Loop

**Phase 1: Discovery**
Establishes what you're building. Scope (page, section, component), audience, mood direction, content status. All via selectable choices.

**Phase 2: Design System Foundation**
Locks down every design token: tech stack, font + weights, color system (exact hex/rgba values), spacing scale, border radius, surface treatment (glass, solid, elevated), animation defaults. Outputs a Global Context Block.

**Phase 3: Component Architecture**
Maps the page structure top-to-bottom as a numbered component tree. Navbar, Hero, Features, Pricing, Footer — whatever the page needs. User approves, adds, removes, or reorders.

**Phase 4: Section-by-Section Specification — The Loop**
The core of the skill. For each section in the component tree:
- Asks layout decisions via selectable options (centered vs. split, solid vs. gradient bg, full viewport vs. contained)
- Drafts a complete pixel-spec for that section (every padding, font size, color, animation, hover state — all numbers)
- Presents the spec for approval
- User approves or requests adjustments
- Moves to the next section

Progress is tracked: "Section 3/7: Features Grid"

**Phase 5: Assembly**
Combines all approved sections into one complete prompt file following a proven structural template: Global Context → Component Tree → Pixel Spec → Responsive Notes.

**Phase 6: Review & Refine**
Final review with options to approve, revise specific sections (loops back to Phase 4), adjust the design system (loops back to Phase 2), or restructure (loops back to Phase 3).

### Interaction Model

Every decision point uses Claude Code's interactive selection UI — the user picks from 2-4 concrete options with arrow keys. Each option shows exact values in the description (hex codes, pixel values, font names). The user can always pick "Other" to provide custom input.

If the user gives vague input ("make it look modern"), the skill rejects it and presents concrete alternatives as selectable options instead.

## The Methodology Behind It

Based on analysis of production UI engineering prompts that consistently generate pixel-perfect frontend code. The key findings:

**What makes prompts work:**
- Zero ambiguity — every value is a number (px, hex, rgba, ms)
- Visual reading order — components described top-to-bottom, matching how code renders
- CSS-native language — layouts described using exact CSS properties
- Layered descriptions — complex elements broken into stacking layers (background → overlay → content)
- All assets upfront — font imports, media URLs, package names declared before any component

**What never appears in effective prompts:**
- Vague adjectives ("modern," "clean," "minimal," "subtle")
- References to other websites ("like Stripe's hero")
- Feeling descriptions ("should feel premium")
- Judgment calls ("add appropriate padding")

The skill enforces these rules throughout the entire design process.

## What It Outputs

A single markdown file containing a complete pixel-spec prompt. Example structure:

```
Build a [page type] for [project].
Use [font] from [source]. Tech: [stack].
Background: [hex]. Theme: [mode].

Global Design System:
- Colors: [all hex/rgba values]
- Typography: [font, weights, size scale]
- Spacing: [scale]
- Effects: [blur, radius, transitions]

[Section 1 — Navbar]:
[complete spec with every value explicit]

[Section 2 — Hero]:
[complete spec]

...

Responsive Behavior:
[breakpoint rules]
```

This prompt, when given to an AI code generator, produces deterministic output — the same prompt generates functionally identical code every time. The AI assembles; it doesn't design.

## Who It's For

- **Developers** who want intentional UI without being designers — the skill makes the design decisions interactive and concrete
- **Designers** who want to translate their vision into AI-executable specs without writing CSS by hand
- **Product managers** who need to communicate UI requirements precisely
- **Anyone** using AI code generators who wants better, more predictable output

## Key Differentiators

| Traditional Approach | /ui-expert |
|---------------------|-----------|
| Write a vague paragraph, hope for the best | Guided decisions with concrete options |
| AI interprets your intent (often wrong) | AI executes your explicit spec (deterministic) |
| Output varies wildly between runs | Same spec = same output every time |
| Requires design expertise to write a good prompt | Skill guides you through each decision |
| One-shot: write prompt, generate, start over | Iterative: approve section-by-section, refine targeted areas |

## Technical Details

- **Format:** Claude Code custom command (`.claude/commands/ui-expert.md`)
- **Dependencies:** `prompt-engineering-patterns.md` knowledge base file
- **Scope:** Global (all projects) or per-project installation
- **Invocation:** `/ui-expert [description of what to design]`
- **Tools used:** `AskUserQuestion` for interactive selections, file write for output
