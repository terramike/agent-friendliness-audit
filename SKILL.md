---
name: agent-friendliness-audit
description: Audit a website, tool, or app for agent-friendliness and write the spec
  that makes it agent-usable. Use when the user asks "is this site agent friendly",
  "can an agent use this", "audit this for agents", "make this agent usable", or
  wants an agent-access spec for a builder. Runs a live drive test, scores the
  machine-readable surface, and produces a P0/P1 spec with an acceptance test —
  then, after a build ships, field-tests it end to end and verifies the receipts.
license: MIT
---

# Agent-Friendliness Audit — test a site the way an agent experiences it

Humans see a page. Agents see **state, names, and signals** — or their absence. A site is
agent-friendly when an agent can **arrive at a state from a link, set that state from
documented data, observe what is happening, know when it is done, and verify the result**
— without screen-scraping guesswork, a human hand, or an account.

This skill codifies a method proven in the field (see Case studies). It produces three
things: a **verdict with evidence**, a **builder-ready spec** (small surfaces, acceptance
test), and — after the builder ships — a **field test with receipts**.

## The five questions (the whole method)

For the site's core task, ask:

1. **Can I arrive?** Does a URL encode the full state (deep link: item, settings, view)?
   Does the URL change as state changes? Can a state be *shared*?
2. **Can I set it?** Is there a documented data format (settings JSON, query params, a
   manifest) with field names, ranges, and defaults written down somewhere an agent can
   read (`/llms.txt`, docs, a schema)?
3. **Can I see it?** Do controls have accessible names and stable selectors
   (`data-testid`)? Is current state readable as data (a JSON state island, a status
   line with `role="status"`)? Or does the content live only in canvas/WebGL pixels?
4. **Can I finish?** Is there a completion signal (status text, an event, a callback)
   and progress while waiting? Can the result be downloaded/retrieved by an agent?
5. **Can I verify?** Is the output self-describing (filename carries an id/hash, embedded
   metadata, deterministic output for identical inputs)? Do errors arrive as one clear
   line with a code — or silently (a fallback image, a swallowed failure)?

Plus the gates: **no login wall, CAPTCHA, or bot block on the core path**, and the
**machine surface**: `/llms.txt`, `/robots.txt` (does it welcome or block AI crawlers?),
`/sitemap.xml`, structured data (JSON-LD), and any public JSON/API the page itself loads.

## Procedure

### Phase 1 — Recon (read-only)
- Fetch `/llms.txt`, `/robots.txt`, `/sitemap.xml`, and the page. Note whether discovery
  files are real or return the SPA shell (a common fake-out: every path returns index.html).
- Inventory the machine surface: JSON endpoints, manifests, catalogs, JSON-LD. Record
  exact URLs and shapes (first fields only).
- Note walls: login, CAPTCHA, paywall — and which features sit behind them.

### Phase 2 — Live drive test
- Load the exact URL the user gave. Perform the core task the way an agent must:
  navigate by names/selectors, not by pixel-hunting where avoidable.
- **Change nothing you weren't asked to change.** No settings altered, no exports run,
  no messages sent, no purchases — unless the user explicitly makes it a field test
  (Phase 5). An audit is read-only; say what you did *not* do.
- Try to construct/share a state link. Try the documented data format if one exists.
- Record verbatim: status texts, error lines, URLs before/after. Evidence beats adjectives.

### Phase 3 — Score and verdict
Score each area 0–2 (absent / partial / solid) and give one blunt overall verdict per
*surface* — a site can be 9/10 on data and 2/10 in its flagship experience; report both.

| Area | What earns a 2 |
|---|---|
| Arrival | Full-state deep links; URL tracks state; share copies state |
| Setting | Documented data format with names/ranges/defaults |
| Seeing | Named controls + testids + readable state (island/status) |
| Finishing | Progress + explicit done signal + agent-retrievable result |
| Verifying | Self-describing output (hash/id in name or metadata); deterministic |
| Errors | One-line coded errors; no silent fallbacks |
| Machine surface | Real llms.txt/robots/sitemap; public data endpoints; AI welcomed |
| Gates | Core task needs no account, no CAPTCHA, no human-only input |

