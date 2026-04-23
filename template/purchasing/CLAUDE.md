# Research Workspace — Purchasing Research

**Variant:** `purchasing`
**Name:** <WORKSPACE_NAME>
**Purchase target:** <PURCHASE_TARGET>

## Purpose

Structured research to support a purchasing decision. Flow is: **spec → candidates → shortlist → decision**. Every stage is logged so the reasoning survives long after the purchase.

## Startup

Run `/research-space:research-init` at the start of each session to load the current spec, candidate set, and any decision notes.

## Workflow

### 1. Spec (in `context/`)

- `context/spec.md` — what you're trying to buy and why. Must-haves, nice-to-haves, hard exclusions.
- `context/criteria.md` — scoring dimensions (price, performance, warranty, availability locally, reviews, vendor trust, etc.) with weights.
- `context/constraints.md` — budget, delivery window, regional/legal constraints (VAT, import, locale-specific availability).

### 2. Candidates (in `candidates/`)

- One folder per candidate: `candidates/<product-slug>/`
  - `dossier.md` — what it is, manufacturer, model, key specs, price(s), where available.
  - `sources.md` — list of `sources/<slug>.md` files referenced.
- Populate via `/research-space:source-log <url>` for product pages, reviews, teardowns.

### 3. Shortlist (in `shortlist/`)

- `shortlist/matrix.md` — scoring matrix across criteria; 2–4 finalists.
- `shortlist/tradeoffs.md` — narrative on what each finalist trades off.

### 4. Decision (in `decisions/`)

- `decisions/decision-YYMMDD.md` — the final pick, reasoning, and explicitly-rejected alternatives (with why).
- Include purchase confirmation details (vendor, price paid, date) once executed.

## Primitives

- `/research-space:source-log <ref>` — log a product page, review, or teardown.
- `/research-space:summarize-sources` — roll up reviews and benchmarks across candidates.
- `/research-space:deep-dive <topic>` — deep comparison between 2–3 candidates or a specific dimension.
- `/research-space:export-report` — compose a purchase brief.

## Conventions

- Tag each source with the candidate: `candidate:<slug>`.
- Record **price**, **currency**, **vendor**, and **captured** date on every product-page source — all four drift fast.
- Flag fake-review risk and vendor-authored content in summaries.
- Keep manufacturer claims and independent tests in separate front-matter categories.

## Rules

- No final decision without a populated `shortlist/matrix.md`.
- Reject sources older than 18 months for pricing/availability claims. Keep them only for fundamentals (architecture, specs, design intent).
- If the purchase is ultimately not made, still log `decisions/decision-YYMMDD.md` explaining why — that's valuable reference for future purchases.
