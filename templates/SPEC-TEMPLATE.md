# Agent-Usability Spec — <site / surface>
**For:** <builder> · **From:** <requester / agent> · **Date:** <date>
**Site audited live:** <URL> · **Status:** Proposal — nothing built yet.
**Scope:** <one line: what this spec is about, and what it deliberately is not>

## 1. Verdict
<The audit's blunt summary. What an agent can and cannot do today, in one paragraph.>

### What already works (keep, and thank you)
- <strengths, with evidence>

### What blocks an agent today (verified live, <date>)
- <blockers, each with the observed evidence>

## 2. P0 — the thin skin
1. **Documented data format.** <settings/state as versioned JSON, written down in
   /llms.txt or docs; emitted complete by the product's own "copy" affordance>
2. **Full-state URLs.** <?param=&s=<encoded state>>; Share copies the full state, not
   the bare page URL.
3. **Names + state in the DOM.** Accessible names on every control, stable
   `data-testid`s, a `role="status"` line, and a JSON state island
   (`<script type="application/json" id="<product>-state">`) rewritten on change.
4. **Self-describing output + coded errors.** <filename/metadata carries an id or hash;
   errors as one line `error: <code> - <why>`; no silent fallbacks>

### P0 acceptance test
An agent can: open a full-state URL → read the state island and confirm every field
matches → trigger the core action → observe progress → receive `done: <artifact id>` →
retrieve the artifact and verify it. <Adapt to the product; if this can't be written
concretely, the spec isn't ready.>

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
