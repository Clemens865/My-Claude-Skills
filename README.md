# My Claude Skills

A collection of custom [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills (slash commands) for design, development, and AI workflows.

## Skills

| Skill | Command | Description |
|-------|---------|-------------|
| [UI Expert](skills/ui-expert/) | `/ui-expert` | Interactive pixel-spec design prompt engineer — guides you through a 6-phase design process and outputs production-ready prompts for AI code generators |

## What Are Claude Code Skills?

Skills are custom slash commands for Claude Code. They're markdown files placed in `.claude/commands/` that define a persona, methodology, and interaction pattern. When you type `/skill-name` in Claude Code, the skill activates and guides the conversation.

**Project-level:** `.claude/commands/skill-name.md` — available in that project only.
**Global:** `~/.claude/commands/skill-name.md` — available in every project.

## Quick Install

### Install a single skill globally

```bash
# Clone the repo
git clone https://github.com/Clemens865/My-Claude-Skills.git
cd My-Claude-Skills

# Copy skill files to global commands
mkdir -p ~/.claude/commands
cp skills/ui-expert/ui-expert.md ~/.claude/commands/
cp skills/ui-expert/prompt-engineering-patterns.md ~/.claude/commands/
```

Then update the `@` file reference inside the copied `.md` to point to the correct location of any knowledge base files.

### Install into a specific project

```bash
# From your project root
mkdir -p .claude/commands
cp path/to/My-Claude-Skills/skills/ui-expert/ui-expert.md .claude/commands/
cp path/to/My-Claude-Skills/skills/ui-expert/prompt-engineering-patterns.md .claude/commands/
```

## License

MIT
