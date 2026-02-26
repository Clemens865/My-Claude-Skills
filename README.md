# My Agent Skills

A collection of [Agent Skills](https://agentskills.io) and Claude Code commands for design, development, and AI workflows.

## Skills

| Skill | Description |
|-------|-------------|
| [ui-expert](skills/ui-expert/) | Interactive pixel-spec design prompt engineer — 6-phase design process with image reference support |

## Two Formats

This repo provides each skill in **two formats**:

### 1. Agent Skills Standard (`skills/`)

Follows the [agentskills.io specification](https://agentskills.io/specification) — the open standard adopted by Claude Code, Cursor, VS Code Copilot, OpenCode, Amp, and others.

```
skills/ui-expert/
  SKILL.md                              # Skill definition (agentskills.io format)
  references/
    prompt-engineering-patterns.md       # Knowledge base
  README.md                             # Documentation
  ui-expert-summary.md                  # Presentation summary
```

### 2. Claude Code Commands (`claude-code-commands/`)

Ready-to-use `/commands` for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). These are user-invocable slash commands you can install globally or per-project.

```
claude-code-commands/
  ui-expert.md                          # Drop into .claude/commands/
```

## Installation

### Agent Skills (agentskills.io)

Copy the skill directory into your project or wherever your agent implementation discovers skills:

```bash
cp -r skills/ui-expert /path/to/your/skills/
```

### Claude Code Command — Global (all projects)

```bash
mkdir -p ~/.claude/commands
cp claude-code-commands/ui-expert.md ~/.claude/commands/
```

Then update the `@` file reference inside the copied `.md` to point to the knowledge base file location. For global install, copy the reference file too:

```bash
cp skills/ui-expert/references/prompt-engineering-patterns.md ~/.claude/commands/
```

And update the `@` path in `ui-expert.md` to:
```
@~/.claude/commands/prompt-engineering-patterns.md
```

### Claude Code Command — Per-project

```bash
mkdir -p .claude/commands
cp claude-code-commands/ui-expert.md .claude/commands/
cp skills/ui-expert/references/prompt-engineering-patterns.md .claude/commands/
```

Update the `@` path in `.claude/commands/ui-expert.md` to:
```
@.claude/commands/prompt-engineering-patterns.md
```

Then use it as `/ui-expert` in Claude Code.

## What Are These?

**Agent Skills** are an open standard ([agentskills.io](https://agentskills.io)) for teaching AI agents repeatable workflows. A skill is a directory with a `SKILL.md` file containing frontmatter metadata and markdown instructions, plus optional `references/`, `scripts/`, and `assets/` subdirectories. Supported by Claude Code, Cursor, VS Code Copilot, and more.

**Claude Code Commands** are markdown files in `.claude/commands/` that Claude Code loads as user-invocable `/slash-commands`. They use a simpler frontmatter (`description`, `argument-hint`) and support `$ARGUMENTS` injection and `@file` references.

Both formats achieve the same goal — giving an AI agent structured instructions — but differ in portability and metadata.

## License

MIT
