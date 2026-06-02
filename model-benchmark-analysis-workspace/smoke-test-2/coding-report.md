# Model Benchmark Analysis — Coding

Date checked: 2026-06-02

**Capability read:** Single, well-defined category. Coding = agentic software engineering (read/edit across files, run terminal commands, run tests, iterate on failures), with single-shot code quality as a secondary signal. Ranked by category fit, not general intelligence.

**Benchmarks used & weighting:** Required boards per the coding playbook — **Artificial Analysis Coding Agent Index** (composite of SWE-Bench-Pro-Hard-AA, Terminal-Bench v2, SWE-Atlas-QnA), **CursorBench v3.1**, and **DeepSWE** (113 tasks / 91 repos / 5 languages) — weighted highest as long-horizon agentic evals. **SWE-Bench Verified / SWE-Bench Pro** and **Terminal-Bench 2.0** used as supporting single-/few-shot signals. General reasoning (AA Intelligence Index) used only as a tie-breaker. OpenRouter programming usage consulted as a ~5% adoption tie-breaker only. Scores are noisy and scaffold-dependent; ordering is directional, not exact. Where boards disagree (notably DeepSWE crowning GPT-5.5 while CursorBench crowns Claude Opus), I weighted the independent agentic boards over the vendor-run CursorBench and noted it.

## Top 5 — Absolute Performance

