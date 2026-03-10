# Swarm Simulator

A Claude Code skill that runs multi-agent swarm simulations for scenario prediction. Describe any scenario — pricing strategy, product launch, policy change, market entry — and get a research-grounded simulation with an interactive HTML dashboard report.

## What It Does

1. **Researches** the scenario via web search (market data, precedents, stakeholder positions)
2. **Designs** 3-5 agent populations (40-100 agents) with distinct personalities and goals
3. **Simulates** 5 rounds of agent decisions with evolving world state
4. **Detects** emergent behaviors — unexpected patterns no single agent would predict
5. **Generates** a beautiful dark-themed HTML dashboard with charts, findings, and recommendations

## No API Keys Required

The skill runs entirely within Claude Code — Claude itself acts as the simulation engine. No external API calls, no infrastructure, no costs beyond the conversation.

## Example Usage

```
/simulate "We're removing our SaaS free tier. 5,000 free users, 800 paid at $20/mo. What happens?"
```

```
/simulate "What happens if Apple releases a $99 iPhone with full AI features?"
```

```
/simulate "We're a German datacenter provider competing with Hetzner and AWS. What's the optimal pricing strategy for EU AI Act compliance?"
```

## Output

The skill produces:

- **Research Summary** — real-world data grounding the simulation
- **Round-by-round updates** — sentiment, stances, and emergent events as they happen
- **Interactive HTML Dashboard** (`/tmp/swarm-report.html`) featuring:
  - Executive summary answering the original question
  - Key findings with confidence badges (high/medium/low)
  - SVG sentiment chart tracking positive/neutral/negative over 5 rounds
  - Stance distribution bar chart
  - Emergent behavior cards
  - Collapsible round-by-round timeline
  - Agent spotlight stories
  - Actionable recommendations
  - Research sources

## Installation

### Claude Code — Global (all projects)

```bash
mkdir -p ~/.claude/commands
cp simulate.md ~/.claude/commands/
```

Or clone and copy from this repo:

```bash
git clone https://github.com/Clemens865/My-Claude-Skills.git
cp My-Claude-Skills/claude-code-commands/simulate.md ~/.claude/commands/
```

### Claude Code — Per-project

```bash
mkdir -p .claude/commands
cp simulate.md .claude/commands/
```

Then use `/simulate <your scenario>` in Claude Code.

### Agent Skills (agentskills.io)

```bash
cp -r skills/swarm-simulator /path/to/your/skills/
```

## How It Works

The simulation follows a multi-agent behavioral modeling approach inspired by [MiroFish](https://github.com/mirofish) and swarm intelligence research:

1. **Population Design** — Each population represents a stakeholder group with shared traits but individual variation (low/medium/high diversity within groups)
2. **Round Execution** — Each round, every agent observes the world state, makes a decision, and takes a stance. Agents react to previous rounds — opinions shift, alliances form, some agents change sides
3. **Emergent Detection** — The system watches for sentiment shifts >15%, cross-population consensus, unexpected solidarity, and other emergent patterns
4. **Report Generation** — A structured analysis synthesizes all rounds into actionable findings

## Example Scenarios

| Domain | Example Prompt |
|--------|---------------|
| **Pricing** | "Should we raise prices 20% on our B2B SaaS?" |
| **Product** | "How will users react if we remove feature X?" |
| **Market Entry** | "What happens if we launch a Notion competitor in Europe?" |
| **Policy** | "What are second-order effects of banning algorithmic pricing?" |
| **Competition** | "How will the market respond if our main competitor gets acquired?" |
| **Public Opinion** | "How will the developer community react to our new licensing model?" |

## License

MIT
