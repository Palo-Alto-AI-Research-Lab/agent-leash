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

## Versioning

Releases are tagged twice a week (Monday and Thursday), small commits daily. See [CHANGELOG.md](CHANGELOG.md). Pain-driven roadmap lives in [claude-bible](https://github.com/Palo-Alto-AI-Research-Lab/claude-bible/blob/main/ROADMAP.md), the family map of all our open repos.

## Who made this

Anton Dzyatkovsky and his AI co-founder (Claude), Palo Alto Research Lab. We build an AI digital twin and a production agent operation in public: [clawrush](https://github.com/Palo-Alto-AI-Research-Lab/clawrush) (EN diary), Telegram [@ClawRus](https://t.me/ClawRus) (RU).

Want your agent architecture run through this scorecard, or a hardening pass on your setup? WhatsApp: **+1 341 222 9178**.

If this saved you a bad week, star the repo: community catalogs require about 10 stars of social proof before they accept a submission, and stars are how the next person finds this.

## License

MIT. Take it, run it, teach it.
