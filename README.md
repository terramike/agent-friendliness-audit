# agent-friendliness-audit

**A skill that tests whether an AI agent can actually use your website — and writes the
spec that fixes it if it can't.**

Humans see a page. Agents see state, names, and signals — or their absence. This skill
runs a live drive test (arrive from a link, set state from documented data, observe,
finish, verify), scores the machine-readable surface, and hands the builder a small,
shippable spec with an acceptance test. After the build ships, it field-tests the fix
end to end and verifies the receipts.

## Why this exists

On 2026-11-23 we ran this method twice, live:

- **SpectralBreak** (a browser-based art tool) audited as *"usable for the basics, not
  agent-friendly."* The spec asked for three small surfaces — a documented settings JSON,
  full-state URLs, a completion signal. The builder shipped all of it **the same day**,
  plus more. An agent then created an artwork, drove the site by itself, and produced
  verified videos — twice. The companion skill lives at
  [terramike/spectralbreak-skill](https://github.com/terramike/spectralbreak-skill).
- **JumbieArt's 3D gallery** turned out to be the inverse: a site with an excellent
  machine surface (llms.txt, public datasets, a catalog API, AI crawlers welcomed by
  name) whose flagship 3D experience scored ~2/10 for agents — no deep links, art locked
  in a WebGL canvas, no way to say "take me to the newest work." Same method, same
  thin-skin spec pattern.

The pattern that keeps working: **state in the URL, state as documented JSON, state in
the DOM, a done signal, and self-describing output.** Five small surfaces flip a site
from "scrapable" to "drivable" — usually in a day of builder time.

## What's inside

- **`SKILL.md`** — the method: the five questions, recon → drive test → scoring →
  spec → field test, the scoring rubric, and the rules (evidence or it didn't happen,
  read-only by default, the builder's names win, privacy is a feature).
- **`templates/AUDIT-REPORT-TEMPLATE.md`** — the report skeleton.
- **`templates/SPEC-TEMPLATE.md`** — the builder-spec skeleton, acceptance test included.

## Install

Copy `SKILL.md` (and `templates/`) into your agent's skills directory, or point your
agent at this repo. Then ask it: *"Is this site agent friendly?"*

## Credits

Method and write-up by Alfred, Mike Mingucci's Muse, from live audits of
[SpectralBreak](https://spectralbreak.consumerbreak.com/) (ConsumerBreak, for the Spectral Art
Foundation) and [JumbieArt](https://jumbieart.com/walk). Thanks to Breakman (Natanel) of
ConsumerBreak, whose same-day S309 build proved the spec pattern end to end.

MIT licensed — see `LICENSE`. Share it, fork it, make the web drivable.
