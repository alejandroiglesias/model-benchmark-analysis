# Benchmark Source Catalog

Sources by capability. Treat the first three as **primary** general boards and
the rest as **task-specific** boards. Always confirm a board is still live and
recently updated before relying on it — leaderboards get renamed, versioned, or
abandoned. When a listed board is gone, search for its current successor rather
than citing stale data. New benchmarks appear constantly; the lists below are a
starting set, not a closed universe — search for newer task-specific boards too.

## Primary general boards

- **Artificial Analysis** — https://artificialanalysis.ai/models — Intelligence
  Index (composite of evals like GPQA Diamond, HLE, Terminal-Bench, SciCode,
  IFBench, GDPval), plus a **Coding Agent Index**, and image/video arenas. Also a
  good cross-check for API pricing and speed/latency.
- **Vellum LLM Leaderboard** — https://www.vellum.ai/llm-leaderboard — GPQA, AIME,
  SWE-Bench, HLE, ARC-AGI-2, with side-by-side pricing.
- **Scale / SEAL leaderboards** — https://scale.com/leaderboard — including
  **MCP Atlas** (real tool-use pass rate over many tasks), plus finance, coding,
  and other domain boards.

> **Heads-up — these are JavaScript-rendered.** Artificial Analysis and the
> OpenRouter rankings page load their tables via JS, so a plain fetch returns an
> empty shell. **Default to a rendering reader proxy** (`https://r.jina.ai/<url>`),
> which works from a plain fetch in any environment; prefer a connected browser
> tool when one is available, or the board's underlying JSON endpoint when you need
> the freshest data. See **SKILL.md step 3 ("Client-rendered boards")** for the
> full ladder and caveats. Always cite the original board for each figure, and
> watch for stale proxy caches.

## Coding / software engineering

- **Artificial Analysis Coding Agent Index** (combines SWE-Bench-Pro-Hard-AA,
  Terminal-Bench, SWE-Atlas-QnA-style evals) — required for coding.
- **CursorBench** (current version, e.g. 3.x) — required for coding; include the
  latest **Cursor Composer** model version among candidates.
- **DeepSWE** — required for coding.
- **SWE-Bench / SWE-Bench Verified / SWE-Bench-Pro**, **Terminal-Bench**,
  **SWE-Atlas**, **LiveCodeBench**, **Aider polyglot**.

## Web browsing / agentic web

- **BrowseComp** (web browsing + extraction).
- **MCP Atlas** (Scale) and other **tool-use / function-calling** boards.
- **GAIA**, **WebArena / VisualWebArena**, **AssistantBench** as proxies.

## Data analysis

- **AstaBench** (data-analysis / code-&-execution tracks).
- Reasoning proxies: **GPQA Diamond**, **AIME**, **HLE** (from Vellum/Scale).
- Tool/code-execution evals where the task involves running stats or charts.

## Image generation

- Image arenas / leaderboards (text-to-image **and** editing), e.g. Artificial
  Analysis image arena and comparable blind-preference image arenas.
- Rank text-to-image and edit quality separately when the task implies editing.
- Do **not** use these for the "Design" task (see category playbooks).

## Video generation

- Video arenas / blind-preference boards (text-to-video, image-to-video),
  e.g. Artificial Analysis video arena and comparable video leaderboards.
- Note audio support, max clip length, and resolution when relevant.

## OCR / document parsing

- **OCRBench** and **OCRBench v2**, **CC-OCR** leaderboard, **ParseBench**,
  **OCR Arena**, **DeltOCR Bench**. Many top OCR models are open vision-language
  models — capture their sizes and free/open availability.

## Trading / finance agents

- Direct market & prediction-market benchmarks: **PredictionMarketBench**,
  **Prediction Arena**, **StockBench**, **TraderBench**, **InvestorBench**,
  **FinToolBench**, and similar finance-agent boards.
- **Vending-Bench 2** — required as a long-horizon economic-agent proxy (pricing
  discipline, inventory/capital allocation, negotiation, counterparty risk,
  sustained tool use). Treat it as a strong business/economic-agency proxy, **not**
  a substitute for securities or prediction-market benchmarks. State the weight you
  give it (e.g. ~25%) and why.
- Warn explicitly: do not let any model place real trades without guardrails;
  benchmark skill rarely transfers to live P&L/risk robustness.

## Creative writing

- Blind preference arenas (e.g. LMArena creative/writing categories),
  **EQ-Bench creative writing**, long-context coherence evals, instruction
  following. Strong long-context and style-control signals matter more than raw
  reasoning here.

## Design (interfaces / brand systems / frontend) — NOT image generation

- Frontend/coding quality (CursorBench, SWE-Atlas, frontend-code reliability),
  visual reasoning, instruction following, and tool-use proxies. Rank for the
  ability to produce professional, tasteful design output in tools like
  Pencil.dev, Open Design, or via frontend code — never on image-generation
  arenas.

## Short-form writing / persuasion / personalization

- Instruction following (**IFBench / IFEval**), preference arenas, and
  summarization/style-control proxies. Useful for social, email, diagnoser,
  pitcher, checker roles.

## Tool use / orchestration / agentic reliability

- **MCP Atlas** (real tool-use pass rate), function-calling boards, **GDPval**,
  agentic/escalation proxies, **HLE** as a reasoning tie-breaker. Useful for
  orchestrator, scout, mobile, filmer roles.

## General reasoning (tie-breakers only)

- **HLE**, **GPQA Diamond**, **ARC-AGI-2**, **AIME**, **GDPval**. Use only to
  break ties between models that are close on task-direct evidence — never as the
  primary ranking signal for a specific task.
