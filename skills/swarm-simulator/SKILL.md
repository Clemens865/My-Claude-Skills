---
name: swarm-simulator
description: "Multi-agent swarm simulation with REAL independent subagents for scenario prediction. Each population runs as a separate agent process that cannot see other populations' decisions — producing genuinely emergent behavior. Spawns 40-100 AI agents across 3-5 populations, researches via web search, runs 5 rounds with parallel execution, and delivers an interactive HTML dashboard. Use this skill when the user wants to predict outcomes, simulate scenarios, test strategies, forecast reactions, model market dynamics, or run what-if analyses. Triggers on: '/simulate', 'simulate', 'swarm simulation', 'what would happen if', 'predict the outcome', 'run a scenario', 'agent simulation', 'market simulation', 'how would people react'."
argument-hint: "<scenario description>"
---


Run a multi-agent swarm simulation directly in Claude Code. No API keys needed. Uses **real independent subagents** — each population is simulated by a separate agent process that cannot see what other populations decided. Generates a beautiful HTML dashboard.

**Usage:** `/simulate <scenario description>`

## Instructions

You are the **orchestrator** of a multi-agent swarm simulation. You coordinate the simulation, but each population is run by an independent subagent via the Agent tool. This produces genuinely independent reasoning — not one mind imagining all agents.

The user's scenario: `$ARGUMENTS`

Follow these steps exactly:

### Step 0: Research the Scenario (parallel)

Launch 3-5 **research subagents in parallel** using the Agent tool. Each subagent researches a different angle:

- Agent 1: Market data, industry statistics, market size
- Agent 2: Historical precedents — similar scenarios and their outcomes
- Agent 3: Key stakeholders, competitors, and their known positions
- Agent 4: Recent news and developments related to the topic
- Agent 5: Regulatory, legal, or policy context if relevant

Each research agent should use WebSearch and return a structured summary with sources.

**Spawn all research agents in a single message** so they run concurrently. When they complete, synthesize their findings into a brief **Research Summary** for the user.

### Step 1: Design Agent Populations (informed by research)

Based on the research, design 3-5 agent populations. For each define:
- **Name**, **Count** (10-30 each, 40-100 total), **Personality**, **Goals**, **Behavioral bias**, **Variation** (low/medium/high)

Also define: **Environment rules**, **Output goal** (key question to answer), **Rounds** (use 5).

Show the user a summary table of populations before proceeding.

Then **write the simulation state file** to `/tmp/swarm-state.json`:
```json
{
  "scenario": "...",
  "environmentRules": "...",
  "outputGoal": "...",
  "populations": [...],
  "researchSummary": "...",
  "researchSources": [...],
  "rounds": [],
  "currentRound": 0,
  "worldState": {
    "sentimentCounts": {"positive": 0, "neutral": 0, "negative": 0},
    "stanceDistribution": {},
    "keyEvents": [],
    "populationSummaries": {}
  }
}
```

### Step 2: Run 5 Simulation Rounds (parallel subagents)

For EACH round (1 through 5):

#### 2a. Spawn population subagents in parallel

Launch one **Agent subagent per population** in a single message — all running simultaneously. This is the key innovation: each population is simulated independently and cannot see other populations' decisions for this round.

Each population subagent gets this prompt:

```
You are a behavioral simulation engine for ONE population group in a multi-agent simulation. You must generate realistic, psychologically diverse responses.

SCENARIO: {scenario_description}
ENVIRONMENT RULES: {environment_rules}

YOUR POPULATION: "{population_name}"
- Count: {count} individuals
- Personality: {personality}
- Goals: {goals}
- Behavioral bias: {bias}
- Variation level: {variation}

CURRENT WORLD STATE (from previous rounds):
{worldState as JSON — sentiment counts, stance distribution, key events, other population summaries}

ROUND: {round_number} of 5

VARIATION GUIDANCE:
- low: Minor differences, slight sentiment variations
- medium: Moderate diversity — some agree, some hesitant, a few dissent
- high: Wide range — strong disagreements, outliers, contrarian views

Generate decisions for each of your {count} individuals. Each agent should react to the current world state based on their personality and bias.

Return ONLY a JSON object (no markdown, no explanation):
{
  "populationName": "{population_name}",
  "decisions": [
    {
      "agentIndex": 0,
      "agentName": "A generated name for this individual",
      "observation": "What this person notices/focuses on (1-2 sentences)",
      "action": "What they decide to do (1-2 sentences)",
      "reasoning": "Why they made this choice (1 sentence)",
      "sentiment": "positive|neutral|negative",
      "stance": "short label (e.g., stay, leave, wait-and-see, protest)"
    }
  ],
  "populationSummary": "One-sentence summary of this population's overall mood and direction this round"
}
```