1. **Claude Opus 4.8** (Anthropic) — #1 on AA Intelligence Index (61.4) and the strongest single-shot SWE results: **SWE-Bench Verified 88.6%**, **SWE-Bench Pro 69.2%** (clear of Opus 4.7's 64.3% and GPT-5.5's 58.6%). Released after the current Coding Agent Index snapshot, which still uses Opus 4.7 as the underlying model. Sources: [AA — Claude Opus 4.8 analysis](https://artificialanalysis.ai/articles/claude-opus-4-8-analysis-and-benchmarks), [Vellum](https://www.vellum.ai/llm-leaderboard).
2. **GPT-5.5 (xhigh)** (OpenAI) — **#1 on DeepSWE at 70%** (16 pts ahead of the field), **#1 on AA Coding Index (59)**, and **#2 on AA Coding Agent Index (65, in Codex)**. The strongest agentic-coding result on the hardest long-horizon board. Sources: [DeepSWE / BenchLM](https://benchlm.ai/benchmarks/deepSwe), [AA Coding Index](https://artificialanalysis.ai/models/capabilities/coding), [AA Coding Agent Index article](https://artificialanalysis.ai/articles/cursor-composer-2-5-coding-agent-index).
3. **Claude Opus 4.7 (max)** (Anthropic) — **#1 on the AA Coding Agent Index (66, in Claude Code)** and **#1 on CursorBench v3.1 (64.8% Adaptive)**; DeepSWE 54% (3rd). Still the reference model in the live Coding Agent Index. Sources: [AA Coding Agent Index article](https://artificialanalysis.ai/articles/cursor-composer-2-5-coding-agent-index), [CursorBench v3.1 / BenchLM](https://benchlm.ai/benchmarks/cursorBench31), [DeepSWE](https://benchlm.ai/benchmarks/deepSwe).
4. **Cursor Composer 2.5** (Cursor) — **#3 on the AA Coding Agent Index (62)**, **CursorBench v3.1 63.2%** (2nd, just behind Opus 4.7), Terminal-Bench 2.0 69.3%, SWE-Bench Multilingual 79.8% — all at ~10–60x lower cost per task. Selectable programmatically via the **Cursor SDK (@cursor/sdk)** and **Cursor CLI** (no standalone REST API). Sources: [AA Coding Agent Index article](https://artificialanalysis.ai/articles/cursor-composer-2-5-coding-agent-index), [Cursor SDK](https://cursor.com/blog/typescript-sdk), [Composer 2.5 docs](https://cursor.com/docs/models/cursor-composer-2-5).
5. **Claude Sonnet 4.6** (Anthropic) — Best non-frontier-tier agentic coder: **DeepSWE 32%** (4th overall, well ahead of all open-weight models) and strong SWE-Bench. The natural quality/cost middle tier. Sources: [DeepSWE / BenchLM](https://benchlm.ai/benchmarks/deepSwe), [Vellum](https://www.vellum.ai/llm-leaderboard).

_Note on ordering: GPT-5.5 leads the independent agentic boards (DeepSWE, AA Coding Index) while Claude Opus leads the harness-based Coding Agent Index and Cursor's own CursorBench. VentureBeat reported DeepSWE's authors flagged a possible benchmark-exploitation pattern in some Opus Coding-Agent-Index runs, which is part of why I rank GPT-5.5 above Opus 4.7 on agentic evidence and place the newer Opus 4.8 at the top largely on its single-shot SWE-Bench Pro lead. Reasonable people could swap #1–#3._

## Top 5 — Budget-Eligible Near-Frontier

_Price cap: input ≤ $2.50/M, output ≤ $9/M (standard API)._

1. **Cursor Composer 2.5** ($0.50 in / $2.50 out, standard usage-based tier — [Cursor / TokenCost](https://tokencost.app/blog/cursor-composer-2-5-pricing)) — By far the highest-scoring budget-eligible coder: **#3 AA Coding Agent Index (62)**, CursorBench 63.2%. Accessed via Cursor SDK/CLI; the "fast" tier ($3/$15) is over cap, but the **standard tier is comfortably under cap**, so it qualifies. Effectively near-frontier coding at a fraction of the cost.
2. **Kimi K2.6** (Moonshot) ($0.74 in / $3.50 out on OpenRouter — [OpenRouter](https://openrouter.ai/moonshotai/kimi-k2.6)) — Best open-weight agentic coder: **SWE-Bench Pro 58.6%** (tops the open field, beats GPT-5.4/Opus 4.6), SWE-Bench Verified 80.2%, DeepSWE 24%. Sources: [BuildFastWithAI](https://www.buildfastwithai.com/blogs/kimi-k2-6-review-benchmarks), [DeepSWE](https://benchlm.ai/benchmarks/deepSwe).
3. **GLM-5.1** (Z.AI) (~$1.38 blended — [Kilo](https://kilo.ai/leaderboard); verify per-provider in/out) — DeepSWE 18%, strong LiveCodeBench; a capable cheap agentic coder. Pricing is aggregator-blended — confirm the in/out split on your provider before relying on cap eligibility.
4. **Gemini 3.5 Flash** (Google) ($1.50 blended — [Kilo](https://kilo.ai/leaderboard)) — DeepSWE 28% (5th overall, ahead of all open-weight models here) at low cost; good fast-tier budget option. Confirm in/out split.
5. **DeepSeek V4 Pro** (DeepSeek) (~$1.61 blended — [Kilo](https://kilo.ai/leaderboard)) — Cheap and widely available, but DeepSWE only 8%; included as the 5th budget option with the explicit caveat that it is materially weaker on agentic coding than the four above.

_Caveat: prices 3–5 come from an aggregator (Kilo) as blended figures; I could not confirm separate standard input/output rates live for each, so treat their cap-eligibility as provisional. Composer 2.5 and Kimi K2.6 have confirmed in/out standard prices under cap._

## Top 5 — Free

_Free via: OpenRouter `:free` routes (per the [free-models collection](https://openrouter.ai/collections/free-models))._

1. **gpt-oss-120b** (OpenAI, open-weight) — Free on OpenRouter; strongest free coder with a real benchmark: ~62% SWE-Bench Verified class, top open-source on 16x coding eval, native tool/function calling. Sources: [16x eval](https://eval.16x.engineer/blog/gpt-oss-120b-coding-evaluation-results), [OpenRouter free](https://openrouter.ai/collections/free-models).
2. **Kimi K2.6** (Moonshot) — Listed in OpenRouter's free-models collection; has the strongest open-weight coding benchmarks of any free-route model (SWE-Bench Pro 58.6%, Verified 80.2%, DeepSWE 24%). Note: also paid at $0.74/$3.50 elsewhere — use the free route and watch its rate limits. Sources: [OpenRouter free](https://openrouter.ai/collections/free-models), [BuildFastWithAI](https://www.buildfastwithai.com/blogs/kimi-k2-6-review-benchmarks).
3. **GLM 4.5 Air** (Z.AI) — Free on OpenRouter; agent-focused with tool use. Coding benchmarks are decent (GLM-4.6 family ~68% SWE-Bench Verified; Air is the lighter variant — treat as a step below). Sources: [OpenRouter free](https://openrouter.ai/collections/free-models), [Spheron](https://www.spheron.network/blog/open-weight-frontier-model-showdown-2026/).
4. **Poolside Laguna M.1** — Free on OpenRouter, explicitly "optimized for complex software engineering tasks" with tool calling + reasoning. Vendor-described coding focus; no independent agentic-board score found, so ranked below models with public scores. Source: [OpenRouter free](https://openrouter.ai/collections/free-models).
5. **gpt-oss-20b** (OpenAI, open-weight) — Free on OpenRouter; lighter agentic model with function calling. Clearly weaker than 120b; included as the 5th free option for low-stakes/high-volume use. Source: [OpenRouter free](https://openrouter.ai/collections/free-models).

_Note: free routes rotate and carry rate/context limits; confirm the specific `:free` route is live on the day of use. Laguna M.1 lacks an independent coding-board score, so its placement is by vendor claim, not measured agentic performance._

## Final Pick

**Claude Opus 4.8** — for high-stakes / quality-critical coding, take the top **Absolute** performer. It has the best measured single-shot SWE-Bench Verified (88.6%) and SWE-Bench Pro (69.2%) and tops the overall intelligence index, making it the safest choice when a coding mistake is expensive and volume is low. **If cost or volume matters, switch to Cursor Composer 2.5** (top of the Budget list): it lands #3 on the AA Coding Agent Index at ~10–60x lower per-task cost and is fully programmable via the Cursor SDK/CLI — the best value-for-quality coder available right now.

## Caveats

- **Benchmarks ≠ production.** Scaffolds, prompts, tool wiring, retry policy, and routing routinely reverse these rankings in real workflows; the AA Coding Agent Index itself shows the same underlying model scoring differently across Claude Code / Codex / Cursor harnesses.
- **Board disagreement is real.** DeepSWE and AA Coding Index crown GPT-5.5; CursorBench (vendor-run, display-only) and the harness-based Coding Agent Index crown Claude Opus. The independent boards were weighted higher. The DeepSWE authors publicly questioned an Opus Coding-Agent-Index pattern — read top-3 ordering as approximate.
- **Pricing assumptions.** Composer 2.5, Kimi K2.6, Claude Opus, and GPT-5-Codex prices were confirmed live; budget items 3–5 rely on aggregator-blended figures and should be re-verified against per-provider standard in/out rates before committing.
