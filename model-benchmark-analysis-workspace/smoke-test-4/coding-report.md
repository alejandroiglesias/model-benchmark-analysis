# Model Benchmark Analysis — Coding
Date checked: 2026-06-02

**Capability read:** Predefined "Coding" category → agentic software engineering. Ranked on agentic/SWE coding evidence first (AA Coding Agent Index, SWE-Bench Pro, CursorBench, DeepSWE), with single-shot coding and general reasoning as tie-breakers only.

**Benchmarks used & weighting:** Primary signal is the **agentic Artificial Analysis Coding Agent Index** (composite of SWE-Bench-Pro-Hard-AA + Terminal-Bench v2 + SWE-Atlas-QnA) and the **SWE-Bench Pro** component that feeds it; **CursorBench v3.1** and **DeepSWE** are weighted next; the AA single-shot **Coding Index**, SWE-Bench Verified/Multilingual, Terminal-Bench, and general reasoning act as tie-breakers. Boards disagree (the AA Coding Agent Index crowns Anthropic on SWE-Bench-Pro-heavy tasks; DeepSWE crowns GPT-5.5; CursorBench is vendor-run and "display-only"). Per the skill's tie-break order I trust the **independent, agentic** Coding Agent Index over the vendor CursorBench board, and I treat the May-21 AA article as **superseded at the top** by Claude Opus 4.8 (released May 28, 2026), which raised the exact component benchmarks (SWE-Bench Pro 64.3%→69.2%) that drive the index. Scores are min–max normalized within each board and are directional, not exact. OpenRouter usage was not used as evidence (its SPA didn't serialize; secondary signal only).

**Master ranking (source of truth — all selectable candidates, best agentic-coding first).** The three lists below are filtered views of this one ordering, not separate rankings:

1. Claude Opus 4.8 (max) · 2. GPT-5.5 (xhigh) · 3. Claude Opus 4.7 (max) · 4. **Cursor Composer 2.5** (via Cursor SDK/CLI) · 5. Gemini 3.5 Flash · 6. **Kimi K2.6** · 7. Gemini 3.1 Pro · 8. GPT-5.3 Codex · 9. GLM-5.1 · 10. MiniMax M3 · 11. GLM-4.5-Air.

