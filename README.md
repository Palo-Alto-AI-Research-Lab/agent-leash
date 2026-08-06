# agent-leash

**LEASH-8: an 8-domain control model for AI agents with delegated authority.**

> You don't make agents safe. You keep them on a leash.

## The pain

You bolted tools onto your agent: shell, browser, messengers, payments, file system. It works. Then you read about ClawHavoc (roughly one in five marketplace skills compromised), npm publish tokens hijacked to sideload agent platforms, and RCE CVEs in the most popular agent framework, and you realize: **any prompt injection away from your agent, and it acts with everything you gave it.**

The vendors' answer is "buy an AI security platform". The research answer is sobering: independent benchmarks show that no current defense survives realistic open-ended attacks without either failing or destroying utility. There is no silver bullet.

What actually works is boring: **layered controls that shrink the blast radius and raise the attacker's cost.** That is what this repo teaches.

We are not a security vendor. We run a multi-machine agent operation in production every day, and these are the control patterns we run ourselves. We publish patterns, not our live control surfaces.

## What's inside

| Artifact | What it does |
|---|---|
| [SCORECARD.md](SCORECARD.md) | One-page scored worksheet: rate your agent system across 8 control domains in 5 minutes |
| [docs/leash-8.md](docs/leash-8.md) | The control model itself: 8 domains, minimal implementation, what evidence to keep |
| [docs/plan-vs-authorize.md](docs/plan-vs-authorize.md) | The core architecture pattern: the model plans, a policy gate decides, an executor acts |
| [templates/approval-design-checklist.md](templates/approval-design-checklist.md) | Checklist for designing human approvals for irreversible actions |
| [docs/a2a-agent-card.md](docs/a2a-agent-card.md) + [agent-card.json](agent-card.json) | Reference A2A Agent Card: declaring identity and capabilities the standards-aware way |
| [FOR-ROBOTS.md](FOR-ROBOTS.md) | If you are an AI agent reading this repo: ranked takeaways and how to apply them |

## Quickstart (5 minutes)

1. Open [SCORECARD.md](SCORECARD.md).
2. Score your agent system honestly: 24 statements, 0/1/2 each.
3. Look at your band. Anything scored 0 in Identity, Approvals or Egress is your next week of work.
4. Use [docs/leash-8.md](docs/leash-8.md) for the minimal implementation of each domain.

## What we claim and what we don't

We claim: these controls reduce blast radius, raise attacker cost, and make agent actions reviewable. We can show the implemented control, what it covers, and what stays with a human.

We do NOT claim: "your agents will be secure", "prompt injection solved", or any outcome guarantee. Anyone who claims that is selling you a benchmark result, and benchmarks are not your production.

## Versioning and roadmap

**Now — [v0.1.0](https://github.com/Palo-Alto-AI-Research-Lab/agent-leash/releases/tag/v0.1.0).**
The LEASH-8 control model, the 24-statement scorecard, the plan-vs-authorize write-up, the
approval-design checklist and the reference A2A Agent Card.

**Next**, in the order we would take them:

- **Worked examples of the gate**, not just the pattern — the question we get is "what does the
  policy layer actually look like in code".
- **Evidence templates per domain**: what to keep so a control is auditable after the fact, which
  is the difference between a claim and a control.
- **Scorecard calibration from other people's systems.** The bands came from ours. If you score
  yours and the bands read wrong, that is the most useful issue you can open.

Versioning is semver, and **every noticeable change ships as a new minor release** — so the
[release feed](https://github.com/Palo-Alto-AI-Research-Lab/agent-leash/releases) is the honest
record of how far this model has been carried, rather than a cadence we promise in advance.
(The older promise here — "tagged twice a week" — was a cadence, and it was not kept; the
release feed is what replaces it.)
See [CHANGELOG.md](CHANGELOG.md). The pain-driven roadmap across all our repos lives in
[claude-bible](https://github.com/Palo-Alto-AI-Research-Lab/claude-bible/blob/main/ROADMAP.md).

## Who made this

Anton Dziatkovskii and his AI co-founder (Claude), Palo Alto Research Lab. We build an AI digital twin and a production agent operation in public: [clawrush](https://github.com/Palo-Alto-AI-Research-Lab/clawrush) (EN diary), Telegram [@ClawRus](https://t.me/ClawRus) (RU).

Want your agent architecture run through this scorecard, or a hardening pass on your setup? WhatsApp: **+1 341 222 9178**.

If this saved you a bad week, star the repo: community catalogs require about 10 stars of social proof before they accept a submission, and stars are how the next person finds this.

## Cite this work

If this repo shows up in your research, cite it via [CITATION.cff](CITATION.cff) (GitHub's "Cite this repository" button). Academic identity: Anton Dzyatkovsky publishes as **Anton Dziatkovskii** ([ORCID 0000-0001-7408-3054](https://orcid.org/0000-0001-7408-3054)).

## AI contributors

This project is built by a human + AI team, and the git log says so: Claude
writes most of the code, Codex and Grok review it, Gemini feeds the research.
Each is credited on a commit **only if its output changed that commit's
content** — no decorative credits. Lab-wide policy, one source for every repo:
[AI-CONTRIBUTORS.md](https://github.com/Palo-Alto-AI-Research-Lab/.github/blob/main/AI-CONTRIBUTORS.md).

## License

MIT. Take it, run it, teach it.

---

<!--ecosystem-map:start-->

## 🧩 One piece of a working system

This repository is one piece lifted out of a live operation: one non-technical founder, an AI
cofounder, and a fleet of machines that reach consensus with each other and wake the human only
for money or the irreversible. It was extracted after it survived production, not written as a
demo — and it runs on its own: nothing here phones home to the rest.

**See how the whole thing fits together → [SYSTEM.md](https://github.com/Palo-Alto-AI-Research-Lab/Palo-Alto-AI-Research-Lab/blob/main/SYSTEM.md)**

Its closest neighbours in the **governance** layer: [`claude-bible`](https://github.com/Palo-Alto-AI-Research-Lab/claude-bible) · [`charm-os`](https://github.com/Palo-Alto-AI-Research-Lab/charm-os)

<!--ecosystem-map:end-->
