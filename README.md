# SuperRadar

The live tool catalog behind [SuperRadar](https://super-agentic.ai/super-radar) — Superagentic AI's map of the agentic AI tooling landscape.

[super-agentic.ai](https://super-agentic.ai) fetches `superradar-tools.json` from this repo's `main` branch at runtime. Push a change here and it goes live on the site within about a minute — no site rebuild or deploy required. `superradar-tools.json` is the only file that matters for a catalog update; nothing else in this repo needs touching.

## If you're an AI agent asked to "update SuperRadar"

This section is your operating manual. Follow it end to end — don't just append a tool and stop.

### 1. Orient yourself
- The whole catalog is one file: `superradar-tools.json`, with four top-level keys: `updatedAt`, `featuredWeekLabel`, `featuredToolIds`, `tools`, `toolRepositories`.
- Read the current file first. Skim `tools[]` for existing `id`s/`name`s so you don't create duplicates, and get a feel for how similar tools are already described (tone, description length, which optional fields get used).

### 2. Research what's actually new
The ask is usually something like "look at the latest tools in this category launched in recent days and update the list." To do that well:
- Web-search for recent (last ~7–30 days, unless told otherwise) launches/releases in the relevant space — coding agents, agent frameworks, orchestration, evaluation/observability, agent protocols (MCP, A2A, ACP), inference engines, memory/vector stores, etc. Search terms like `"<category>" agent tool launch 2026`, `"<product>" release notes`, or `site:github.com/trending agent` work well.
- Good sources: official product blogs/changelogs/GitHub release pages (first-party, always preferred), GitHub Trending, Hacker News "Show HN" and front page, Product Hunt AI category, AI-focused newsletters, and the announcement threads of major labs (OpenAI, Anthropic, Google DeepMind, Meta) when they ship agent-relevant tooling.
- **Verify against a first-party source before adding anything** — the official site, repo, or announcement post. Don't add a tool, a star count, or a release date on the strength of a secondhand summary or a tweet alone. If you can't verify a detail, leave that optional field out rather than guess.
- Check whether the tool already exists under a different `id`/name before adding it as new.

### 3. Classify each tool correctly
Every tool needs a `layer` and a `category` that fits it:

| Layer | Categories |
|---|---|
| `Models` | Foundational Models, Sovereign AI |
| `Frameworks` | Agent Frameworks, Multi-Agent Orchestration, Agent Protocols, Web, Browser & API |
| `Coding` | Coding Models, Coding Agents, Agentic IDE, Agentic CLI, Code Review, Vibe Code (Browser), Extensions, Wiki |
| `Knowledge` | Embedding Model Providers, Vector Stores, Memory Systems, Knowledge Graphs, Search |
| `AgentOps` | Evaluation, Observability, Sandbox, Connectors, Security, Model Serving, Model Hosts, Inference Engines |

`state` is a Tech-Radar-style ring — pick the one that best reflects reality, don't default to `"Assess"` for everything:
- **Adopt** — mature, production-proven, a safe default choice for most teams.
- **Trial** — promising, worth piloting on a real project, not yet battle-tested everywhere.
- **Assess** — early-stage or narrow use case, worth knowing about, not yet worth betting on.
- **Hold** — declining, superseded, or otherwise not recommended for new projects (don't delete these outright — see step 5).

### 4. Fill in the fields
| Field | Type | Notes |
|---|---|---|
| `id` | string | unique, kebab-case |
| `name` | string | |
| `state` | `"Adopt" \| "Trial" \| "Assess" \| "Hold"` | see rubric above |
| `description` | string | one sentence, factual, no marketing fluff |
| `category` / `layer` | string | from the table above |
| `icon` | string | **must** be one of the names in the list below — an unrecognized name silently renders with no icon (not an error) |
| `sourceType` | `"Open Source" \| "Open Weights" \| "Closed Source" \| "Commercial"` | optional |
| `websiteUrl`, `githubUrl`, `logoUrl` | string | optional, verify they resolve |
| `country` | string | ISO country code, optional, only if confidently known |
| `trending`, `new`, `deprecated`, `highlighted` | boolean | optional |

Valid `icon` values (lucide-react names): `Layers`, `Code`, `Database`, `Server`, `Shield`, `Search`, `FlaskConical`, `Brain`, `Network`, `Zap`, `Package`, `Lock`, `ChevronsUp`, `Cpu`, `Cloud`, `Settings`, `ServerCog`, `Sparkles`, `Activity`, `GitBranch`, `Terminal`, `Bot`, `Users`, `FileText`, `MessageSquare`, `Wrench`, `Blocks`, `Satellite`, `BarChart`, `CheckCircle`, `ShieldCheck`, `TrendingUp`, `Globe`. Pick the closest match — don't invent a new name.

If a tool has its own GitHub repo/org and docs site, add a matching entry to `toolRepositories` keyed by the same `id`:
```json
"tool-id": { "repo": "https://github.com/org/repo", "docs": "https://docs.example.com" }
```

### 5. Handle duplicates and removals
- Two entries for the same product → keep the more complete/accurate one, delete the other from both `tools[]` and `toolRepositories`.
- A tool that's genuinely dead/discontinued → delete it, or set `deprecated: true` if it's still worth showing for historical context (your judgement call; prefer deleting if unsure).
- Don't remove a tool just because it's old — only for real duplicates or defunct projects.

### 6. Featured tools of the week (optional, only if asked)
Update `featuredToolIds` (up to 5 tool `id`s, most newsworthy first) and `featuredWeekLabel` (e.g. `"3–9 August 2026"`) to reflect the current release radar.

### 7. Before you push — checklist
- [ ] JSON is valid (no trailing commas, matching brackets) — a broken file means the site silently keeps showing its last-known-good data, so a bad push looks like nothing happened rather than an obvious error.
- [ ] Every new/edited `id` is unique and kebab-case.
- [ ] **`updatedAt` is bumped to the current UTC ISO timestamp** (e.g. `"2026-08-05T14:30:00Z"`) — mandatory on every change. The site only re-applies data when `updatedAt` differs from what it last saw, so forgetting this can make a real edit silently not go live.
- [ ] You haven't restructured the top-level shape (still exactly `updatedAt`, `featuredWeekLabel`, `featuredToolIds`, `tools`, `toolRepositories`).
- [ ] Commit message says what changed (e.g. `Add 3 new coding agents, remove duplicate X`), pushed to `main`.

### File shape
```json
{
  "updatedAt": "2026-08-05T14:30:00Z",
  "featuredWeekLabel": "22–28 July 2026",
  "featuredToolIds": ["tool-id-1", "tool-id-2"],
  "tools": [
    { "id": "tool-id-1", "name": "Tool Name", "state": "Adopt", "...": "..." }
  ],
  "toolRepositories": {
    "tool-id-1": { "repo": "https://github.com/org/repo", "docs": "https://docs.example.com" }
  }
}
```

## Maintained by

[Superagentic AI](https://super-agentic.ai).
