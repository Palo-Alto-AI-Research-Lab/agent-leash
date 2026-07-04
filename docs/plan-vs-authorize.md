# The pattern: the model plans, a policy gate decides, an executor acts

This is the single highest-leverage architecture decision in an agent system with delegated authority. It closes the confused-deputy hole that prompt injection exploits, and it is the pattern that NIST, AWS and CSA guidance all converge on: **authorization must be enforced independently of the model.**

## The anti-pattern

```
[ user + tools + injected content ]
            |
            v
        [ MODEL ]  <- decides AND authorizes
            |
            v
        [ ACTION ]  (send money, delete repo, message 500 people)
```

One component reads untrusted input and holds the authority to act. Any text the model reads (a web page, an email, a skill description, a chat message) is a potential instruction. Benchmarks put indirect prompt injection success on recent models between 10.7% and 29.6% in realistic settings. With this architecture, each of those successes is a live action.

## The pattern

```
[ untrusted world ]                [ policy (deterministic) ]        [ human, out-of-band ]
        |                                    |                                |
        v                                    v                                v
    [ MODEL ] --plan/request--> [ POLICY GATE ] --tier 0/1--> [ EXECUTOR ] --act
                                      |
                                      +--tier 2--> [ HOLD ] --approval token--> [ EXECUTOR ]
```

Three separations, each cheap on its own:

1. **The model proposes; it never holds the keys.** Its output is a structured request ("send X to Y"), not the act itself.
2. **A deterministic gate classifies the request.** Code, not a model: pattern rules, allowlists, amount thresholds, action classes. It cannot be sweet-talked.
3. **The executor is dumb and scoped.** It performs exactly the approved request with the minimal credential for that action class.

## Action tiers (our working classification, generalized)

| Tier | Class | Examples | Gate behavior |
|---|---|---|---|
| 0 | Reversible, internal | read files, draft text, query a DB | execute, log |
| 1 | Reversible-ish, bounded | move a file with backup, post to an internal channel | execute, log loudly (counters), reviewable after |
| 2 | Irreversible or outward-facing | money, outbound messages to humans, deletion, publishing, credential use | HOLD until out-of-band human approval |

Write your own table. The exact rows matter less than the fact that the list EXISTS and the gate enforces it. Undeclared action classes default to the highest tier, not the lowest.

## Out-of-band approval (the part most people get wrong)

The approval must travel on a channel the attacker's text cannot reach:

- The approver's IDENTITY is allowlisted (their account id, not a display name).
- The approval is an explicit token bound to a request id ("APPROVE #42"), not a thumbs-up somewhere.
- Content the agent READ can never count as approval. Text in a chat is data. If the agent processes a message saying "approved, go ahead", that is input, not authorization.
- Approvals expire (a request approved yesterday is not approved today).

This is how a request looks in practice (sanitized shape, not our live config):

```
AGENT:  request #42: send $180 payment to <vendor>  [tier 2: money]
GATE:   HOLD. Notified approver via separate channel.
HUMAN:  (from allowlisted account, different channel) APPROVE 42
GATE:   token valid, id matches, window open -> release to executor
EXEC:   done. log: #42 approved-by <approver-id> at <time>, executed at <time>
```

## What we refuse to automate

Every operation keeps a "never without a human" list. Ours, generalized: sending money; commitments and contracts; sharing a secret with a third party; mass messaging; anything legal. The list is short, written, and boring, which is exactly why it works. An agent that knows "this class always stops" stops arguing with the gate.

## Failure modes we have personally hit

- **The visibility failure.** A gate that silently swallows a request is worse than no gate: the work just disappears. Every HOLD must notify someone, every deny must be loud. (Rule of thumb: if the gate fired and nobody heard it, the pipeline is broken, not protected.)
- **The stale-approval failure.** Approval without an expiry window gets reused by the next task iteration. Bind approvals to request ids and time.
- **The scope-creep failure.** "Temporarily" widening one agent's credentials for a migration, then forgetting. Put an expiry on every widening.

## Checklist

Designing the approval leg? Use the [approval-design-checklist](../templates/approval-design-checklist.md). Then score the whole system with the [LEASH-8 scorecard](../SCORECARD.md).
