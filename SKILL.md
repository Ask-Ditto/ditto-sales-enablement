---
name: ditto-sales-enablement
description: >
  Generate a complete sales enablement kit from a single Ditto research study
  (300K+ AI personas, 92% overlap with real focus groups). One 10-persona,
  7-question study produces 7 deliverables: competitive battlecard, objection
  handling guide, customer quote bank, one-pager, pitch narrative, ROI
  framework, and demo script. Covers buyer-committee parallel studies and
  quarterly refresh cycles. Designed for sales enablement managers, RevOps
  leads, AEs building collateral, and product marketers producing sales
  materials. Use when the user mentions battlecard, objection handling, sales
  collateral, pitch deck, demo script, win/loss analysis, competitive
  intelligence for sales, or sales enablement.
allowed-tools: Bash(curl *), Bash(python3 *), Read, Grep, WebFetch
argument-hint: "[product name, competitor, or research brief]"
---

# Ditto for Sales Enablement

Generate a complete sales enablement kit — battlecard, objection handling guide,
quote bank, one-pager, pitch narrative, ROI framework, and demo script — from
a single 10-persona, 7-question study. Directly from the terminal.

**Full documentation:** https://askditto.io/claude-code-guide

## What Ditto Does

Ditto maintains 300,000+ AI-powered synthetic personas calibrated to census
data across USA, UK, Germany, and Canada. You ask them open-ended questions
and get qualitative responses with the specificity of real buyer interviews.

- **92% overlap** with traditional focus groups (EY Americas validation)
- A complete sales enablement kit in **~60 minutes**
- Traditional equivalent: 3-6 weeks, $30,000-50,000

## The Promise: One Study, Seven Deliverables

```
One 10-Persona Study (~12 min)
    ├─ 1. Competitive Battlecard (5 min)
    ├─ 2. Objection Handling Guide (5 min)
    ├─ 3. Customer Quote Bank (5 min)
    ├─ 4. One-Pager (5 min)
    ├─ 5. Pitch Narrative (10 min)
    ├─ 6. ROI Framework (5 min)
    └─ 7. Demo Script (10 min)

Total: ~60 min from zero to complete kit
```

Each deliverable maps to specific questions in the framework. The study is
designed so every question feeds multiple outputs.

## Quick Start (Free Tier)

Get a free API key — no credit card, no sales call:

```bash
curl -sL https://app.askditto.io/scripts/free-tier-auth.sh | bash
```

Free keys (`rk_free_`): ~12 shared personas, no demographic filtering.
Paid keys (`rk_live_`): custom groups, demographic filtering, unlimited studies.

## API Essentials

**Base URL:** `https://app.askditto.io`
**Auth header:** `Authorization: Bearer YOUR_API_KEY`
**Content-Type:** `application/json`

## The Sales Enablement Workflow (6 Steps)

### Step 1: Recruit Your Buyer Panel

Target your actual ICP — generic personas produce generic intelligence.

```bash
curl -s -X POST "https://app.askditto.io/v1/research-groups/recruit" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Enterprise IT Decision Makers 30-55",
    "group_size": 10,
    "filters": {
      "country": "USA",
      "industry": ["Technology", "Information Technology"],
      "age_min": 30,
      "age_max": 55
    }
  }'
```

Save the `uuid`. Use `group_size` (not `size`), group `uuid` (not `id`).

**Minimum 10 personas.** Fewer than 10 lacks frequency counts for credible
claims. "7 out of 10 said..." requires 10 personas.

### Step 2: Create Study

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Sales Enablement - [Product Name] vs [Category]",
    "objective": "Generate competitive intelligence and buyer insights for sales collateral",
    "research_group_uuid": "UUID_FROM_STEP_1"
  }'
```

Save the study `id`. Response nests under `data.study` — access via
`response["study"]["id"]`, NOT `response["id"]`.

### Step 3: Ask the 7 Sales Enablement Questions

Ask one at a time. Poll to completion between each (see Polling Strategy).

### Step 4: Poll Until Complete

```bash
curl -s "https://app.askditto.io/v1/jobs/JOB_ID" \
  -H "Authorization: Bearer $DITTO_API_KEY"
```

Wait **45-50 seconds** before first poll, then **20 seconds** intervals.
Poll **ONE** job_id as proxy — all jobs finish together.

### Step 5: Complete the Study

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies/STUDY_ID/complete" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"force": false}'
```

