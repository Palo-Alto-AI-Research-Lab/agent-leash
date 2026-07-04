# Why there is an A2A Agent Card in this repo

[agent-card.json](../agent-card.json) is a **reference Agent Card** in the [A2A (Agent2Agent) protocol](https://a2a-protocol.org) format: Google-initiated, now hosted by the Linux Foundation, v1.0 as of April 2026, adopted by 150+ organizations with SDKs in five languages.

## What an Agent Card is

A2A's answer to "how do agents discover and trust each other": a machine-readable declaration of an agent's identity, provider, capabilities, skills, and input/output modes, served at a well-known URL. Think of it as a passport plus a menu. v1.0 added **signed** Agent Cards, so the declaration can be cryptographically tied to its publisher.

## Why we publish one for an educational agent

Three honest reasons:

1. **Interoperability discipline is a control.** Writing the card forces you to state what the agent can do, what it cannot, and what it never holds (credentials, execution paths). That exercise IS domain 1 (Identity) of [LEASH-8](leash-8.md) applied to yourself.
2. **Standards-awareness is a trust signal.** Partners and technically sophisticated readers check whether you speak the ecosystem's language. A card costs an hour; bespoke integration docs cost weeks.
3. **It keeps us honest.** The card declares NO securitySchemes and NO live endpoint, because there is none yet. When one ships, the card gets signed. Declaring less than you could is the cheapest credibility there is.

## What we deliberately did NOT do

- No fabricated live endpoint URL. The `url` points at this repo.
- No signature block pretending to be signed. `signature: null`, with a note.
- No AP2 (Agent Payments Protocol). We do not do agent-mediated payments, so we do not declare payment capability. If your agent does not move money, neither should its card.

## If you copy this pattern

1. Copy [agent-card.json](../agent-card.json), replace name/provider/skills with yours.
2. Declare only what is live. A card that overstates capability is a false claim with your signature on it (regulators increasingly read it that way too).
3. When you host a real A2A endpoint: serve the card at the well-known path, add authentication in `securitySchemes`, and sign it.
4. Re-check the card whenever capability changes. A stale card is a stale promise.
