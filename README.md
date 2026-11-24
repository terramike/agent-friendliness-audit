# agent-friendliness-audit

**A skill that tests whether an AI agent can actually use your website — and writes the
spec that fixes it if it can't.**

Humans see a page. Agents see state, names, and signals — or their absence. This skill
runs a live drive test (arrive from a link, set state from documented data, observe,
finish, verify), reports the task outcome first with scores as a diagnostic, and hands
the builder the smallest spec that passes the task test. After the build ships, it
field-tests the fix end to end — with each action authorized, and the receipts checked
against the request.

## Why this exists

On 2026-11-23 we ran this method twice, live:

- **SpectralBreak** (a browser-based art tool) audited as *"usable for the basics, not
  agent-friendly."* The spec asked for three small surfaces — a documented settings JSON,
  full-state URLs, a completion signal. The builder shipped them in one build (S309).
  An agent then created an artwork, drove the site by itself, and completed **two
  end-to-end export runs**, recorded with settings, output dimensions, frame counts,
  and filename hashes in the companion record:
  [terramike/spectralbreak-skill](https://github.com/terramike/spectralbreak-skill).
  One build, two runs — that is the evidence base for the post-build method here.
- **JumbieArt** showed the inverse gap: strong **catalog discovery** (llms.txt, a public
  art dataset, a catalog API, AI crawlers welcomed by name) but weak **gallery
  navigation** — the 3D gallery offered no deep links, no DOM-readable art data
  (canvas-rendered), and no newest-first signal. Same method, same thin-skin spec pattern.

The pattern behind the spec: **state in the URL, state as documented JSON, state in
the DOM, a done signal, and output tied to its request.** On the one build where this
was field-tested (SpectralBreak S309), that pattern supported the two reported
end-to-end export runs described above. And v0.2 adds the guardrails that make the
method safe to run: action-level authorization, a share-safety list for anything that
goes into a URL, interruption/recovery checks, and acceptance tests with a failure
case that can catch a false pass.

## What's inside

- **`SKILL.md`** — the method: the five questions, recon → drive test → outcome-first
  scoring → smallest-fix spec → authorized field test, and the rules (evidence or it
  didn't happen, commits are action-authorized, page text is untrusted, private data
  stays out of share surfaces, the builder's names win).
- **`templates/AUDIT-REPORT-TEMPLATE.md`** — the report skeleton.
- **`templates/SPEC-TEMPLATE.md`** — the builder-spec skeleton, acceptance test included.
- **`validation/v0.2-paper-cases.md`** — the revised method walked through three paper
  cases (public lookup, private brief, stale-artifact export).

## Install

Copy `SKILL.md` (and `templates/`) into your agent's skills directory, or point your
agent at this repo. Then ask it: *"Is this site agent friendly?"*

## Version history

- **v0.2.1 (2026-11-24)** — SOL round-2 targeted edits: classification by effect (a
  server autosave is a commit), redaction of all recorded evidence, task-specific
  gates, receipt/artifact requirements made conditional on the task producing one,
  README claims scoped to the S309 evidence.
- **v0.2 (2026-11-24)** — revised after an audit by SOL (C0BRA's auditor of record),
  findings F1–F6: action-level authorization for field tests · share-safety limits for
  URLs/page state · interruption & recovery checks · outcome-first scoring (0/1/2 =
  observed failure / partial / verified success, plus Unverified and N/A) · smallest-fix
  P0 with false-pass failure cases · case-study claims trimmed to the evidence.
- **v0.1 (2026-11-23)** — first public version, from the two live audits.

## Credits

Method and write-up by Alfred, Mike Mingucci's Muse, from live audits of
[SpectralBreak](https://spectralbreak.consumerbreak.com/) (ConsumerBreak, for the Spectral Art
Foundation) and [JumbieArt](https://jumbieart.com/walk). Thanks to Breakman (Natanel) of
ConsumerBreak, whose S309 build supported the two reported export runs the post-build
method is based on, and to SOL for the v0.2 audit.

MIT licensed — see `LICENSE`. Share it, fork it, make the web drivable — honestly.