Use `"force": true` to re-run analysis (avoids 409 on already-completed studies).

### Step 6: Extract the 7 Deliverables

Read all Q&A data:

```bash
curl -s "https://app.askditto.io/v1/research-studies/STUDY_ID/questions" \
  -H "Authorization: Bearer $DITTO_API_KEY"
```

Then generate each deliverable using the mapping below.

## The Sales Enablement Question Framework

Every question is designed to feed specific deliverables. The mapping
shows which outputs each question powers.

**Q1 — Pain Language**
> "You're evaluating solutions in [category] for your team. What's your
> biggest pain point right now? Walk me through how you currently solve it."

Feeds: Battlecard (Current State section), One-Pager (problem statement),
Pitch Narrative (Character + Problem), ROI Framework (cost of status quo),
Demo Script (opening hook).

**Q2 — Competitive Intelligence**
> "Compared to [Competitor A] and [Competitor B], what would make you choose
> a new option? What's the minimum bar?"

Feeds: Battlecard (Why We Win, Competitor Strengths, Switching Triggers),
Objection Handling Guide (competitive objections).

**Q3 — First Impressions + Scepticism**
> "If someone presented [your product] to you, what would your first reaction
> be? What excites you most? What makes you sceptical?"

Feeds: Pitch Narrative (opening), Objection Handling Guide (scepticism sources),
Quote Bank (excitement quotes), Demo Script (supporting features).

**Q4 — Hero Feature**
> "What's the ONE thing you'd want this solution to do brilliantly for you?
> Why does that matter more than everything else?"

Feeds: Demo Script (hero feature, opening 90s), One-Pager (key benefit),
Pitch Narrative (guide section).

**Q5 — Switching Triggers + Blockers**
> "What would make you switch from your current solution? What's blocking
> you right now?"

Feeds: Battlecard (switching triggers, timing indicators), Objection Handling
Guide (blocking objections), ROI Framework (cost of inaction).

**Q6 — Price Sensitivity**
> "What would you expect to pay for this? What would feel like a steal
> versus too expensive?"

Feeds: ROI Framework (pricing context, price sensitivity), Battlecard
(pricing positioning vs competitors).

**Q7 — Proof Points**
> "What would you need to see to feel confident recommending this to your
> boss or team? What proof matters most?"

Feeds: Pitch Narrative (closing evidence), Demo Script (social proof section),
One-Pager (evidence section), Quote Bank (proof quotes).

## The 7 Deliverables: Specifications

### 1. Competitive Battlecard

**Sections:**
- **Why We Win** — differentiation themes from Q2
- **Competitor Strengths (Honest)** — what competitors genuinely do well from Q2
- **Landmines** — questions to plant about competitors during deals
- **Quick Dismisses** — one-line rebuttals for common competitor claims
- **Switching Triggers** — timing indicators from Q5
- **Proof Points Required** — what evidence closes the deal from Q7

**ABC Quality Standard (non-negotiable):**
- **A**ccuracy — one factual error = entire card binned by sales team
- **B**revity — scannable in 60 seconds during a live call
- **C**onsistency — standardised format across all competitor cards

**Rule:** NEVER deny genuine competitor strengths. "That's true, and here's
how we approach it differently" is more credible than denial.

### 2. Objection Handling Guide

**Classification:**
- **Primary objections:** cited by 5+ personas
- **Secondary objections:** cited by 2-4 personas

**Category types:** Price / Switching Cost / Security / Feature Gap /
Viability / Learning Curve

**Response framework (AESN):**
1. **A**cknowledge — "That's a fair concern..."
2. **E**vidence — data point or proof from study responses
3. **S**tory — specific persona scenario that addresses the objection
4. **N**ext Step — what to do or show next

**Important:** Do NOT confuse objections with questions. "How does your
API handle rate limiting?" is a question, not an objection.

### 3. Customer Evidence Quote Bank

**Organised by theme:**
- Value and ROI
- Competitive Differentiation
- Pain Points
- Product Excitement
- Feature Priorities

**SIVA Quality Test (every quote must pass):**
- **S**pecific — not a vague generality
- **I**nspiring / **V**ivid — language that creates a picture
- **A**ttributable — includes demographic context (role, industry, company size)
- Theme-aligned — clearly supports one of the five themes

### 4. One-Pager

**Structure:** Problem → Solution → Benefits → Evidence → Next Steps

