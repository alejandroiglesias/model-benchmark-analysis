# Budget, Free, and OpenRouter Rules

All prices change frequently. **Verify every price live** from the provider's
official pricing page (or a current aggregator) on the day you run this. The
numbers in the worked examples below are illustrative of *how the rule works*,
not a price table to reproduce.

## Text / reasoning / coding / agent budget cap

A model is **budget-eligible** only if **both** conditions hold at standard API
pricing:

```
input_price_per_1M_tokens  <= 2.50 USD
AND
output_price_per_1M_tokens <= 9.00 USD
```

- The cap is **inclusive**: a model priced exactly at $2.50 input or $9.00 output
  still qualifies. A boundary value is fine; only prices *above* the cap fail.
- Use **standard** API prices — not Batch, Flex, priority, or promotional rates.
  If a model is only cheap via a non-standard tier, note that and treat it as not
  qualifying on standard pricing.
- This is **not** score-per-dollar, and **not** "the second-best expensive
  frontier models." After filtering, rank survivors by the same normalized
  category score from the main workflow. The list answers: *"Among models clearly
  within the affordable tier, which perform best for this category?"*
- Do **not** rank a weak model highly just because it is cheap. Do **not** include
  an expensive model just to reach five. If fewer than five qualify, show fewer
  and say so explicitly.

**Illustrative examples of the rule (verify current prices before relying on these):**
- Qualifies — input and output both under cap: e.g. a model at $1.50 in / $4 out,
  or $0.95 in / $4 out, or $1.25 in / $2.50 out.
- Fails on output — e.g. $2.00 in / $12 out, or $1.75 in / $14 out: output exceeds
  $9 even though input is fine.
- Fails on both — e.g. $5 in / $30 out, or $6.25 in / $25 out, or $3.00 in /
  $15 out.

## Image generation budget cap

Image models price per image / per edit / per token-equivalent, so the token cap
does not apply. Instead:

1. Choose and **state** a clear per-unit cap. Default: **≤ $0.08 per
   1024-equivalent image or edit** where per-image pricing exists; otherwise a
   comparable token-equivalent image cost.
2. Apply the cap before ranking. Rank only budget-eligible image models by their
   category benchmark score (e.g. text-to-image and edit arena placement).
3. If a model's image pricing is unavailable, exclude it from the budget list
   unless you explain the gap and explicitly label it "pricing unavailable."
4. Do not mix subscription-only, promotional, and API usage pricing without
   explaining the difference.

## Video generation budget cap

1. Choose and **state** a clear per-minute cap. Default: **≤ $6 per generated
   minute** of video at published API pricing.
2. Apply before ranking; rank survivors by video-arena/benchmark score.
3. Same "pricing unavailable" labeling rule as images.

## Free-model eligibility

A model qualifies for the **Free** list only if it is genuinely free to use
**and** has a public benchmark for the category being analyzed (a model with no
benchmark for this category cannot be quality-ranked — note it as unrankable, do
not guess). Check these registries for current free availability:

- **OpenCode Zen** free tier — https://opencode.ai/docs/zen/#pricing
- **OpenRouter free models** — https://openrouter.ai/collections/free-models
- **NVIDIA build** preview/free endpoints —
  https://build.nvidia.com/models?filters=nimType%3Anim_type_preview
- **Manifest free models** — https://manifest.build/free-models/

Free offerings rotate quickly (models get added, gated, or removed). Confirm the
specific model is free *today* on the provider you'd actually use, and name that
provider in the rationale. Watch for rate limits, context caps, or
provider-specific restrictions and mention them when meaningful. If fewer than 5
free models qualify with real benchmarks, return fewer and state why — do not pad
with models that lack category benchmarks.

**"Free" means a free hosted endpoint, not "open weights."** Many open-weight
models (e.g. various DeepSeek/GLM/Qwen releases) are free to *self-host* but
*paid* on the hosted endpoints you'd actually call. Classify by how the user would
realistically use it:
- Available on a genuinely free hosted endpoint (no card, e.g. an OpenRouter
  `:free` route, OpenCode Zen free tier, NVIDIA preview) → **Free list**, and name
  the provider + its rate/context limits.
- Open weights but only available paid on hosted endpoints (self-hosting aside) →
  **Budget list** if its hosted price is under the cap, not the Free list.
- If a model is free on a hosted route *and* the same model is paid elsewhere,
  it can appear on the Free list (note the free route) and, if its paid price is
  under cap, also be mentioned in Budget — just be explicit about which is which.

## OpenRouter rankings — secondary signal only

Use the per-domain leaderboard/usage charts at https://openrouter.ai/rankings
**only** as a real-world adoption/practicality signal — never as primary
benchmark evidence. It may inform tie-breaks, platform/practicality notes,
"models to watch," and at most ~5–10% of a category's weighting when the signal is
directly relevant. Always explain that OpenRouter rankings reflect user/app mix,
price and free-tier effects, routing defaults, availability, and selection bias —
not controlled task quality.

## Pricing-source hygiene

- Cite the pricing page for every price you use, with the date checked.
- If a price is missing, state the assumption you're making and flag it.
- Prefer the provider's first-party pricing; use aggregators (e.g. Artificial
  Analysis, Vellum, OpenRouter) to cross-check, noting that aggregators sometimes
  list blended or provider-specific prices.
