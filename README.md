# model-benchmark-analysis

A skill for live-researched AI model benchmark rankings per task or agent role.

Given a task category (e.g. `coding`, `web browsing`, `data analysis`) or a freeform agent role (e.g. `ad creative producer agent`, `infoproduct book writer agent`), this skill:

1. **Decomposes** the task into underlying capability dimensions
2. **Researches** current benchmarks live across primary boards (Artificial Analysis, Vellum, Scale) and task-specific leaderboards
3. **Normalizes and weights** benchmark scores by task fit
4. **Filters** for budget-eligible models ($2.50 input / $9.00 output cap) and genuinely free models
5. **Ranks** three Top-5 lists: Absolute performance, Budget-eligible near-frontier, and Free
6. **Recommends** a single model matched to task stakes

## Usage

```text
/model-benchmark-analysis coding
/model-benchmark-analysis web browsing
/model-benchmark-analysis trading --full
/model-benchmark-analysis ad creative producer agent
```

You can also ask naturally, for example: "Which model should I use for a customer support triage agent?"

**Flags:**

- `--full` — emit complete methodology preamble (sources, normalization, weighting, pricing assumptions, budget rules, limitations) in addition to the default focused report.

## Output

Focused report (default):

- Date checked
- Capability read & benchmarks used (how weighted)
- Top 5 — Absolute Performance
- Top 5 — Budget-Eligible Near-Frontier (with price cap and pricing source)
- Top 5 — Free (with free-tier provider and rate/context limits)
- Final Pick (one model, with reasoning on tier choice)
- Caveats (benchmarks ≠ production, proxy-heavy if applicable, pricing assumptions)

All figures are cited with source links. Models priced or accessed via SDKs/CLIs are included (e.g. Cursor Composer via the Cursor SDK). Models with zero programmatic access (invite-only previews) are surfaced in notes but not ranked.

## Predefined categories

### Task categories

- Coding
- Web browsing
- Data analysis
- Image generation
- Video generation
- Social media
- Email
- Calendar
- Trading
- OCR
- Design (interfaces / brand systems, not image-gen)
- Creative writing

### Agent roles

- Orchestrator agent
- Scout agent
- Diagnoser agent
- Landing page builder agent
- Filmer agent
- Pitcher agent
- Checker agent
- Mobile agent

### Novel tasks

If your argument doesn't match the predefined list, the skill decomposes it into weighted capability dimensions and maps them to benchmark families. Examples:

- `ad creative producer agent` → marketing copy (45%) + image generation (30%) + brand taste (15%) + tool use (10%)
- `infoproduct book writer agent` → long-form writing (50%) + instruction following (25%) + long-context (15%) + factual reliability (10%)
- `customer support triage agent` → instruction following (35%) + tool use (30%) + tone/empathy (20%) + latency/cost (15%)

## Key design principles

- **Live data only.** Model versions, leaderboard positions, and API prices change weekly. Every number is researched and cited on the day you run the skill.
- **Rank by task fit, not general intelligence.** A model that tops HLE may be mediocre at OCR or tool-calling. Weight task-direct evidence highest.
- **Don't overclaim precision.** Normalized scores are directional signals, not exact measurements. Cross-board numbers are not directly comparable.
- **Don't pad lists.** If fewer than 5 models qualify for a tier (especially Budget or Free), return fewer with explicit explanation.
- **Benchmarks ≠ production.** Scaffold, prompts, tools, retry policy, routing, and context management routinely reorder models in real workflows. Treat rankings as starting points, not fixed law.

## Special rules

### Budget eligibility

Text/reasoning/coding/agent models qualify for the Budget-Eligible list only if **both** conditions hold:

- Input ≤ $2.50 USD per 1M tokens
- Output ≤ $9.00 USD per 1M tokens

(The cap is inclusive; exactly $9.00 output qualifies.)

For image generation: ≤ $0.08 per 1024-equivalent image or edit (or comparable token-equivalent).
For video generation: ≤ $6 per generated minute.

After filtering, rank survivors by normalized category score — not by score-per-dollar, and not by "second-best expensive frontier." The list answers: _"Among models clearly within the affordable tier, which perform best?"_

### Free eligibility

A model qualifies for the Free list only if it is available on a genuinely free hosted endpoint (e.g. OpenRouter `:free`, OpenCode Zen free tier, NVIDIA preview endpoints) **and** has a public benchmark for the category.

"Free" means a free _hosted_ endpoint, not "open weights." Open-weight models that are only free via self-hosting go in the Budget list (if their hosted price is under cap).

### OpenRouter usage

The per-domain rankings at OpenRouter are used **only as a secondary, optional tie-breaker** (~5–10% weighting max). It reflects user/app mix, price effects, routing defaults, and availability — not controlled task quality. Never treat it as primary evidence.

## Files

- `SKILL.md` — full workflow and instructions (237 lines)
- `references/category-playbooks.md` — benchmark mappings for all predefined categories and roles; novel-task decomposition method (142 lines)
- `references/benchmark-sources.md` — benchmark source catalog by capability with URLs and what each measures (119 lines)
- `references/budget-and-free-rules.md` — budget caps, free-model registries, OpenRouter guidance, pricing-hygiene rules (120 lines)

## Installation

1. Run `npx skills add alejandroiglesias/model-benchmark-analysis`.
2. Invoke it as `/model-benchmark-analysis coding` or ask naturally.

## Example output

See `model-benchmark-analysis-workspace/smoke-test/coding-report.md` for a live-researched example on the `coding` category (run 2026-06-02).

## License

This skill and all reference materials are open-source and public. Use, modify, and share as you see fit.

## Caveats

- Benchmark leaderboards are constantly updated; rankings can shift week to week.
- Some providers (Artificial Analysis, OpenRouter) use JS rendering, so plain fetches may need fallbacks to mirrors/aggregators. The skill knows to handle this.
- Pricing data is a moving target. Verify the exact rates on the provider's official pricing page for production decisions.
- Trading evidence is especially fragile — benchmarks like Vending-Bench 2 measure economic agency, not securities risk/P&L robustness. Never let a model place real trades without guardrails.