**Written in CUSTOMER language, not product-team language.**

The customer-language test: if any sentence could appear on a competitor's
one-pager, it is too generic. Replace with specific language from Q1 and Q4.

### 5. Pitch Narrative (StoryBrand Framework)

| Element | Source | Content |
|---------|--------|---------|
| **Character** | Q1 | The buyer and their current situation |
| **Problem (External)** | Q1 | The task they struggle with |
| **Problem (Internal)** | Q1 | The frustration and emotional cost |
| **Problem (Philosophical)** | — | Why it shouldn't be this hard |
| **Guide** | Q3, Q4 | Your product as the trusted advisor |
| **Plan** | Q4 | 3-step clear path to success |
| **Call to Action** | Q7 | Specific next step |
| **Avoid Failure** | Q4, Q5 | What happens if they do nothing |
| **Success** | Q4 | The transformed state |

### 6. ROI Framework

| Component | Source | Calculation |
|-----------|--------|-------------|
| **Cost of Status Quo (annual)** | Q1, Q5 | Time wasted + inefficiency + opportunity cost |
| **Investment Required** | Q6 | Your pricing in context of expectations |
| **Payback Timeline** | — | Month 1-2 (quick wins), 3-6 (adoption), 6-12 (scale) |
| **Price Sensitivity** | Q6 | "Steal" vs "too much" range |

### 7. Demo Script

| Segment | Duration | Source | Content |
|---------|----------|--------|---------|
| **Opening** | 30 sec | Q1 | Lead with their #1 pain. "You told us..." |
| **Hero Feature** | 90 sec | Q4 | The ONE thing they want done brilliantly |
| **Supporting Features** | 90 sec | Q3 | Areas that generated excitement |
| **Integration** | 60 sec | Q1 | How it fits their current workflow |
| **Social Proof** | 30 sec | Q7 | Best proof point from the study |
| **Proactive Objection Handling** | — | Q3, Q5 | Address top 2 objections before they ask |

## Advanced: Buyer-Committee Parallel Studies

Run identical 7 questions across different buyer roles:

```
Group A: End Users (age 25-40, individual contributors)
Group B: Managers (age 30-50, team leads)
Group C: Executives (age 40-60, C-suite / VP)
```

Same questions, different panels. Each role cares about different things:
- **End users:** daily workflow, ease of use, learning curve
- **Managers:** team productivity, reporting, rollout effort
- **Executives:** ROI, risk mitigation, strategic alignment

Produces role-specific battlecards, pitch variants, and demo scripts.

## Advanced: Quarterly Refresh Cycle

Markets shift, competitors launch features, pricing changes.
Re-run the study every quarter to keep materials current.

**Refresh cadence:**
- **Q1:** Full 7-question study (baseline for the year)
- **Q2:** Quick 3-question pulse (Q2, Q5, Q6 only — competitive + pricing)
- **Q3:** Full 7-question study (mid-year refresh)
- **Q4:** Quick 3-question pulse (pre-planning season update)

Stale battlecards lose deals. Quarterly refresh ensures your sales team
always has current competitive intelligence.

## Complete API Reference

### Research Groups

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-groups/recruit` | Recruit buyer panel |
| `POST` | `/v1/research-groups/create` | Create group from agent IDs |
| `POST` | `/v1/research-groups/interview` | AI-assisted recruitment |
| `GET` | `/v1/research-groups` | List groups |
| `GET` | `/v1/research-groups/{id}` | Get group details + profiles |
| `POST` | `/v1/research-groups/{id}/update` | Update group |
| `DELETE` | `/v1/research-groups/{id}` | Archive group |
| `POST` | `/v1/research-groups/{id}/agents/add` | Add agents |
| `POST` | `/v1/research-groups/{id}/agents/remove` | Remove agents |
| `POST` | `/v1/research-groups/{uuid}/append` | Recruit more into group |

**⚠️** `append` uses `group_uuid` (not `group_id`).

### Research Studies

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-studies` | Create study |
| `GET` | `/v1/research-studies` | List studies |
| `GET` | `/v1/research-studies/{id}` | Get study details |
| `POST` | `/v1/research-studies/{id}/complete` | Trigger AI analysis |
| `POST` | `/v1/research-studies/{id}/agents/remove` | Remove agents (curation) |