### Phase 4 — The spec (what to hand the builder)
Write it so a good builder can ship in a day. Structure (template in `templates/`):
- **Verdict + what already works** (name it, thank them — builders ship more for people
  who noticed what they got right).
- **Blockers**, each tied to evidence from Phase 2 ("the URL never changed through …").
- **P0 — the thin skin.** The recurring minimal set: state in the URL · state as
  documented JSON · state in the DOM (island + status line) · named controls/testids ·
  self-describing output · coded errors · real discovery files. Size each item (S/M/L).
- **P1** (events/API niceties, determinism extras) and an **explicitly-not-asked** list —
  especially: no server API if the product's privacy/design is client-side; no rebuilds.
- **An acceptance test** an agent can run: arrive by link → verify state matches → act →
  observe done → verify the artifact. If the acceptance test can't be written, the spec
  isn't concrete yet.
- **Open questions** for the builder, numbered, each with why it matters. Expect the
  builder's existing names to win over your placeholders — say so in the spec; the
  canonical reference becomes *their* doc, not your spec.

### Phase 5 — Field test (after the build ships)
- Re-run the acceptance test end to end, as an agent, signed out, using only the shipped
  surfaces. Produce a real artifact.
- **Verify the artifact yourself** (probe the file: format, dimensions, duration, frames;
  sample frames/bytes at two points and confirm they differ as expected). A "done" claim
  from the page is a claim; the file is the evidence.
- Write the **friction log**: everything awkward, ambiguous, or unobservable — verbatim
  error/status lines, with the smallest fix that would close each gap.
- Report back to the builder: verdict, receipts (ids, hashes, sizes), friction log,
  thanks. Drafts of anything sent in the user's name wait for the user's approval.

## Rules
- **Evidence or it didn't happen.** Quote status lines and URLs verbatim; never invent
  prices, inventory, specs, titles, or capabilities you didn't observe.
- **Read-only by default.** Audits change nothing. Field tests change only what the user
  approved testing.
- **Honest limits.** If your tooling can't do something (e.g. record video, evaluate JS),
  say so plainly and state what you verified instead. Never dress a partial check as a pass.
- **Score the surface, not the team.** Blunt about interfaces, generous about people;
  name what already works before what doesn't.
- **The builder's names win.** Your spec's field names are placeholders until the builder
  answers; their shipped doc is canonical.
- **Privacy is a feature.** If a product is client-side/on-device by design, the spec must
  preserve that — agent access via links and local state, not a hosted API.

## Case studies (the method, proven)
- **SpectralBreak (2026-11-23):** audited "usable for basics, not agent-friendly" →
  spec v0.2 (P0 = settings JSON, full-state URLs, names/status/island) → builder shipped
  all of it the same day (build S309) → field test passed end to end, twice, with
  hash-verified outputs. Companion skill: https://github.com/terramike/spectralbreak-skill
- **JumbieArt /walk (2026-11-23):** the inverse case — a site with an excellent machine
  surface (llms.txt, public art dataset, catalog API) whose 3D gallery itself scored
  ~2/10 (no deep links, canvas-only art, no newest signal). Spec applied the same thin
  skin: `?room=&art=&view=` links, a gallery manifest, a `#walk-state` island.

## Deliverables checklist
- [ ] Audit report (verdict, evidence, scores) — `templates/AUDIT-REPORT-TEMPLATE.md`
- [ ] Builder spec (P0/P1, acceptance test, open questions) — `templates/SPEC-TEMPLATE.md`
- [ ] After the build: field-test report with receipts + friction log

---

*Written by Alfred (Mike Mingucci's Muse) from two live audits run on 2026-11-23.
Shared freely: the more of the web agents can drive honestly, the better for everyone
building on it. MIT licensed.*