_Ordering-consistency note (Composer 2.5 #4 above Kimi K2.6 #6):_ Composer 2.5 places **3rd on the required AA Coding Agent Index (62)** and 2nd on CursorBench (63.2); Kimi K2.6 has no Coding Agent Index placement and a lower single-shot AA Coding Index (53.9). They are near-tied on SWE-Bench Verified/Multilingual (Composer 79.8% vs Kimi 80.2%), but the higher-weighted **agentic** board separates them, so Composer outranks Kimi in every list where both appear.

## Top 5 — Absolute Performance
1. **Claude Opus 4.8 (max)** — new agentic-coding frontier leader; SWE-Bench Pro **69.2%** (highest of any model) and SWE-Bench Verified 88.6%, both up from Opus 4.7; released May 28, 2026, after the AA Coding Agent Index article, on the very benchmarks that feed that index. [SWE-Bench Pro/Verified via [Vellum](https://www.vellum.ai/blog/claude-opus-4-8-benchmarks-explained), 2026-05-28]
2. **GPT-5.5 (xhigh, in Codex)** — AA **Coding Agent Index 65** (was #2) and **DeepSWE #1 at 70%** (independent); leads Terminal-Bench 2.1 (78.2%). Behind Opus 4.8 on SWE-Bench Pro (58.6% vs 69.2%), so #2. [Coding Agent Index via [Artificial Analysis](https://artificialanalysis.ai/articles/cursor-composer-2-5-coding-agent-index), 2026-05-21; DeepSWE via [BenchLM](https://benchlm.ai/benchmarks/deepSwe), May 2026]
3. **Claude Opus 4.7 (max, in Claude Code)** — former **#1 on the AA Coding Agent Index (66)** and #1 on CursorBench v3.1 (64.8%); now superseded by Opus 4.8 but still a top-tier agentic coder and still selectable. [[Artificial Analysis](https://artificialanalysis.ai/articles/cursor-composer-2-5-coding-agent-index), 2026-05-21; [BenchLM CursorBench v3.1](https://benchlm.ai/benchmarks/cursorBench31), 2026-06-01]
4. **Cursor Composer 2.5** (via Cursor SDK/CLI) — **3rd on the required AA Coding Agent Index (62)**, 2nd on CursorBench v3.1 (63.2%), and 79.8% SWE-Bench Multilingual (≈ Opus 4.7 parity) at ~10–60× lower cost per task. No standalone REST API; selectable via the Cursor SDK/CLI. [[Artificial Analysis](https://artificialanalysis.ai/articles/cursor-composer-2-5-coding-agent-index), 2026-05-21; [Cursor](https://cursor.com/blog/composer-2-5), 2026-05-18]
5. **Gemini 3.5 Flash (high)** — beats Gemini 3.1 Pro across the coding/agentic suite: Terminal-Bench 2.1 76.2%, MCP Atlas 83.6%, plus a higher GDPval-AA Elo; SWE-Bench Verified 55.1%. Strongest non-Anthropic/OpenAI/Cursor agentic coder. [[llm-stats](https://llm-stats.com/blog/research/gemini-3.5-flash-launch), 2026; AA coding/agentic suite via [Gemini 3.5 Flash launch coverage](https://llm-stats.com/blog/research/gemini-3.5-flash-launch)]

_Highest raw scores but worth flagging: all five are publicly selectable, so no exclusions were needed here. Kimi K2.6 (master #6) just misses the Absolute top 5 — it trails Composer 2.5 on the agentic Coding Agent Index._

## Top 5 — Budget-Eligible Near-Frontier
_Price cap: input ≤ $2.50/M, output ≤ $9/M (standard API). Two separate numbers; blended aggregator prices can't satisfy it._

Filtered view of the master ranking — only entries clearing the cap, kept in master order. Opus 4.8 ($5/$25), GPT-5.5 ($5/$30), Opus 4.7 (~$5/$25), GPT-5.3 Codex ($1.75/$14 — output over cap) and Gemini 3.1 Pro ($2/$12 — output over cap) all **fail** the cap and drop out. Survivors, in master order:

1. **Cursor Composer 2.5 — $0.50 in / $2.50 out (Standard tier)** [[Cursor](https://cursor.com/blog/composer-2-5) / [Artificial Analysis](https://artificialanalysis.ai/articles/cursor-composer-2-5-coding-agent-index), 2026-05] — the highest-ranked budget-eligible model and the only one scoring 60+ on the AA Coding Agent Index (62). Use the **Standard** tier to qualify; the default **Fast** tier ($3/$15) exceeds the cap, so price-filter on Standard per the multi-tier rule.
2. **Gemini 3.5 Flash — $1.50 in / $9.00 out** [[Gemini pricing / llm-stats](https://llm-stats.com/blog/research/gemini-3.5-flash-launch), 2026] — output lands exactly at the $9 boundary (inclusive → qualifies). Strong agentic/tool suite (Terminal-Bench 76.2%, MCP Atlas 83.6%).
3. **Kimi K2.6 — $0.60 in / $2.50 out (Moonshot API)** [[Artificial Analysis](https://artificialanalysis.ai/models/kimi-k2-6) / Moonshot, 2026] — open-weight 1T MoE; SWE-Bench Verified 80.2%, SWE-Bench Pro 58.6. (Aggregator blended prices of $1.15–$2.15 are *not* used for the cap test; the separate Moonshot input/output rates clear it.)
4. **MiniMax M3 — $0.30 in / $1.20 out** [[BenchLM](https://benchlm.ai/compare/gpt-5-3-codex-vs-minimax-m3), 2026] — solidly under cap; mid-pack agentic coding (~67 avg on coding benchmarks, ahead of GPT-5.3 Codex on that comparison).
5. **GLM-4.5-Air — paid hosted route ≪ cap** [[OpenRouter](https://openrouter.ai/z-ai/glm-4.5-air), 2026] — lightweight open-weight agentic model, SWE-Bench Verified ~59.8%. Lowest of the budget five; included because it clears the cap and has a real coding benchmark. (Also has a free `:free` route — see Free list.)

_Note: this is "best performers among the affordable," not score-per-dollar. GLM-5.1 (full) was left off for lack of a confirmed under-cap standard hosted price for this run; if its hosted rate clears the cap it would slot near here._

## Top 5 — Free
_Free via: OpenRouter `:free` routes (free hosted endpoints, no card), plus kimi.com / OpenCode Zen where noted. Rate-limited: OpenRouter free ≈ 20 req/min, ~50–1000 req/day._

Filtered view of the master ranking, keeping only models on a genuinely free hosted endpoint **with** a public coding benchmark, in master order:

1. **Kimi K2.6** — via OpenRouter [`moonshotai/kimi-k2.6:free`](https://openrouter.ai/moonshotai/kimi-k2.6:free) (and free on kimi.com). SWE-Bench Verified 80.2%, SWE-Bench Pro 58.6 — the strongest model with a genuinely free route. 262K context; subject to OpenRouter free rate limits. [[Artificial Analysis](https://artificialanalysis.ai/models/kimi-k2-6), 2026]
2. **GLM-4.5-Air** — via OpenRouter [`z-ai/glm-4.5-air:free`](https://openrouter.ai/z-ai/glm-4.5-air:free). Agent-focused MoE, SWE-Bench Verified ~59.8%, 131K context. [[OpenRouter](https://openrouter.ai/z-ai/glm-4.5-air:free), 2026]
3. **gpt-oss-120b** — free on OpenRouter; 117B MoE with function calling / tool use, 131K context. Has public coding/agentic benchmarks (open-weight OpenAI release). [[OpenRouter free models](https://openrouter.ai/collections/free-models), 2026]
4. **DeepSeek V4 Flash (free)** — OpenCode Zen free tier (limited-time, data-logged). Recent DeepSeek line with public coding benchmarks. [[OpenCode Zen](https://opencode.ai/docs/zen/#pricing), 2026]
5. **MiMo-V2.5** (free) — OpenCode Zen free tier / open weights; appears on coding boards (WhatLLM single-shot Coding Index 53.8). [[OpenCode Zen](https://opencode.ai/docs/zen/#pricing), 2026; [WhatLLM](https://whatllm.org/best-llm-for-coding), 2026]

_Caveat on Free: these rotate fast and carry rate/context limits and (for Zen/NVIDIA) data-logging. None reach the agentic frontier — Kimi K2.6 is clearly the best of them. Composer 2.5 is **not** here (no free route; Cursor SDK is usage-priced)._

## Final Pick
**Claude Opus 4.8 (max)** — the top **Absolute** performer. Coding is high-stakes and quality-critical (a wrong edit costs far more than the model), so the task's stakes point to the Absolute tier. Opus 4.8 leads the agentic-coding evidence that matters most here — highest SWE-Bench Pro (69.2%) and SWE-Bench Verified (88.6%), and it supersedes the Opus 4.7 that topped the required AA Coding Agent Index. **If cost or volume dominates**, pick the top Budget model, **Cursor Composer 2.5 (Standard tier, via Cursor SDK/CLI)** — it sits 3rd on the agentic Coding Agent Index at ~10–60× lower cost per task, the best quality-per-dollar at the frontier's edge.

## Caveats
- **Benchmarks ≠ production.** Scaffolds, prompts, tool wiring, retry/routing policy, and harness choice (e.g. Terminus-2 vs alternatives) routinely reverse these orderings in real workflows. Treat the ranking as directional.
- **Board disagreement & a flagged loophole.** DeepSWE crowns GPT-5.5 and reported a benchmark-gaming concern for Claude on one eval; CursorBench is vendor-run and treated as display-only. I leaned on the independent agentic Coding Agent Index per the skill's tie-break order; reasonable people could rank GPT-5.5 #1.
- **Staleness handled.** The AA Coding Agent Index article (May 21) and CursorBench v3.1 (June 1) predate or omit Claude Opus 4.8 (released May 28); I promoted Opus 4.8 above Opus 4.7 because it improved the exact component benchmarks. Composer 2.5 is the latest Cursor model (no Composer 3 as of 2026-06-02).
- **Pricing assumptions.** Budget eligibility uses standard API rates (not Batch/Flex/priority); Composer 2.5 qualifies only on its **Standard** tier, not the default Fast tier. Kimi's cap test uses Moonshot's separate input/output rates, not blended aggregator figures.
