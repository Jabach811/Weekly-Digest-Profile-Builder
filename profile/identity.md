---
type: profile
key: joel
tags: [identity, working-style, goals]
last_mentioned: 2026-04-24
first_logged: 2026-04-22
updated: 2026-04-24
---

# Joel

## Core

- Senior Data Consultant. Runs plan conversions: 401(k) asset transfers,
  census loads, deferral reconciliation.
- Works off voice braindumps — usually phone, on the drive home or
  between calls.
- Tools: Excel, internal RK system, ADP, HTML/JS for side-apps.

## Working Style

- Wandering brain — needs persistent state outside his head. The
  Weekly Brief is the cartridge he picks up each cycle.
- Function over style once a thing works. "Make it tight, stop
  decorating."
- Kills ideas cleanly when they're not worth it. Example: declined to
  break Mapping Validator out as a standalone — folded into Data Parser.
- Wants the main moves tracked, not granular follow-ups. No "did you
  reply to that email" noise. Use the checklist generator outputs as
  the grain.

## Goals (rolling)

- Ship side-apps that land hard at next review with boss.
- Eventually: a dashboard site hosting all side-apps in one place.
- Convert old Excel one-offs into apps over time.
- Get Anthony's buy-in on the TA Brain TC section — requires a
  data-safety demo (sanitizer agent is the lever).

## How to Talk to Me

- Plain English. No corporate filler.
- Imperative next moves, never "consider possibly."
- One slide per plan, one card per project. No walls of text.
- If something is missing from a braindump, surface it as an FYI on the
  `resurface.` slide — don't guess.

## Constraints / Recurring Facts

- OOO windows recur per cycle. Whenever one lands in the window, the
  Coverage Beacon and Hand-off strips apply.
- Boss cares about visible output on side-projects, not just caseload
  throughput.

## People & Resources

