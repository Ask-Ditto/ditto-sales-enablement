# Ditto for Sales Enablement — Claude Code Skill

Generate a complete sales enablement kit — battlecard, objection handling
guide, quote bank, one-pager, pitch narrative, ROI framework, and demo
script — from a single research study using [Ditto](https://askditto.io)'s
synthetic persona platform. Directly from your terminal via Claude Code.

## What This Skill Does

When installed, Claude Code automatically loads this skill whenever you
ask it to create sales collateral, build battlecards, write objection
handling guides, or generate demo scripts.

Claude will:
- Recruit a panel matching your ICP
- Run a 7-question study designed to feed all 7 deliverables
- Generate each deliverable using proven frameworks (ABC, SIVA, StoryBrand, AESN)
- Produce a complete sales enablement kit in ~60 minutes

## One Study, Seven Deliverables

| # | Deliverable | Framework |
|---|------------|-----------|
| 1 | Competitive Battlecard | ABC Quality Standard |
| 2 | Objection Handling Guide | AESN Response Framework |
| 3 | Customer Quote Bank | SIVA Quality Test |
| 4 | One-Pager | Customer-Language Test |
| 5 | Pitch Narrative | StoryBrand Framework |
| 6 | ROI Framework | Cost-of-Status-Quo |
| 7 | Demo Script | Buyer-Priority Order |

## Installation

```bash
# For a specific project
cp -r ditto-sales-enablement /path/to/your/project/.claude/skills/

# For all your projects (personal skills)
cp -r ditto-sales-enablement ~/.claude/skills/
```

## Setup

Get a free Ditto API key (no credit card required):

```bash
curl -sL https://app.askditto.io/scripts/free-tier-auth.sh | bash
```

Set it as an environment variable:

```bash
export DITTO_API_KEY="rk_free_YOUR_KEY_HERE"
```

## Usage

```
"Build a competitive battlecard for our product vs Salesforce and HubSpot.
Target enterprise IT decision-makers."

"Generate an objection handling guide for our sales team.
Focus on security and pricing concerns."

"Create a complete sales enablement kit for our API product.
10 personas, B2B SaaS buyers aged 30-50."
```

Or invoke directly:

```
/ditto-sales-enablement "battlecard for [product] vs [competitor]"
```

## What's Included

| File | Purpose |
|------|---------|
| `SKILL.md` | Main skill — 7-deliverable workflow, question-to-output mapping, quality standards |

## About Ditto

Ditto maintains 300,000+ AI-powered synthetic personas calibrated to
census data across USA, UK, Germany, and Canada. A complete sales enablement
kit takes ~60 minutes — traditional equivalent: 3-6 weeks, $30,000-50,000.

## Links

- **Ditto:** https://askditto.io
- **Full API Guide:** https://askditto.io/claude-code-guide
- **PMM Skill:** https://github.com/Ask-Ditto/ditto-product-marketing

## License

MIT