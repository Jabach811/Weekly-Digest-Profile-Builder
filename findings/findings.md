---
type: findings
key: findings
tags: [reference, gotchas, heuristics]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Findings

One-off things worth remembering. Gotchas, reusable heuristics, vendor
quirks, decisions made, random facts the next cycle might need.

Entries are dated and appended. Never rewritten. If a finding becomes
obsolete, leave it in place and add a newer entry noting the correction.

Tag each entry in the body with bracketed chips (e.g. `[ADP]`,
`[PATTERN]`, `[VENDOR]`, `[DATA]`) so a future scan can filter fast.

---

### 2026-04-22

- `[ADP]` `[DATA]` ADP's census records are reliable. ADP's **deferral**
  records are not — you cannot tell if a participant is genuinely still
  in default within the (min, max−1) window. Known workaround: load
  everyone out of default at their current %; hand the client the
  ambiguous group so they can notify directly.

- `[PATTERN]` `[TA-BRAIN]` The backslash (`/`) command palette pattern
  from NBI Tracker is a strong fit anywhere with many tags or filters.
  Reuse candidate across side-apps.

- `[DECISION]` `[DATA-PARSER]` Mapping Validator does not get extracted
  as a standalone. It lives inside Data Parser. The 2-for-1 was the
  whole point of folding it in.

- `[CLIENT]` `[CALLIDI]` Client balance files can arrive incomplete
  (participants with balances missing from the roster). Always
  cross-check SSNs from the balance file against the roster population
  before accepting the transfer set as complete.

- `[PATTERN]` `[SECURITY]` The sanitizer agent (strips socials, PII,
  identifiers; generalizes at high granularity) is the unlock for any
  conversation about expanding access to sensitive data — e.g., Anthony
  → TC section. Always lead with the safety demo, not the feature
  pitch.