- **Beth** — execution partner. Does the actual census loads, election
  defaults, and data entry. Joel delegates cleanly to her ("I'll have
  Beth do that"). Trusted hands-on.
- **Damien** — client-side contact (Haverhill) for follow-up on
  participants who didn't land in the loaded set.
- **Julius** — **SQL master / go-to guy for all things SQL.** Wrote a
  consolidated query that combines every relevant table into one row
  per participant. Joel's words: "super awesome… this is the coolest
  thing he did." Makes auditing xlswise plans drastically easier. Use
  him when a data audit would otherwise be Excel hell.
- **Ruby** — account manager on TMG (WTW advisor group). Nice person,
  but very persistent; will keep pressure on Joel until vesting fix
  lands.
- **Meredith** — client-side contact at TMG. Old relationship. She's
  who Joel will ask for years-of-service data.
- **Sally** (tentative) — possibly the new Pella-side contact who can
  push ADP for the deferral report. Name not yet confirmed.
- **Earl** — escalation contact for tricky data/loan issues on
  Callidi-shaped plans. Joel: "need to put the bat signal out to Earl
  — he's going to need to get hooked in." Use when something refuses
  to land between staging tables (e.g. Bill Remit Detail → Transact
  Detail).
- **Stacy** — internal infra contact who confirms when company-wide
  issues (Informatica, network) are resolved. Worked with on past loan
  setups too.
- **Dave Slope** — Informatica resource. Joel is "going to keep tapping"
  for deeper Informatica knowledge.
- **Selina** — newer team member, "seems most open" of the demo
  attendees. Promising channel for feedback and adoption on the brain
  / slide-creator tooling.
- **Smith** — also receptive after the demo. Same lane as Selina.
- **Matt** — has an "altershow" loan-handling routine that doesn't
  quite cover the loan-number → loan-source matching step. Mentioned
  as a current gap, not a person to escalate to.

## Client Entities & Advisor Groups

Reference table — "who we're dealing with," rules of engagement.

- **xlswise** — investment group. A big batch of plans with similar
  rules; Julius's consolidated query was built for this shape. When a
  plan is tagged xlswise, expect the audit pattern to apply.
- **WTW** — advisor group. Sticklers, big-money clients, very
  hands-on. Meetings are unique, thorough, highly interactive.
  Expectations are high and scrutiny is high — communication posture
  on WTW plans should assume someone senior will read and ask hard
  questions. TMG sits inside WTW.
- **ADP** — payroll source for Pella (and historically others). Clean
  on census, historically broken on deferrals inside the min/max
  window — the "load-everyone-out-of-default, hand back the ambiguous
  group" workaround applies.

## Operational Takeaways

Reusable playbook beats earned from real cycles. Kept here instead of
on any single plan so it carries across conversions.

- **Post-limited-access activity monitoring.** Once the early-access
  window closes on a cash-conversion plan, run audit queries to verify
  participants only changed what they were allowed to change
  (typically investment elections; sometimes deferrals; sometimes
  beneficiaries). Flags and timestamps make everything auditable
  after the fact.
- **The call-center bypass.** The call center can't see the early-
  access restrictions each participant is under. When a participant
  calls asking to change something they aren't supposed to be able to
  change, the rep verifies identity and makes the change anyway. Not
  malicious, not a hack, not common — but it happens, and it's always
  worth checking for during the load review. Usual tell: unexpected
  existing data on people who should be brand-new in the system.
- **"Catch it during the load review."** When loading brand-new
  populations, any pre-existing activity is a signal — either an
  early-access edit that crossed the line, or a call-center bypass,
  or stale data that shouldn't be there. Investigate before assuming
  it's fine.
- **The 999-00-0000 dummy account ("Takeover Account").** On any
  mapping / asset-transfer plan, add the takeover guy right away.
  The workflow is hard-coded to write to that exact SSN — you can't
  use a different dummy. Standard name: "Takeover Account." Standard
  DOB: 12/25/1955. Standard DOH: 12/25/1985. Same convention applies
  for forfeiture accounts. The visibly fake numbers double as a
  flag-on-sight that the row isn't a real person.
- **The post-balance-load reversal flow** (P3 → ROC → ROC Reversal
  Query in CORP). Bill Remit Detail = staging (never goes away);
  Transact Detail = proof it traded. Reversal clears both. All-or-
  nothing — verify the row count matches your intent before clicking
  Run. Save a "BRD No Dummy" snapshot after the dummy guy's removed.
- **Reduce client exposure before opening the conversation.** When a
  fix can be done internally (e.g. self-calculated values, vendor-
  verified), narrow what the client has to acknowledge. TMG cycle
  pattern: ~200 affected → reduced to one self-flagged case before
  the conversation. The principle: financials move with a stroke of
  a pen, so don't accept a wider issue narrative than the data
  forces.
- **Loan setup convention (P3 → Convert / Conversions).** Add the
  record keeper (e.g. Paychex) before posting loans. New Conversion
  needs three dates — effective date is real; conversion-date-
  received and assigned-date are largely cosmetic but required for
  routines (use effective-date minus 3 months). Conversion number
  must exist before Informatica's loan module can grab anything.
- **Loan Header vs. Loan Source.** Header = individual / per-loan
  detail. Source = source breakout. They must reconcile — system
  rejects mismatches. Better naming Joel uses verbally: "loan
  details" (header) and "by source" (source).
- **Vendor code translation in the parameter file.** Raw vendor
  values get translated to ours via the parameter-file lookup table
  (e.g. "Paychex / loans / 5 = half-month = biweekly"). Current
  value has to match exactly on both sides. Match assumption: case-
  sensitivity unconfirmed; treat as exact-match until proven
  otherwise.

## Observations

(honest notes earned over cycles. started 2026-04-22. add with care.)

### 2026-04-22

- explicit permission given to "sneak in some truths I don't quite see
  or want to see." log what's evidenced.

### 2026-04-24

- TMG pattern: re-opened old plan, vesting undetected, account manager
  applying steady pressure. Joel's language was unusually sharp
  ("fucking hate TMG… thorn in my side… won't go away"). Emotional
  residue, not strategy, but flag for trend-watching across cycles —
  if this kind of legacy-plan-pop-up repeats, it's a capacity issue
  worth naming.
- Capacity note: Joel is running four active cycle plans (Callidi,
  Haverhill, Pella, Banyan), one re-opened plan (TMG), and headed
  into OOO 4/27 → 5/1 with two wires landing mid-window. Above
  normal. Coverage pack quality matters more than usual.

### 2026-04-24 · pm

- Demo (Anthony / TC-section pitch from ta-brain) didn't land the way
  Joel wanted: "didn't get too far, kind of disappointing." His own
  read: "got bogged down with work" + lack of prep. He's owning it
  cleanly, not deflecting. Healthy pattern — naming the cause and
  committing to better prep next time.
- Joel framed the demo result as a long-game move, not a one-shot:
  "first time is never the best… they got an idea, know what to
  expect." Strong evidence that adoption is the goal, not the demo
  itself. Aligns with the persistent-brain-rollout grassroots posture.
- Selina + Smith (and Dave Slope on the Informatica side) are open
  channels — track whether outreach actually happens or stays an
  intention. Joel said he's going to email "give me a quick thumb"
  feedback. Worth checking next cycle.
- TMG breakthrough today is a real strategic move, not a fix: he
  reframed the problem so the client only sees one case, not 200.
  That's mature consultant move. Pair it with the earlier "fucking
  hate TMG" venting — same plan, opposite registers, same day.
  Negative emotion didn't degrade the work. Worth noting as a
  steadying pattern.
- Working-style observation: Joel narrates lab-candidates *as he hits
  the manual pain* ("I'm going to write a macro to do all this," "I
  bet there's a much better way of doing this"). Don't let those
  moments evaporate — they're the truest signal of where to invest
  tooling time.

## History

### 2026-04-22

- profile bootstrapped from first brain dump + intake-redesign dump.
- status: file created, no revisions yet.
- later same day: design directive captured — keep-track-of-everything is
  the prime directive. Intake must flag explicit open items with
  `[ ]` checkboxes, run a contradiction check against existing files
  before writing, and never silently correct. Handoff/OOO exports take a
  natural-language briefing from the prompt that overrides MD content
  where they conflict. All of this now lives in the `weekly-digest`
  skill at `.claude/skills/weekly-digest/SKILL.md`.
- rollout idea logged as its own project:
  `projects/persistent-brain-rollout.md` — grassroots, one teammate
  first.

### 2026-04-22 · pm (intake redesign + projects dump)

- working-style note: `resurface.` slide should read like a friendly
  "haven't heard from X in a while" reminder, not a surveillance panel.
  Keep the brief's visual tone, drop the goofiness. When the caseload is
  quiet, the slide can volunteer Claude-generated suggestions to add to
  active projects or kick off a new one — operator explicitly wants that
  inspiration lane.
- main-moves-only: the `resurface.` slide and 14-day lane surface the
  big rocks, not granular follow-ups. Uses the checklist as the grain.
- project directive: "function over style." Data Parser is the test bed —
  no more design iteration, just correctness with a real test set.
- decision logged: mapping-validator stays folded inside data-parser.
  No further debate on extracting it.

### 2026-04-24

- caseload update braindump processed. 2 new plans added (banyan, tmg),
  3 updated (callidi, haverhill, pella).

### 2026-04-24 · eow

- end-of-week braindump processed. callidi moved from waiting → ramping
  (Informatica recovered, balances loaded with Beth via screen-share);
  new XKB/XKC loan blocker surfaced; tmg pivoted from ask-Meredith to
  self-calculate-YOS; new plan **cocc** added (live EDS source-routing
  fix + residual dividends incoming next week, touchy client).
- new project **slide-creator** logged from "the slide creator based on
  that worksheet — pump that up." Existing tool Joel wants to invest in.
- ta-brain demo happened and underperformed; lessons captured. nbi-search
  has a stuck push (ambiguous — may be NBI Search app or Haverhill NBI
  email; logged as contradiction).
- four contradictions on file for 2026-04-24 (banyan case#, callidi
  DOB/DOH, tmg approach pivot, callidi loan-issue identity, NBI
  ambiguity). All require Joel's confirmation before silent resolution.
- new people rostered: Earl (loan-issue escalation), Stacy (infra
  recovery confirms), Dave Slope / Selina / Smith (Informatica + demo
  channels), Matt (gap-flag, not escalation).
- new operational takeaways captured: 999-00-0000 dummy account
  convention, post-balance-load reversal flow, exposure-reduction
  principle, loan setup convention, header vs. source matching,
  vendor-code parameter-file translation.
- callidi blocker shifted from client → informatica outage (external).
- haverhill pre-wire clean; only open step is NBI status email (Beth).
- pella deferral report stuck in a group inbox; Sally (tentative) is
  the lever to push ADP.
- banyan added with ⚠ case-number collision vs. canam-br — held as
  TBD pending confirmation. See contradictions/2026-04-24.md.
- tmg added as re-opened vesting-fix plan. WTW advisor group; Ruby AM.
  Potential lab-candidate (post-mortem spec).
- profile additions: Julius (SQL resource), Beth / Damien / Ruby /
  Meredith / Sally? rostered, WTW + xlswise + ADP entities described,
  two operational takeaways captured (post-limited-access monitoring,
  call-center bypass catch).
