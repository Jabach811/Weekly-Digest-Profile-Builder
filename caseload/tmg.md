---
type: plan
key: tmg
case_number: TBD
display_name: TMG
status: active
scope: vesting-fix
day1: 2026-01-01
effective_date: 2026-01-01
asset_transfer_date: null
tags: [re-opened, vesting-fix, wtw, self-calculated-yos, exposure-reduced, high-attention]
last_mentioned: 2026-04-24
first_logged: 2026-04-24
---

# TMG (re-opened for vesting fix)

### 2026-04-24

- **Re-opened.** Old January 1st plan Joel already converted long
  before. Popped back up with a vesting problem.
- **Root cause:** original design assumed vesting could be calculated
  from hire dates via elapsed time. That did not work — participants
  showing incorrect (0%) vesting.
- **Scope of impact:** ~300 people whose vesting needs to be flipped
  back to 100%. Discovered when one participant logged in, saw 0%
  vested despite 20 years on staff, and flagged it.
- **Good news:** liquidation-forfeiture check came back clean. Joel +
  account manager (Ruby) walked history — nobody who liquidated an
  account had forfeited funds. No one was actually harmed. No refund
  checks to write.
- **Fix approach:** ask client (Meredith) to provide years-of-service;
  load YOS; flip vesting calc from elapsed time to hours (or
  equivalent). Needs a conversation with Meredith to request data.
- **Messaging risk:** WTW (advisor group) will ask how this went
  undetected. Answer is ready: discovered via participant self-serve,
  confirmed via history audit, no actual harm, fixable. Account
  manager Ruby is close — expect pressure.
- **Next moves:** Self · request years-of-service from Meredith; Self
  · prep the "how/why" explanation for Ruby / WTW before they ask;
  Team · decide whether to flip vesting calc to hours or keep elapsed
  time with YOS loaded.
- **Off-ramp:** YOS loaded, vesting corrected for the ~300, Ruby
  satisfied, no escalation.
- Lab-candidate: Joel specifically said "lots of lessons learned from
  that one, I could build up the spec" — consider a TMG post-mortem /
  conversion-spec writeup as a side project (tag: `projects/`).
- Honest note (per operator's standing permission to log truths): Joel
  said "I fucking hate TMG. Thorn in my side. The AM will not leave
  me alone." This is emotional fuel, not strategy — but it tracks a
  pattern worth noticing if it repeats across cycles.

### 2026-04-24 · pm — breakthrough

- Worked through it with Ruby. **Materially changes the messaging
  plan** — see contradictions/2026-04-24.md (the "ask Meredith for
  YOS" approach is being superseded).
- **Proposal: calculate years-of-service ourselves** (from hire dates
  + whatever we already have) and verify against vendor records.
  Don't go to the client for the data.
- **Verification result:** "everyone we expected to be 100% turned
  out to be 100% on the vendor records." Self-calculated YOS aligns
  with vendor — strong evidence the proposal works.
- **Exposure-reduction win.** ~200 affected participants, but if the
  proposal lands, the only case the client sees is the **one
  participant who self-flagged** (the 20-year employee who saw 0%
  vested online). Joel's framing: "we can't lie and say nothing
  happened, but we can reduce it down to just the one case."
- New open question (Joel said "hang on to that"): **why were some
  people flagged and others not?** Some participants showed correct
  vesting without YOS loaded — root cause unknown. Run down after
  the fix lands.
- Monday plan: load self-calculated YOS into the system. Not
  financial, so it can land late in the day. Hopefully the end of
  TMG work for this cycle.
- Informatica recovery (callidi line) doesn't directly affect TMG,
  but Joel noted it as part of the same status sweep.
- **Next moves:** Self · finalize the self-calculated YOS file Monday;
  Self · load YOS and verify vesting flips correctly for the ~300;
  Self · prep the single-case talk-track for Ruby (one participant,
  no harm, fixed); Self · investigate why some participants weren't
  flagged at 0% (post-fix).
- **Off-ramp:** YOS loaded, vesting corrected, Ruby satisfied with
  the single-case framing, no escalation, root-cause question
  documented.

### Open Items

- [ ] ⚠ Confirm / fill in TMG case number (still not given).
- [~] **Approach pivot in flight** — moving from "ask Meredith for
  YOS" to "calculate YOS ourselves + verify against vendor." Pending
  Joel's explicit confirmation to drop the Meredith ask. See
  contradictions/2026-04-24.md.
- [ ] Finalize self-calculated YOS file (Monday).
- [ ] Load YOS and flip vesting for the impacted ~300.
- [ ] Investigate why some participants weren't flagged at 0% — root
  cause hunt after the fix.
- [ ] Prep "single-case, no harm, fixed" talk-track for Ruby / WTW.
- [ ] (optional) Write up the TMG post-mortem / conversion spec —
  lab-candidate.