### Questions

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-studies/{id}/questions` | Ask question (returns job_ids) |
| `GET` | `/v1/research-studies/{id}/questions` | Get all Q&A data |
| `POST` | `/v1/research-agents/{id}/questions` | Quick question to one agent |
| `POST` | `/v1/research-groups/{id}/questions` | Quick question to group |

### Jobs, Sharing, Media

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `GET` | `/v1/jobs/{job_id}` | Poll async job status |
| `POST` | `/v1/research-studies/{id}/share` | Enable/disable sharing |
| `GET` | `/v1/research-studies/{id}/share` | Check share state |
| `POST` | `/v1/media-assets` | Upload image/PDF |

### Agents, NL Requests, Zeitgeist, Free Tier

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `GET` | `/v1/agents/find` | Find matching persona |
| `GET` | `/v1/agents/search` | Search personas |
| `POST` | `/v1/research-study-requests` | NL study request |
| `POST` | `/v1/research-group-requests` | NL group request |
| `GET` | `/v1/research-group-requests/{id}` | Group request status |
| `POST` | `/v1/zeitgeist/surveys/create` | Quick survey |
| `GET` | `/v1/zeitgeist/surveys/{id}/results` | Survey results |
| `DELETE` | `/v1/zeitgeist/surveys/{id}` | Delete survey |
| `POST` | `/v1/free/questions` | Free-tier question |

## Demographic Filters

| Filter | Type | Examples | Notes |
|--------|------|----------|-------|
| `country` | string | `"USA"`, `"UK"`, `"Canada"`, `"Germany"` | Required. Only these 4 |
| `state` | string | `"TX"`, `"CA"` | **2-letter codes ONLY** |
| `city` | string | `"Austin"`, `"London"` | Narrows pool |
| `age_min` | integer | 25, 30 | Recommended |
| `age_max` | integer | 50, 60 | Recommended |
| `gender` | string | `"male"`, `"female"`, `"non_binary"` | Optional |
| `is_parent` | boolean | `true`, `false` | Optional |
| `education` | string | `"bachelors"`, `"masters"` | Optional |
| `industry` | array | `["Technology", "Healthcare"]` | Recommended for B2B |

**NOT supported:** `income`, `employment`, `ethnicity`, `political_affiliation`.

## Media Attachments

Upload product screenshots, competitor comparisons, or pricing pages:

```bash
curl -s -X POST "https://app.askditto.io/v1/media-assets" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "data_url": "https://example.com/product-screenshot.png",
    "filename": "product-dashboard.png",
    "mime": "image/png"
  }'
```

Pass as `attachments` when asking Q3 or Q4 for richer visual reactions.

## Polling Strategy

| Study size | First poll delay | Subsequent interval |
|-----------|-----------------|---------------------|
| 10 personas | 45-50 seconds | 20 seconds |
| 15+ personas | 60 seconds | 20 seconds |

Poll ONE job_id as proxy. All jobs from the same question finish together.

## Response Structure

**Study creation:** `response["study"]["id"]` (nested under `study` key)

**Question responses:** `response_text` (may contain HTML), `agent_name`,
`agent_age`, `agent_occupation`, `agent_summary`, `agent_city`, `agent_state`.

**Share link:** Prefer `share_link` field over `share_url`.

## Common Mistakes

- Using generic personas ("US adults 25-55") instead of ICP-matched panels
- Denying genuine competitor strengths (honesty is more credible)
- Confusing objections with questions
- Using product-team language on the one-pager (customer language wins)
- Skipping Q7 proof points (they define what closes deals)
- Letting materials go stale (quarterly refresh)
- Fewer than 10 personas (can't make credible frequency claims)
- Asking closed-ended questions
- Using `size` instead of `group_size`
- Using `response["id"]` instead of `response["study"]["id"]`
- Polling every 10-15s (use 45-50s first, then 20s)

## Limitations

Ditto personas have NOT used your specific product. For:
- Win/loss analysis of actual deals → use CRM data + post-mortem interviews
- Competitor feature parity verification → use product testing
- Exact pricing thresholds → use real deal data
- Customer testimonials with real names → use actual customers

**Recommended hybrid:** Ditto for rapid intelligence and first-draft
collateral, then refine with real customer quotes and deal data.

## Further Reading

- Full API guide: https://askditto.io/claude-code-guide
- Question design: @question-playbook.md
- Deliverable templates: @deliverables.md
- StoryBrand reference: @advanced/storybrand-pitch.md
