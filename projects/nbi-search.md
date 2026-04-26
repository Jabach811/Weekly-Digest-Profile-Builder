---
type: project
key: nbi-search
status: ready-to-ship
tags: [ui, search, side-app]
flags: [ship-blocker:file-picker, push-stuck, ambiguous-with-haverhill-nbi]
people: []
depends_on: []
last_mentioned: 2026-04-24
first_logged: 2026-04-22
---

# NBI Search

Fast search UI over NBI data.

## Invariants

- Design is locked and loved. Function-only changes from here.
- File picker has to die — replace with a hard-coded or config-loaded
  path.

## Off-ramp

Path-free launch experience → shippable.

## Next move

Confirm whether the 4/24 "trouble pushing my update out" refers to
this app or the Haverhill NBI status email
(see [contradictions/2026-04-24.md](contradictions/2026-04-24.md)).

## Log

### 2026-04-22

- Design complete. Do not iterate on it.
- One ship-blocker: the file picker. Replace with a hard-coded path
  (or config-loaded path) and a refresh button.

### 2026-04-24

- ⚠ Logged tentatively against this project — Joel said "having
  troubles with NBI at the moment — not sure what's going on.
  Probably won't be able to push my update out, but good thing is
  it's in there. It matches, so we can continue forward." This
  may instead be the Haverhill NBI status email (caseload). See
  contradictions/2026-04-24.md.
- If this IS the app: a push / deploy is currently stuck. Update
  exists locally and "matches" (works) but isn't getting out. Likely
  blocked on the file-path picker still being the ship-blocker, or a
  separate deploy issue. Diagnose Monday.

## Open Items

- [ ] ⚠ Confirm whether the 4/24 "trouble pushing my update out"
      refers to this app or the Haverhill NBI status email.
- [ ] If app: diagnose what's blocking the push Monday; resolve.
- [ ] Replace file-picker with hard-coded / config-loaded path.
- [ ] Wire refresh button.
- [ ] Decide whether path is literal or config-driven.
