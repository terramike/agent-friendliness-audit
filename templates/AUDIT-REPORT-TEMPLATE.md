# Agent-Friendliness Audit — <site / surface>
**Date:** <date> · **Auditor:** <agent> · **URL(s):** <exact URLs tested>
**Method note:** <what was done, and explicitly what was NOT done — e.g. "read/draft
steps only; no commit action (send/spend/publish/state change) was taken">
**Redaction:** <all recorded evidence in this report — URLs, status text, excerpts —
has been redacted for sensitive values (tokens, personal data, private content)>

## Task outcome (lead with this)
**Task attempted:** "<the user's task, in plain words>"
**Outcome:** <completed and verified / stopped at a required approval / failed>
**First blocker or approval stop:** <the first thing that stopped progress, or "none">
**Client(s) tested:** <browser/agent client per score row>

## What already works
- <named strength, with evidence>

## Blockers (each with evidence)
1. <blocker> — <verbatim status line / URL behaviour / observation>

## Interruptions and recovery
| Stop point | What the agent observed | Who takes over | How the agent resumes |
|---|---|---|---|
| <sign-in / approval / expiry / rate limit / write timeout> | | | |

## The five questions
| Question | Answer | Evidence |
|---|---|---|
| Arrive? (state in URL) | | |
| Set? (documented data) | | |
| See? (names / state readable) | | |
| Finish? (done signal) | | |
| Verify? (receipt tied to request + a property that matters) | | |

## Machine surface (discovery evidence only — proves discoverability, not access or completion)
- /llms.txt: <real / SPA shell / absent> — <notes>
- /robots.txt: <AI crawlers welcomed / blocked / absent>
- /sitemap.xml, JSON-LD, public JSON/API: <inventory>

## Gates (judged per task — an expected, signalled, resumable gate is not a blocker)
| Gate | Expected for this task? | Where the person takes over | How the agent resumes |
|---|---|---|---|
| <login / CAPTCHA / approval / paywall> | <yes / no> | | |

## Scores (diagnostic — per task, per client)
2 = verified success · 1 = partial · 0 = observed failure · Unverified · N/A

| Task | Client | Arrival | Setting | Seeing | Finishing | Verifying | Errors | Recovery | Surface | Gates |
|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | | |

## Top tasks an agent cannot do for a user today
1. "<user request in plain words>" — <why not>
