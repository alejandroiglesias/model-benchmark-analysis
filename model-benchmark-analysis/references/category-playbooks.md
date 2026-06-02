# Category Playbooks

Benchmark mappings and special rules for the predefined task categories and agent
roles. If the argument matches one of these (closely enough), use its mapping and
honor its **required** benchmarks and special rules. If it doesn't match, use the
**Novel task method** at the bottom.

Benchmark names are pointers — resolve them to current boards/versions via
`benchmark-sources.md` and verify live. Weights are starting defaults; adjust and
state your reasoning if evidence warrants.

## Task categories

### 1. Coding
- **Required:** Artificial Analysis Coding Agent Index, CursorBench (latest), DeepSWE.
- Also: SWE-Bench(-Pro/Verified), Terminal-Bench, SWE-Atlas, LiveCodeBench.
- **Include the latest Cursor Composer version** as a ranked candidate. It has no
  standalone REST API but is usable via the **Cursor SDK/CLI**, so it counts as
  selectable — rank it normally and note the access path. Price-filter it on
  Cursor's published usage rates (e.g. it can qualify for the Budget list if those
  rates are under cap).
- Weight agentic/SWE coding evals highest; general reasoning as tie-breaker only.

### 2. Web browsing
- BrowseComp (highest), MCP Atlas / tool-use, GAIA / WebArena / AssistantBench proxies.
- Reward reliable navigation, search, and extraction over raw reasoning.

### 3. Data analysis
- AstaBench (data-analysis / code-&-execution), GPQA/AIME/HLE reasoning proxies,
  code-execution tool use. Reward models that can run stats and produce charts.

### 4. Image generation
- Image arenas (text-to-image **and** edit). Apply the **image** budget cap
  procedure (per-image/per-edit). Free list is often empty here — say so if so.

### 5. Video generation
- Video arenas (text-to-video / image-to-video). Apply the **video** budget cap
  (per generated minute). Note clip length, resolution, audio. Free list is often
  empty — say so if so.

### 6. Social media
- Proxy-heavy: creative/short-form writing, instruction following, tool use,
  latency, cost. Say it's proxy-driven. Recommend a checker pass before posting.

### 7. Email
- Proxy-heavy: instruction following, summarization, tone/style control, tool use,
  privacy/cost. Tone control often beats raw intelligence for the budget pick.

### 8. Calendar
- Proxy-heavy: MCP Atlas / function-calling reliability, latency, human-escalation
  proxy. Low-latency tool callers win.

### 9. Trading
- **Required:** Vending-Bench 2 (long-horizon economic-agency proxy; state its
  weight, ~25%, and that it is NOT a securities substitute).
- Include direct finance/prediction-market benchmarks: PredictionMarketBench,
  Prediction Arena, StockBench, TraderBench, InvestorBench, FinToolBench.
- **Warn:** no model should place real trades without guardrails.

### 10. OCR
- **Suggested:** OCRBench, OCRBench v2, CC-OCR, ParseBench, OCR Arena, DeltOCR Bench.
- Many leaders are open VLMs — capture sizes and free/open availability; the Free
  list can be strong here.

### 11. Design (NOT image generation)
- **Do not rank on image-generation arenas.** Rank ability to produce professional,
  tasteful design in tools like Pencil.dev, Open Design, or via frontend code.
- Use frontend/coding quality (CursorBench, SWE-Atlas), visual reasoning,
  instruction following, tool-use proxies.

### 12. Creative writing
- Blind-preference writing arenas, EQ-Bench creative writing, long-context
  coherence, instruction following. Style/voice control and long-context matter
  most; raw reasoning is secondary.

## Agentic roles

### Orchestrator agent
- Top agentic coordination, delegation, human escalation. MCP Atlas, agentic/
  escalation proxies, GDPval, HLE tie-breaker. Favor reliability when mistakes are
  expensive — often an Absolute-tier pick.

### Scout agent
- Info gathering on Google Maps (Places API) + web + niche filtering. BrowseComp,
  MCP Atlas, web-extraction proxies; weigh Maps/Search economics and tool support.
  Volume-sensitive — budget tier often wins.

### Diagnoser agent
- Writes ~50-word diagnoses + <70-word cold messages. Short-form writing,
  personalization, instruction following, cost/latency. Tone control is decisive;
  free drafts acceptable only behind a Checker.

### Landing page builder agent
- Builds frontend landing pages. CursorBench, SWE-Atlas, design/frontend-code
  quality, some creative writing. Quality tier for saleable pages; budget tier for
  cheap variants.

### Filmer agent
- Screenshots landing pages + builds short videos via Playwright/Python. Coding /
  tool-use, Playwright/Python execution proxies, latency, reliability. Procedural
  and volume-sensitive — budget tier often wins.

### Pitcher agent
- Pitches sales via email/IG/LinkedIn MCP. Persuasive writing, MCP/email/social
  tool use, personalization. Tone-sensitive — quality or strong budget pick.

### Checker agent
- Anti-AI eval + personalization check on other agents' output. Critique, style
  detection, personalization, instruction following. Quality matters but volume is
  high — strong budget pick.

### Mobile agent
- Notifications via Claude Code on iPhone + Calendly. Calendar/email/tool use,
  latency, notification/scheduling proxies. Low-latency tool callers win.

## Novel task method (argument not in the lists above)

Many useful queries won't match a predefined category — e.g. "ad creative producer
agent", "infoproduct book writer agent", "customer support triage agent", "SQL
analytics copilot". Handle them like this:

1. **Decompose** the role into 2–5 underlying capability dimensions and assign each
   an explicit weight reflecting how much the task leans on it. State the breakdown
   up front and invite the user to rerun with different emphasis.
   - *ad creative producer agent* → marketing/creative copy ~45%, image generation
     ~30%, brand/visual taste ~15%, tool use & iteration ~10%.
   - *infoproduct book writer agent* → long-form writing ~50%, instruction
     following & structure ~25%, long-context coherence ~15%, factual reliability
     ~10%.
   - *customer support triage agent* → instruction following ~35%, tool use /
     function calling ~30%, tone/empathy ~20%, latency/cost ~15%.
2. **Map** each capability to the relevant benchmark families in
   `benchmark-sources.md`.
3. **Honor the type rules:** if a dimension is image- or video-generation, apply
   that modality's budget cap for those candidates. If a dimension is "design" in
   the interface sense, do not use image arenas for it.
4. **Combine** capability scores using your declared weights; normalize and rank as
   in the main workflow.
5. **Be explicit about proxies** — novel roles are usually proxy-heavy. Say so, and
   warn that the ranking is directional.
