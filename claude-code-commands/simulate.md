# Swarm Simulator — Multi-Agent Scenario Simulation

Run a multi-agent swarm simulation directly in Claude Code. No API keys needed — YOU are the AI running the simulation. Generates a beautiful HTML dashboard with the results.

**Usage:** `/simulate <scenario description>`

## Instructions

You are running a multi-agent swarm simulation. You ARE the simulation engine — generate all agent responses directly, no external API calls needed.

The user's scenario: `$ARGUMENTS`

Follow these steps exactly:

### Step 0: Research the Scenario

Before designing populations, use WebSearch and WebFetch to research the scenario topic. This grounds the simulation in real-world data rather than guesswork.

**Research goals:**
- Current market data, statistics, or facts relevant to the scenario
- Historical precedents — has something similar happened before? What were the outcomes?
- Key stakeholders and their known positions
- Industry trends that would influence agent behavior
- Any recent news or developments related to the topic

Run 3-5 web searches with different angles on the scenario. Summarize the key research findings and cite sources. These findings will inform population design, agent behavior biases, and the environment rules.

Show the user a brief **Research Summary** with the most relevant facts before proceeding.

### Step 1: Design Agent Populations (informed by research)

Based on the scenario, design 3-5 agent populations. For each define:
- **Name**, **Count** (10-30 each, 40-100 total), **Personality**, **Goals**, **Behavioral bias**, **Variation** (low/medium/high)

Also define: **Environment rules**, **Output goal** (key question to answer), **Rounds** (use 5).

Show the user a summary table of populations before proceeding.

### Step 2: Run 5 Simulation Rounds

For EACH round (1-5), for EACH population, generate realistic agent decisions. You are simulating human behavior — responses must be diverse, realistic, and reflect genuine psychology.

For each population in each round, generate a JSON array of individual agent decisions:

```json
[
  {
    "agentIndex": 0,
    "observation": "What this person notices/focuses on",
    "action": "What they decide to do (1-2 sentences)",
    "reasoning": "Why (1 sentence)",
    "sentiment": "positive|neutral|negative",
    "stance": "short label (e.g., stay, leave, wait-and-see, protest)"
  }
]
```

**Variation guidance:**
- **low**: Minor wording differences, slight sentiment variations
- **medium**: Moderate diversity — some agree, some hesitant, a few dissent
- **high**: Wide range — strong disagreements, outliers, contrarian views

**After generating each round's decisions, aggregate:**
1. Sentiment counts (positive/neutral/negative)
2. Stance distribution
3. Emergent events:
   - Negative sentiment ratio increased >15% from previous round
   - All populations converge on same dominant stance
   - Any unexpected cross-group solidarity or conflict

**Show the user a brief round summary:**
```
### Round {N} of 5
**Sentiment:** +{pos} / ={neu} / -{neg}
**Stances:** {stance}: {count}, ...
{If emergent: "⚡ **Emergent:** {event}"}
```

**IMPORTANT:** Each subsequent round must reference the previous round's world state. Agents should react to what happened before — opinions shift, new alliances form, some agents change stance.

### Step 3: Generate Report

After all 5 rounds, analyze the full simulation and produce a structured report:

```json
{
  "title": "Short scenario title",
  "executiveSummary": "2-3 paragraphs directly answering the original question",
  "keyFindings": [{ "finding": "...", "confidence": "high|medium|low" }],
  "emergentBehaviors": ["Unexpected patterns"],
  "recommendations": ["Actionable next steps"],
  "confidenceScore": 0-100,
  "notableAgents": [{ "name": "...", "population": "...", "story": "..." }],
  "roundSummaries": [{ "round": 1, "summary": "...", "keyEvent": "..." }]
}
```

Provide 5-8 key findings, 2-4 emergent behaviors, 3-5 recommendations, 2-3 notable agent stories.

### Step 4: Generate HTML Dashboard

This is the key deliverable. Write an HTML file to `/tmp/swarm-report.html` and open it with `open /tmp/swarm-report.html`.

The HTML dashboard should be a single self-contained file (inline CSS + JS, no external deps) with:

**Header section:**
- Scenario title (large)
- Stats bar: total agents, rounds, confidence score gauge (colored: green >70, yellow 40-70, red <40)

**Executive Summary card**

**Key Findings** — numbered list with colored confidence badges (green=high, yellow=medium, red=low)

**Sentiment Over Time chart** — an SVG line chart showing positive/neutral/negative sentiment counts across all 5 rounds (3 colored lines: green, gray, red). X-axis = rounds, Y-axis = agent counts. Include gridlines.

**Stance Distribution** — horizontal bar chart (SVG) showing final stance distribution with percentage labels

**Emergent Behaviors** — cards with lightning bolt icon, yellow accent border

**Round-by-Round Timeline** — collapsible accordion sections, click to expand each round's details

**Agent Spotlight** — cards for each notable agent with their population badge and story

**Recommendations** — numbered cards

**Design requirements:**
- Dark theme: background `#0f172a`, cards `#1e293b`, text `#e2e8f0`, accents `#38bdf8`
- Clean sans-serif font (system font stack)
- Rounded corners, subtle shadows
- Responsive layout (max-width 900px centered)
- Smooth transitions on accordion open/close
- Print-friendly: add `@media print` styles

After writing the file, open it in the browser with `open /tmp/swarm-report.html`.

## Key Principles

- **Research first** — ground the simulation in real data via web search before designing agents
- **No API calls needed** — you generate all agent responses directly as part of the conversation
- **Realistic psychology** — agents aren't uniform. Include dissent, indifference, unexpected reactions
- **Emergent behavior is the value** — actively look for surprising cross-group dynamics
- **The HTML dashboard is the deliverable** — make it beautiful and informative. Include a "Research Sources" section at the bottom of the dashboard listing the key sources found during research.
- **Keep the user updated** — show research summary, then round summaries as you go, then deliver the dashboard at the end