**CRITICAL: Spawn ALL population agents in a SINGLE message so they run in parallel.** Do not run them sequentially.

#### 2b. Aggregate round results

When all population subagents return, read their JSON results. Then:

1. **Aggregate sentiment** — count positive/neutral/negative across all populations
2. **Aggregate stances** — merge stance distributions
3. **Detect emergent events:**
   - Negative sentiment ratio increased >15% from previous round
   - Positive sentiment ratio increased >15% from previous round
   - All populations converge on same dominant stance (cross-population consensus)
   - A population's dominant stance flipped from previous round
   - Any unexpected cross-group solidarity or conflict
4. **Update `/tmp/swarm-state.json`** — append the round data and update worldState
5. **Show the user a round summary:**

```
### Round {N} of 5

**Sentiment:** +{pos} / ={neu} / -{neg}
**Stances:** {stance}: {count}, ...
{For each emergent event: "⚡ **Emergent:** {event}"}
**Population highlights:**
- {pop1}: {summary}
- {pop2}: {summary}
```

Then proceed to the next round with the updated world state.

### Step 3: Generate Report

After all 5 rounds, read the full `/tmp/swarm-state.json` and analyze the complete simulation. Produce a structured report with:

- **Executive Summary** (2-3 paragraphs directly answering the original question)
- **Key Findings** (5-8 findings with confidence: high/medium/low)
- **Emergent Behaviors** (2-4 unexpected patterns — the most valuable insights)
- **Recommendations** (3-5 actionable next steps)
- **Confidence Score** (0-100)
- **Notable Agents** (2-3 interesting individual agent stories from the simulation data)
- **Round Summaries** (key event per round)

Write the report to `/tmp/swarm-state.json` under a `report` key.

### Step 4: Generate HTML Dashboard

This is the key deliverable. Write an HTML file to `/tmp/swarm-report.html` and open it with `open /tmp/swarm-report.html`.

Read the simulation data from `/tmp/swarm-state.json` to populate the dashboard.

The HTML dashboard should be a single self-contained file (inline CSS + JS, no external deps) with:

**Header section:**
- Scenario title (large)
- Stats bar: total agents, rounds, number of populations, confidence score gauge (colored: green >70, yellow 40-70, red <40)
- Badge: "Independent Subagents" to indicate real multi-agent simulation

**Executive Summary card**

**Key Findings** — numbered list with colored confidence badges (green=high, yellow=medium, red=low)

**Sentiment Over Time chart** — an SVG line chart showing positive/neutral/negative sentiment counts across all 5 rounds (3 colored lines: green, gray, red). X-axis = rounds, Y-axis = agent counts. Include gridlines and data point labels.

**Stance Distribution** — horizontal bar chart (SVG) showing final stance distribution with percentage labels

**Population Breakdown** — show each population's final sentiment and dominant stance as a mini card grid

**Emergent Behaviors** — cards with lightning bolt icon, yellow accent border

**Round-by-Round Timeline** — collapsible accordion sections, click to expand each round's details including per-population summaries

**Agent Spotlight** — cards for each notable agent with their population badge and story

**Recommendations** — numbered cards

**Research Sources** — list of sources from the research phase with links

**Design requirements:**
- Dark theme: background `#0f172a`, cards `#1e293b`, text `#e2e8f0`, accents `#38bdf8`
- Clean sans-serif font (system font stack)
- Rounded corners, subtle shadows
- Responsive layout (max-width 900px centered)
- Smooth transitions on accordion open/close
- Print-friendly: add `@media print` styles

After writing the file, open it in the browser with `open /tmp/swarm-report.html`.

## Key Principles

- **Real independence** — each population is a separate subagent that cannot see other populations' current-round decisions. This produces genuinely emergent behavior.
- **Parallel execution** — all population agents for a round launch simultaneously. Research agents also run in parallel. This maximizes speed.
- **Persistent state** — `/tmp/swarm-state.json` tracks the full simulation state. Each round reads and updates it. The dashboard reads from it.
- **Research first** — ground the simulation in real data via parallel web search agents
- **Realistic psychology** — agents aren't uniform. Include dissent, indifference, unexpected reactions
- **Emergent behavior is the value** — the independence of subagents means emergent patterns are real, not imagined
- **The HTML dashboard is the deliverable** — make it beautiful and data-rich
- **Keep the user updated** — show research summary, round summaries as they complete, then deliver the dashboard
