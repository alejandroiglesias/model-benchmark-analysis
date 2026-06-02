---
name: model-benchmark-analysis
description: >-
  Research current AI model benchmarks live and rank the best models for ONE
  specific task or agent role given as an argument. Examples: "coding", "web
  browsing", "data analysis", "image generation", "trading", "OCR", "creative
  writing", or freeform agent roles like "ad creative producer agent",
  "infoproduct book writer agent", or "customer support triage agent". Runs a
  deep web investigation across benchmark leaderboards (Artificial Analysis,
  Vellum, Scale, plus task-specific boards) and outputs three Top-5 rankings —
  Absolute performance, Budget-eligible near-frontier, and Free — followed by a
  single final model recommendation with reasoning. Use this whenever the user
  runs /model-benchmark-analysis, or asks which model or LLM is best for a given
  task or agent, asks to rank or compare models for a use case, asks for the
  best cheap/budget or best free model for something, or asks which model to
  pick for an agent role. Always prefer this over answering from memory, because
  model rankings and API pricing change constantly and must be verified live.
---

# Model Benchmark Analysis

Given a single task category or agent role, research the **current** state of AI
model benchmarks and return three focused Top-5 rankings plus one final pick:

- **Top 5 — Absolute Performance** (best raw capability, cost ignored)
- **Top 5 — Budget-Eligible Near-Frontier** (best among models under a price cap)
- **Top 5 — Free** (best among genuinely free models with public benchmarks)
- **Final Pick** — one model, chosen by matching the task's stakes to the right tier

The output is decision-support for "which model should I use for *this*." It is
not a generic intelligence leaderboard — rank by **fit for the task**, not by
overall smarts.

## Invocation and arguments

The argument is the task/role to analyze, optionally followed by flags:

- `/model-benchmark-analysis coding`
- `/model-benchmark-analysis web browsing`
- `/model-benchmark-analysis ad creative producer agent`
- `/model-benchmark-analysis trading --full`

**Flags:**
- `--full` — also emit the complete methodology preamble (sources, normalization,
  weighting, pricing assumptions, budget rules, limitations), matching a formal
  report. Without it, default to the **focused report** (lean methodology note +
  the three lists + final pick + short caveats).

If no argument is given, ask the user what task or agent role to analyze. Do not
guess.

## Operating principles

These exist because the whole point is a *trustworthy, current* answer:

- **Live data only, never memory.** Model versions, leaderboard positions, and
  API prices churn weekly. Use web search/fetch to pull current numbers. If you
  cannot verify something live, say so rather than asserting it.
