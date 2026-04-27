---
type: plan
key: callidi
case_number: JK62945-00001
status: ramping
scope: full
day1: 2026-01-01
effective_date: 2026-04-01
asset_transfer_date: 2026-04-10
tags: [loan-issue, asset-transfer, paychex, post-balance-load]
last_mentioned: 2026-04-26
first_logged: 2026-04-22
---

# Callidi Tas

### 2026-04-22

- Blocked on client: missing DOB + DOH for the participants who were in
  the asset transfer but not in our system. Cross-checked SSNs myself —
  the gap is real, not a mistake on our side. Worth flagging: the client's
  balance file was incomplete.
- One loan is out of order — payment either logged but not made, or made
  but not logged. Loans on this plan have been frozen/unfrozen previously;
  may need a re-amortize. Only one participant, one loan — contained.
- **Next moves:** nudge client for DOB/DOH; figure out the loan
  discrepancy.
- **Off-ramp:** once participants + balances + loan load — this plan is
  off the plate. Target: end of week.
- Transmission-worthy: client data-completeness issue (balance file
  missed participants) should be surfaced.

### 2026-04-24

- Blocker shifted: **Informatica is down** (company-wide / network).
  Can't post participant balances or loans until it's back. Not on us.
- Balances and loans are **prepped and staged** — ready to import. This
  implies the DOB/DOH gap is effectively resolved (can't stage balances
  without participants loaded). ⚠ CONTRADICTS FILE: previous open item
  said "nudge client for DOB/DOH" — appears closed but operator didn't
  confirm. See contradictions/2026-04-24.md.
- **Planned sequence once Informatica opens:** post participant balances
  → process loans → process elections → done.
- Expect to finish prep today so the trigger can be pulled the moment
  Informatica is back.
- **Next moves:** Self · finish prep today; watch for Informatica
  recovery; fire the sequence as soon as it clears.
- **Off-ramp:** all three steps (balances, loans, elections) land.
- Transmission-worthy: carrying a fully-staged plan pending an external
  system outage — the work is done, the trigger is external.

### 2026-04-24 · pm

- status: waiting → ramping. **Informatica resolved 4/26 ~10:57 AM** —
  Stacy confirmed the company-wide network issue was fixed; Joel's test
  workflow ran clean (only the expected "SSN < 9 chars" warning).
  Closes the [~] external block.
- **Participant balances are loaded.** Walked Beth through the full
  sequence on a screen-share: parameter file flipped Y→N (production),
  Informatica run succeeded (~20 sec), Bill Remit Detail doubled as
  expected, totals verified. Reversed the 999-00-0000 dummy account
  (Takeover Account, "BRD No Dummy" snapshot saved) via P3 → ROC
  Reversal Query in CORP — 29 ref numbers, all-or-nothing run, clean.
  $11M total back where it should be.
- **New blocker on loans:** ref numbers **XKB and XKC are not landing
  in Transact Detail** (they ARE in Bill Remit Detail). Suspected
  cause: fund-letter-prefix collision after the alphabet rolls past Z
  into AA/BA/CA — the query may be missing them. Joel running it down
  in AQT. Need Earl pulled in if it doesn't resolve quickly. ⚠ Note:
  this is a different loan issue than the 4/22 "frozen/unfrozen
  re-amortize" item — that one may still be open, may have been
  superseded; see contradictions/2026-04-24.md.
- **Loan setup completed:** added Paychex as record keeper in P3 →
  Convert; created the Conversion record (effective 4/1; conversion
  + assigned dates = 3 months prior, per the standing convention).
  Conversion number is attached to the loan output.
- **Manual loan-number → loan-source matching done by hand** for the
  participant with five loans — totals add up, ready for import. Not
  financial, so safe to land late in the day.
- COCC source-routing problem is unrelated but live (see cocc.md).
- **Next moves:** Self · diagnose XKB/XKC missing-from-Transact-Detail
  with Earl on Monday; Self · re-run loans once cleared; Beth ·
  process elections Monday; then close out.
- **Off-ramp:** loans land + elections processed → off the plate.
- Transmission-worthy: today's screen-share with Beth produced a clean
  run of the takeover-balance-load procedure, including the 999-00-0000
  dummy-account reversal flow and the Paychex loan-setup steps. Full
  set of screenshots captured — feeds the slide-creator pipeline (see
  projects/slide-creator.md).
- Lab-candidate: macro/script for the **manual loan-number combinatorial
  match** ("find which subset of source amounts sums to which header
  total"). Joel: "I bet there's a much better way of doing this. I'm
  going to write something to fix this."
- Lab-candidate: **automate the post-balance-load reversal + recon flow**
  (BRD copy → ROC reversal → BRD No Dummy → Transact Detail check).
  Joel: "I'm going to write a macro to do all this — don't like all this
  manual work."
- UI-observation (not a plan item): the P3 ROC Validate button gives no
  feedback beyond enabling Analyze. Joel flagged it as fixable. Logged
  as a small lab-candidate in profile/identity.md.

### 2026-04-26

- **Loan diagnosis advanced.** XKB/XKC still not landing in Transact
  Detail. Joel's working theory: **case number issue** causing a silent
  error — Informatica returns "success" but the records don't hit the
  tables. This is the same silent-error pattern that usually signals a
  bad case number in the parameter file or import file. Lean toward the
  import file as culprit. Need to verify and fix.
- **NBI status email — Haverhill (census + elections):** NBI system was
  down end of last week (company-wide; new field broke the service). Beth
  tried to send, couldn't. Joel taking ownership. Wants to double-check
  all participants uploaded and elections defaulted before sending.
- **NBI status email — Callidi (participant balances):** need to send this
  once NBI is back up so the plan can proceed.
- Update loan import procedures / best-practices doc once the case number
  issue is confirmed — flag the silent-error pattern explicitly.
- **Next moves:** Self · diagnose XKB/XKC case-number theory in import
  file; Self · re-run loans once fixed; Self · send NBI status emails
  (Haverhill first, then Callidi) after double-check.
- **Off-ramp:** loans land + elections processed → off the plate.

### Open Items

- [x] (2026-04-24) **Informatica down** — resolved company-wide ~10:57.
- [x] (2026-04-24) Finish prep so nothing blocks the post-Informatica
  sequence.
- [x] (2026-04-24) Post participant balances.
- [x] (2026-04-24) Reverse the 999-00-0000 dummy account.
- [ ] Diagnose XKB/XKC not landing in Transact Detail — working theory
  is case number issue in import file (silent "success" error pattern).
  Pull Earl in if not resolved quickly.
- [ ] Re-run / process loans once XKB/XKC is resolved.
- [ ] Beth · process elections once loans land.
- [ ] Send NBI status email for Callidi (participant balances) once NBI
  system is back up.
- [ ] Update loan import procedures / best-practices: flag the
  silent-error / case-number pattern explicitly.
- [ ] ⚠ Confirm whether the older 4/22 "loan order — frozen/unfrozen,
  re-amortize" issue is the same as the XKB/XKC issue or still
  separate. See contradictions/2026-04-24.md.
- [ ] ⚠ Confirm whether DOB/DOH from client is fully resolved (still
  never explicitly stated; balances staged anyway).
- [ ] Surface balance-file completeness issue up the chain
  (transmission-worthy).
