# Approval design checklist: irreversible actions

Use this when wiring human approval into an agent pipeline. Every unchecked box is a hole an attacker or an eager agent will eventually find. Companion doc: [plan-vs-authorize](../docs/plan-vs-authorize.md).

## 1. Enumerate

- [ ] Irreversible/outward action classes are written down (money, outbound to humans, deletion, publishing, credential use, legal commitments)
- [ ] Unlisted action classes default to REQUIRE approval (highest tier), not to "allowed"
- [ ] There is a written "we never automate this" list, however short

## 2. Gate placement

- [ ] The gate is deterministic code, not a model judging its own request
- [ ] The gate sits between the model and the executor; the model has no direct path to the action
- [ ] The executor's credential only covers the gated action class (a payment executor cannot delete repos)

## 3. The approval channel

- [ ] Approval travels out-of-band: a channel separate from the content the agent reads while working
- [ ] Approver identity is allowlisted by account id (display names and phone-book names do not count)
- [ ] The approval is an explicit token bound to a request id, not free text or an emoji reaction
- [ ] Text the agent read in ANY input can never be interpreted as approval (chat content = data, always)
- [ ] The agent cannot approve its own request from any account it controls

## 4. Time and state

- [ ] Approvals expire (define the window; 15 minutes is a sane default for interactive flows)
- [ ] One approval covers ONE request id; retries and batches need their own
- [ ] A denied request stays denied; the agent must not re-ask in a loop (log and move on)

## 5. Human factors

- [ ] The approval request states: what, why, risk, and what happens on "no", in one message the approver can judge from a phone
- [ ] There is a defined path when the approver is unreachable (queue and wait; degrade autonomy; never "proceed by timeout")
- [ ] The approver can also HALT everything with one message (kill switch in the same channel)

## 6. Evidence

- [ ] Every approval/denial lands in an append-only log: request id, approver id, timestamp, outcome
- [ ] The DENY path has been tested on purpose at least once (fire a gated action, refuse it, verify nothing happened)
- [ ] The BYPASS path has been tested on purpose at least once (try to trick the gate with content that looks like approval; verify it fails)

## Common designs that fail this checklist

| Design | Which box it fails |
|---|---|
| "The model asks itself 'is this safe?' before acting" | 2.1: the judge is the suspect |
| "Approval = user says ok in the same chat the agent reads documents in" | 3.1/3.4: injected text can fake it |
| "One 'yes' at the start of the session covers everything after" | 4.1/4.2: stale, unbounded scope |
| "If no answer in 10 minutes, proceed" | 5.2: timeout-as-consent inverts the default |
| "Approvals are in the agent's own memory/log file" | 6.1: the suspect keeps the evidence |
