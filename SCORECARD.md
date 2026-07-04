# LEASH-8 Scorecard

**Rate your agent system in 5 minutes.** For each statement: **0** = not true, **1** = partially true, **2** = fully true and you could show evidence (config, log, screenshot) in under a minute. Be honest: the score is for you, not for marketing.

This measures **control coverage**, not "safety". A high score means smaller blast radius and reviewable actions. It does not mean you cannot be attacked.

## 1. Identity

| # | Statement | 0/1/2 |
|---|---|---|
| 1.1 | Every agent runs as its own principal (its own account/token/session), not under a human's superuser context | |
| 1.2 | Each agent's actions are attributable: given a bad action, you can say which agent, which machine, which task | |
| 1.3 | No two agents share one credential "because it was easier" | |

## 2. Secrets

| # | Statement | 0/1/2 |
|---|---|---|
| 2.1 | Secrets never sit in the agent's always-loaded context (system prompt, memory index, config it re-reads every turn) | |
| 2.2 | Agent credentials are scoped to what that agent actually does (least privilege), not org-wide | |
| 2.3 | You can rotate or revoke any one agent's credentials without breaking the others | |

## 3. Tool and skill supply chain

| # | Statement | 0/1/2 |
|---|---|---|
| 3.1 | Production agents only load tools/skills from an internal allowlist or registry, never straight from the internet | |
| 3.2 | Third-party skills/tools are pinned to versions; an upstream update cannot silently change what runs | |
| 3.3 | New tools get a read-before-run review (code and instructions, not just code) before entering the allowlist | |

## 4. Approvals

| # | Statement | 0/1/2 |
|---|---|---|
| 4.1 | Irreversible action classes are enumerated in writing (money, outbound messages, deletion, publishing, credential use) | |
| 4.2 | Those actions require approval OUTSIDE the model loop: a deterministic gate or a human, not the model's own judgment | |
| 4.3 | Approval arrives out-of-band (separate channel, allowlisted approver identity), so injected text cannot approve itself | |

## 5. Containment

| # | Statement | 0/1/2 |
|---|---|---|
| 5.1 | Agents default to read-only; write access is granted per task, not standing | |
| 5.2 | Risky execution happens in a sandbox/worktree/container, not on the host with everything mounted | |
| 5.3 | Shared config propagates one-way to worker machines (receive-only), so one compromised worker cannot rewrite the fleet |  |

## 6. Egress

| # | Statement | 0/1/2 |
|---|---|---|
| 6.1 | You know every channel through which an agent can send data out (messengers, email, HTTP, git push) and the list is short on purpose | |
| 6.2 | Outbound destinations are allowlisted where the platform permits it | |
| 6.3 | Anything published externally passes a leak scan (secrets, personal data) as a hard gate, not a habit | |

## 7. Observability

| # | Statement | 0/1/2 |
|---|---|---|
| 7.1 | Agent actions land in an append-only log the agent itself cannot edit | |
| 7.2 | Success is visible by counters ("N sent, M failed"), so silent failure is impossible by design | |
| 7.3 | Silence is treated as an incident: a worker that stops reporting triggers an alert, not a shrug | |

## 8. Incident response

| # | Statement | 0/1/2 |
|---|---|---|
| 8.1 | You have a kill switch per agent and per machine, and you have actually used it at least once (drill or real) | |
| 8.2 | You can downgrade autonomy (agent keeps drafting, stops sending) without a full shutdown | |
| 8.3 | You know your time-to-disable: how many minutes from "something is wrong" to "that agent can act no more" | |

## Your band

| Score | Band | What it means |
|---|---|---|
| 0-16 | **Free-running** | Your agent is one prompt injection away from acting with everything you own. Start with domains 4 (Approvals) and 6 (Egress) this week. |
| 17-32 | **Loose leash** | Controls exist but an attacker has real room. Close every 0 in Identity, Approvals, Egress first. |
| 33-42 | **On a leash** | Solid layered posture. Now drill: break your own gates on purpose and measure time-to-disable. |
| 43-48 | **Operator-grade** | You run agents the way the guidance from NIST/CSA/cloud vendors points. Keep evidence fresh; controls rot. |

## What next

- Minimal implementation per domain: [docs/leash-8.md](docs/leash-8.md)
- The single most important pattern behind domains 4-5: [docs/plan-vs-authorize.md](docs/plan-vs-authorize.md)
- Want us to walk your architecture through this scorecard and hand you a written 8-domain report? WhatsApp **+1 341 222 9178**.
