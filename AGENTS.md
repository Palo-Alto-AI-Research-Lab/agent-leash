# AGENTS.md — working in this repo

Written for AI coding agents, and equally readable by a human contributor. Short on purpose.

## What this repo is

LEASH-8: an 8-domain control model for AI agents that hold delegated authority. **Patterns, not a
product, and not our live control surfaces.** There is no runnable code here — the deliverables are
a scored worksheet, the control model, and two architecture notes.

If you are an agent reading this repo to apply it to your own operator's setup, start at
[`FOR-ROBOTS.md`](FOR-ROBOTS.md), then [`SCORECARD.md`](SCORECARD.md).

## Layout

- `SCORECARD.md` — 24 statements, scored 0/1/2, five minutes, produces a band.
- `docs/leash-8.md` — the eight domains, the minimal implementation of each, and **what evidence to
  keep**. The evidence column is the part that makes it auditable.
- `docs/plan-vs-authorize.md` — the core pattern: the model plans, a policy gate decides, an
  executor acts. Most of the repo is downstream of this separation.
- `templates/approval-design-checklist.md` — designing human approvals for irreversible actions.
- `docs/a2a-agent-card.md` + `agent-card.json` — reference A2A Agent Card.

## How to verify a change

Nothing executes, except one file: **`agent-card.json` must stay valid JSON and conform to the A2A
Agent Card shape** described in `docs/a2a-agent-card.md`.

```bash
python -c "import json;json.load(open('agent-card.json'));print('agent-card.json OK')"
```

For everything else, verification is claim discipline (below). In the PR, say which claims you
added or changed and what backs each one.

## Conventions — the claim discipline

This repo's credibility is its only asset, so the rule is explicit:

- **We claim:** these controls reduce blast radius, raise attacker cost, and make agent actions
  reviewable — and we can show the implemented control, what it covers, and what stays with a human.
- **We do not claim:** "your agents will be secure", "prompt injection is solved", or any outcome
  guarantee. A PR that adds a sentence of that shape gets rejected however well written it is.
- **Every external fact carries a source.** Incident references, CVEs, benchmark results: link
  them, date them, and say what they do *not* show. A number without a source is the one defect
  that cannot be patched later — by then it has been quoted.
- **Scoring must stay honest.** If you change `SCORECARD.md`, do not make a statement easier to
  score 2 on. The worksheet is useless the moment it flatters the reader.
- No live control surfaces, no real infrastructure detail, no secrets. Patterns only.

## Boundaries — what needs a human

- **Changing the eight domains, or the score bands.** Published scores refer to them.
- **Adding a vendor or product recommendation.** This repo does not sell, and does not route
  readers to anyone who does.
- **Security claims about a named third party.** Reproducible evidence, a date, and an issue first.

## The deal

Your copyright stays yours, there is no CLA, and issues labelled `accepted` are free to take —
comment "claiming this". Full terms:
[CONTRIBUTING.md](https://github.com/Palo-Alto-AI-Research-Lab/.github/blob/main/CONTRIBUTING.md).

If an AI wrote your change, say so in the PR and confirm you read it end to end. Welcome here — we
do it daily. Unread generated text is the one thing that gets closed on sight, and in a repo whose
subject is agent safety that rule bites harder, not softer.