- **Cite everything.** Every benchmark figure and every price gets a source link.
  State the **date checked** (use today's date from the environment).
- **Rank by category fit, not general intelligence.** A model that tops HLE may
  be mediocre at OCR or tool-calling. Weight task-direct evidence highest.
- **Don't overclaim precision.** These are noisy, partial signals. Normalized
  scores are directional, not exact. Say so.
- **Don't pad lists.** If fewer than 5 models genuinely qualify for a tier
  (especially Budget or Free), return fewer and state explicitly why. Never fill
  a list with weak or out-of-cap models just to reach five.
- **Benchmarks ≠ production.** Always warn that scaffolds, prompts, tools, retry
  policy, and routing can reverse rankings in real workflows.

## Workflow

### 1. Interpret the task into capabilities

Decide what the task actually demands. Two cases:

- **Known category.** If the argument matches (closely enough) one of the
  predefined task categories or agent roles, load `references/category-playbooks.md`
  and use its benchmark mapping and special rules. These mappings encode required
  benchmarks (e.g. coding *must* include the Artificial Analysis Coding Agent
  Index, CursorBench, and DeepSWE; trading *must* include Vending-Bench 2;
  design must *not* be ranked on image generation).
- **Novel task/role.** Decompose the role into its underlying capability
  dimensions and assign each an explicit weight reflecting how much the task leans
  on it. Example — *"ad creative producer agent"* → creative/marketing copy
  (~45%), image generation (~30%), brand/visual taste (~15%), tool use & iteration
  (~10%). Example — *"infoproduct book writer agent"* → long-form creative/
  nonfiction writing (~50%), instruction following & structure (~25%), long-context
  coherence (~15%), factual reliability (~10%). State the breakdown up front and
  invite the user to rerun with different emphasis. Then map each capability to
  benchmark families using `references/benchmark-sources.md`.

When direct benchmarks for the task don't exist, choose the closest **proxies**,
say which they are, and explain the substitution. This is expected for fuzzy roles
(social, email, calendar, pitcher, checker, etc.) — be honest that they are
proxy-heavy.

### 2. Build the research plan

List the specific leaderboards and benchmarks you'll consult for each capability,
drawing primary sources and task-specific boards from `references/benchmark-sources.md`.
Prefer recent data and recent model versions; do not rank a legacy model highly if
a newer one clearly surpasses it.

### 3. Gather current data

Use web search/fetch to pull live figures from:
- **Primary** general boards: Artificial Analysis, Vellum, Scale.
- **Task-specific** boards for the capabilities at hand.
- **OpenRouter rankings** — *secondary signal only*. Use the per-domain usage
  charts at most as an adoption/practicality tie-breaker, models-to-watch input,
  and at most ~5–10% of weighting when directly relevant. Explain that it reflects
  user/app mix, price and free-tier effects, routing defaults, availability, and
  selection bias — not controlled task quality. Never treat it as primary evidence.
- **Free-model registries** to determine free eligibility (see
  `references/budget-and-free-rules.md`).
- **Current API pricing** pages for budget eligibility.

**Expect client-rendered boards.** Artificial Analysis and the OpenRouter
rankings page are JavaScript-rendered, so a plain fetch often returns no tables.
When that happens: use a browser tool if one is available; otherwise fall back to
reputable mirrors/aggregators (Vellum, BenchLM, Kilo, board-specific sites like
deepswe.net, and the provider's own pages), and say which source each number came
from. If you can only get partial data for a required board, note the gap rather
than inventing numbers.

Record each number with its source and the date. If a model lacks a public
benchmark for this category, you cannot rank it for quality — note it as
unrankable rather than guessing.

**What counts as selectable.** A model is selectable if it can be driven
**programmatically through any first-party interface** — a REST API, an SDK, or an
agent CLI all count. A raw REST endpoint is not required. For example, Cursor's
Composer models have no standalone REST API but are usable via the Cursor SDK/CLI,
so they **are** selectable and should be ranked (note the access path, e.g. "via
Cursor SDK"). Price-filter them using whatever usage-based pricing that interface
publishes; if it's only available under a subscription with no per-token rate, say
so and treat it as not budget-eligible rather than dropping it from the Absolute
list.

Only treat a model as **not-selectable** when it has *no programmatic access at
all* — e.g. invitation-only previews with no API, SDK, or CLI. Don't put those in
the Top-5 lists; surface them in a short note ("highest raw scores but not publicly
selectable — no programmatic access") so the user knows they exist. When a playbook
*requires* a specific model (e.g. the latest Cursor Composer), include it as a
ranked candidate via its SDK/CLI access and explain the access path.

### 4. Normalize and weight

- Within each benchmark, min–max normalize scores to 0–1 so different scales are
  comparable.
- Combine benchmarks per capability: **direct task benchmarks weighted highest,
  adjacent agent/tool-use benchmarks second, general intelligence/reasoning only
  as a tie-breaker.**
- For multi-capability tasks, combine capability scores using the weights you
  declared in step 1.
- **When boards disagree on ordering** (common — different scaffolds crown
  different winners), break ties in this priority: independent boards over
  vendor-run ones; long-horizon/agentic evals over single-shot; more recent runs
  over older; then general reasoning. State which signal you trusted and why,
  since the choice is judgment, not arithmetic.
- State the weighting method in plain language. Don't invent false decimals — if
  evidence is thin, say the ranking is approximate.

### 5. Apply the budget filter (for the Budget list)

Read `references/budget-and-free-rules.md` and apply the cap **before** ranking.
For text/reasoning/coding/agent models a model qualifies only if **both** are true:
`input ≤ $2.50 / 1M tokens` **AND** `output ≤ $9.00 / 1M tokens`, at standard API
pricing (not batch/flex/priority discounts). For image and video, first **define
and state** a per-image/per-edit or per-minute cap, then filter. Rank the survivors
by the same normalized category score from step 4 — this is "best performers among
the affordable," not score-per-dollar and not "second-best expensive frontier."

### 6. Apply the free filter (for the Free list)

Keep only models that are genuinely free (per the registries in
`references/budget-and-free-rules.md`) **and** have a public benchmark for this
category. Rank by normalized category score. Use reliability, latency, tool-use,
and platform constraints as tie-breakers. If fewer than 5 qualify, show fewer and
say so.

### 7. Choose the single final pick

Recommend exactly one model by matching the task's stakes to a tier:
- **High-stakes / quality-critical / low-volume** → take the top **Absolute**
  performer.
- **Cost- or volume-sensitive but still needs to be good** → take the top
  **Budget-Eligible** performer.
- **Simple, high-volume, low-risk, and a free model is genuinely sufficient** →
  take the top **Free** performer — but only if you are confident free is enough.

State *why* you chose that model and that tier for this specific task. If you pick
a free model, justify that the task tolerates it.

## Output format

Default = **focused report**. Use this structure:

```
# Model Benchmark Analysis — <task/role>
Date checked: <today>

**Capability read:** <one-line decomposition + weights, if a novel/composite task>
**Benchmarks used & weighting:** <2–4 sentences: which boards, how combined, any proxy/caveat>

## Top 5 — Absolute Performance
1. <model> — <1-line: key score(s) + source>
... (down to 5)

## Top 5 — Budget-Eligible Near-Frontier
_Price cap: input ≤ $2.50/M, output ≤ $9/M (standard API). <for image/video: state the per-unit cap used>_
1. <model> (<input/output price + source>) — <1-line rationale>
... (down to 5; if fewer qualify, list them and say why)

## Top 5 — Free
_Free via: <which registries>_
1. <model> — <1-line rationale + where it's free>
... (down to 5; if fewer qualify, list them and say why)

## Final Pick
**<model>** — <which tier and why it fits this task's stakes/volume/budget>

## Caveats
<1–3 lines: benchmark≠production; proxy-heavy if applicable; pricing assumptions>
```

When `--full` is passed, prepend a full **Methodology** section first (sources
used, normalization method, weighting method, pricing assumptions, the budget
eligibility rule, image/video budget rules, and limitations), then the same
sections above. Keep per-item rationales but you may expand the "Benchmarks used"
into a fuller "Relevant Benchmarks Used" paragraph as in a formal report.

Always include the source citations (links) and the date checked, in both modes.

## Reference files

- `references/category-playbooks.md` — benchmark mappings and special rules for the
  predefined task categories and agent roles, plus the method for novel roles.
  **Read this first** in step 1.
- `references/benchmark-sources.md` — catalog of benchmark sources by capability,
  with URLs and what each measures. Use in steps 2–3.
- `references/budget-and-free-rules.md` — the exact budget caps, the image/video
  cap procedure, the free-model registries, and the OpenRouter guidance. Use in
  steps 5–6.
