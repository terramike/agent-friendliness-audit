# Agent-Usability Spec — <site / surface>
**For:** <builder> · **From:** <requester / agent> · **Date:** <date>
**Site audited live:** <URL> · **Status:** Proposal — nothing built yet.
**Scope:** <one line: what this spec is about, and what it deliberately is not>

## 1. Verdict
<The audit's blunt summary: the task outcome, the first blocker, and what an agent can
and cannot do today — in one paragraph.>

### What already works (keep, and thank you)
- <strengths, with evidence>

### What blocks an agent today (verified live, <date>)
- <blockers, each with the observed evidence>

## 2. P0 — the smallest fix that passes the task test
Work down the ladder; stop at the first rung that solves the task. Reuse existing
state and interfaces before proposing anything new — a canonical link may solve
arrival; a named HTML control may solve operation.

1. **<fix>** — unblocks task-test step <n>. <Prefer, in order: existing interface →
   documentation of what exists → new surface (named control, test id, JSON island,
   API) only if the earlier rungs fail.>

### Share-safety (required if any state goes into URLs or page state)
Fields **safe to share** in links / state islands (explicit list — if a field is not
listed, it does not go in):
- <field> — <why it's public-safe>

Credentials, private drafts, and personal data never appear in URLs or in page state
exposed for discovery. For private or account workflows, use the product's existing
authenticated state instead of mirroring state into links/islands.

### Acceptance test (define success before running it)
- **Task:** <the user's task>
- **Expected result:** <what success looks like>
- **Receipt (only if the task produces one):** <the id/record this request produces,
  tying output to request. Read-only lookups produce no receipt or artifact — their
  verification is matching the displayed content to the request.>
- **Verify:** <one user-meaningful property of the output to check — a hash identifies
  bytes and a receipt identifies a transaction; neither alone proves the result
  matched the request>
- **Run:** agent arrives by link → confirms state matches → acts → observes done →
  retrieves the result → checks the receipt and the property.
- **Failure case (must be included):** <a failure or cancellation that could expose a
  false pass — e.g. the action is cancelled or times out and a stale/previous artifact
  is returned with a success status; the test must reject it>
- **Authorization note:** <which steps are commits (send/spend/publish/state change)
  and whose approval they require; in tests, stop at review or use a test environment>

## 3. P1 — worth it, not blocking
- <determinism notes, events (`<product>:done/progress/error`), a small JS API
  (`window.<Product>`), text equivalents for pointer-only interactions>

## 4. P2 / explicitly NOT asked
- <e.g. postMessage embeds, web component, headless core — and the line that protects
  the product's design: no hosted API if the product is client-side by design>

## 5. Build order
<Steps in dependency order, each sized S/M/L. The smallest set that passes the
acceptance test ships first.>

## 6. Open questions for the builder
1. <question> — <why it matters>. <Your existing names should win over this spec's
   placeholders; your shipped doc becomes the canonical reference.>
