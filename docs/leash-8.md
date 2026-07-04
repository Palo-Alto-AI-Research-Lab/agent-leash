# LEASH-8: the control model

**L**ayered **E**nforcement, **A**uditability and **S**upply-chain **H**ygiene, across **8** domains.

The premise, supported by current benchmarks (AgentDojo, ASB, AgentDyn, LivePI): no single defense survives realistic attacks. Filters get bypassed, "hardened prompts" get outreasoned, detectors miss malice split across code and instructions. What holds is what has always held in security: **layers that bound what a compromised component can do.**

The model is deliberately implementation-agnostic. Each domain below states what it is, why it earns its place, the minimal implementation, and the evidence you should be able to show.

## 1. Identity: every agent is its own principal

**Why:** if agents share a human's session or one god-token, one injected agent equals total compromise, and you cannot even tell which one it was.

**Minimal implementation:** separate account/token/API-key per agent role; per-machine identity for fleet members; action attribution (agent, machine, task) stamped into logs.

**Evidence:** show two agents, show two credentials; show a log line naming its author.

## 2. Secrets: short-lived, scoped, out of context

**Why:** a secret inside the model's always-loaded context is one exfiltration prompt away from leaving. A long-lived broad token turns a small compromise into a long campaign.

**Minimal implementation:** secrets live in a store the agent queries at use time, never in system prompt or persistent memory; scopes match the agent's actual job; rotation does not require redeploying the fleet.

**Evidence:** grep your agent's loaded context for anything that looks like a key. It should come back empty.

## 3. Tool and skill supply chain: an allowlist, not a marketplace

**Why:** the largest real-world agent incidents so far were supply chain: compromised marketplace skills at scale, hijacked publish tokens. Installing a skill is installing code with your agent's privileges.

**Minimal implementation:** internal registry or plain allowlist file; versions pinned; a read-before-run review that covers the skill's INSTRUCTIONS as well as its code (malice hides in prose that models obey); no production install-from-URL.

**Evidence:** show the allowlist; show the pinned version; show one rejected candidate and why.

## 4. Approvals: authorization outside the model loop

**Why:** if the model both decides and authorizes, you built a confused deputy. Injected content becomes instructions; instructions become actions.

**Minimal implementation:** enumerate irreversible classes (money, outbound, deletion, publishing, credentials); route them through a deterministic policy gate; require out-of-band human approval with an allowlisted approver identity for the highest tier. Full pattern: [plan-vs-authorize.md](plan-vs-authorize.md). Design worksheet: [approval-design-checklist](../templates/approval-design-checklist.md).

**Evidence:** trigger a gated action in a test; show it blocked without approval; show the deny path working.

## 5. Containment: read-only by default, blast radius by design

**Why:** the industry keeps re-learning this: capabilities grow, and with standing privilege the blast radius grows with them. Anthropic's own guidance trends the same way: read-only defaults, explicit escalation, sandboxes.

**Minimal implementation:** agents start read-only; writes are granted per task; risky execution in sandboxes/worktrees/containers; fleet config propagates one-way (workers receive, never write back to canon).

**Evidence:** show a worker machine failing (by design) to modify shared canon.

## 6. Egress: few doors, all of them known

**Why:** exfiltration needs an exit. Every messenger, mail account, webhook and git remote your agent can reach is an exit. Most operators cannot list theirs.

**Minimal implementation:** write the list of egress channels; keep it short deliberately; allowlist destinations where possible; put a leak scan (secrets, personal data, live topology) as a HARD gate before anything is published externally.

**Evidence:** the list itself, plus one publish attempt blocked by the leak gate.

## 7. Observability: logs the agent cannot edit, counters that cannot lie

**Why:** in agent systems the scariest failure is the silent one: the job that "ran" and did nothing, the worker that quietly died, the action nobody logged. You cannot review what you cannot see.

**Minimal implementation:** append-only action log outside the agent's write scope; result counters for every routine ("sent 5, failed 0"), so success is measured, not narrated; watchdogs where silence itself raises an alert.

**Evidence:** pick yesterday's run; show its counters; show who wrote the log and prove the agent could not have rewritten it.

## 8. Incident response: kill switch, autonomy downgrade, time-to-disable

**Why:** something will eventually go wrong. The difference between an anecdote and a disaster is how fast you can stop one agent without stopping the business.

**Minimal implementation:** per-agent and per-machine kill switch; an intermediate mode where agents keep drafting but stop sending; a measured time-to-disable; a drill on the calendar.

**Evidence:** your last drill's timestamp and its time-to-disable number.

## How to use this

Score yourself with the [SCORECARD](../SCORECARD.md), then implement the cheapest missing control first. In our experience the order that pays fastest for a small operation: Approvals, then Egress, then Supply chain, then Observability. Identity and Secrets are usually half-done already; Containment and IR mature as you go.

And keep the language honest when you talk about it: you implemented controls with known coverage. You did not become "secure". The difference is not pedantry; it is what you can defend in front of a customer, a regulator, or your own postmortem.
