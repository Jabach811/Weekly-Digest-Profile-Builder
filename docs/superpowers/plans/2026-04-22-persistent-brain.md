# Persistent Brain Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Stand up the folder-native knowledge base that backs the Weekly Brief — profile, caseload, projects, braindumps — seeded from current state, plus extend `weekly-brief-spec.md` to cover the new `resurface.` slide and intake loop.

**Architecture:** Pure markdown + YAML frontmatter. No code, no generator. Claude maintains the files by hand each cycle per the intake loop in the design spec. File shape is uniform across profile/plan/project so the same author rules apply everywhere.

**Tech Stack:** Markdown, YAML frontmatter. Filesystem only. Git for history (optional; not required by this plan).

**Spec:** [docs/superpowers/specs/2026-04-22-persistent-brain-design.md](../specs/2026-04-22-persistent-brain-design.md)

---

## File Structure

Files to create (relative to `Weekly Digest/`):

- `profile/identity.md`
- `caseload/callidi.md`
- `caseload/prime.md`
- `caseload/pella.md`
- `caseload/canam-br.md`
- `caseload/stoutco.md`
- `caseload/bonnier.md`
- `caseload/leibys.md`
- `caseload/canam-bd.md`
- `caseload/haverhill.md`
- `caseload/isd.md`
- `projects/data-parser.md`
- `projects/mapping-validator.md`
- `projects/nbi-search.md`
- `projects/ta-brain.md`
- `projects/ta-brain-tc-section.md`
- `projects/app-portfolio-site.md`
- `projects/excel-to-app-pipeline.md`
- `braindumps/2026-04-22-intake-redesign-and-projects.txt`

Files to modify:

- `weekly-brief-spec.md` — append §10 (resurface slide) and §11 (intake loop)

Each plan file corresponds 1:1 with a row in `plans-registry.csv`. No registry changes in this plan; statuses already match.

---

## Task 1: Create Folder Skeleton

**Files:**
- Create: `profile/`, `caseload/`, `projects/`, `braindumps/`

- [ ] **Step 1: Create directories**

Run:
```bash
mkdir -p "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/profile" \
         "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/caseload" \
         "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/projects" \
         "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/braindumps"
```

- [ ] **Step 2: Verify**

Run:
```bash
ls -la "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/"
```
Expected: `profile`, `caseload`, `projects`, `braindumps` all present.

---

## Task 2: Archive Today's Braindump

**Files:**
- Create: `braindumps/2026-04-22-intake-redesign-and-projects.txt`

- [ ] **Step 1: Archive verbatim**

Source: the operator's 2026-04-22 braindump — **two parts**, both
delivered in the original brainstorming conversation:

1. the intake-redesign voice dump (describes profile folders, tagging,
   resurface slide, inspiration slide).
2. the projects voice dump (covers Data Parser, Mapping Validator kill
   decision, NBI Search, TA Brain, Anthony / TC section, future
   Excel→app pipeline, portfolio site).

Concatenate both into one file at `braindumps/2026-04-22-intake-redesign-and-projects.txt`,
in order (intake redesign first, projects second), separated by a single
blank line. No edits, no cleanup, no trimming. Preserve original
paragraph breaks. If the executor is a subagent and doesn't have the
transcript in context, the orchestrating session must pass it in.

- [ ] **Step 2: Verify**

Run:
```bash
wc -l "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/braindumps/2026-04-22-intake-redesign-and-projects.txt"
```
Expected: non-zero line count matching source.

---

## Task 3: Write `profile/identity.md`

**Files:**
- Create: `profile/identity.md`

- [ ] **Step 1: Write the file**

Contents:

```markdown
---
type: profile
key: joel
tags: [identity, working-style, goals]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
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

## Observations

(honest notes earned over cycles. started 2026-04-22. add with care.)

### 2026-04-22

- explicit permission given to "sneak in some truths I don't quite see
  or want to see." log what's evidenced.

## History

### 2026-04-22

- profile bootstrapped from first brain dump + intake-redesign dump.
- status: file created, no revisions yet.
```

- [ ] **Step 2: Verify frontmatter parses**

Run:
```bash
head -8 "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/profile/identity.md"
```
Expected: a clean YAML frontmatter block (triple-dash fenced) followed by a blank line, then `# Joel`.

---

## Task 4: Write Caseload Files (10 plans)

Each caseload file uses the same frontmatter shape and body structure. Statuses match `plans-registry.csv`. Dated sub-heading `### 2026-04-22` captures current state from the First Brain Dump.

**Files:**
- Create: `caseload/callidi.md`
- Create: `caseload/prime.md`
- Create: `caseload/pella.md`
- Create: `caseload/canam-br.md`
- Create: `caseload/stoutco.md`
- Create: `caseload/bonnier.md`
- Create: `caseload/leibys.md`
- Create: `caseload/canam-bd.md`
- Create: `caseload/haverhill.md`
- Create: `caseload/isd.md`

- [ ] **Step 1: Write `caseload/callidi.md`**

```markdown
---
type: plan
key: callidi
case_number: JK62945-00001
status: waiting
scope: full
day1: 2026-01-01
effective_date: 2026-04-01
asset_transfer_date: 2026-04-10
tags: [client-blocked, loan-issue, asset-transfer]
last_mentioned: 2026-04-22
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
```

- [ ] **Step 2: Write `caseload/prime.md`**

```markdown
---
type: plan
key: prime
case_number: QK62489-00001
status: on_hold
scope: full
day1: 2026-01-01
effective_date: 2026-08-01
asset_transfer_date: 2026-08-01
tags: [pushed, monitor]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Prime Healthcare

### 2026-04-22

- Pushed to 8/1 — effectively off the radar for the next couple months.
- No active work. Monitor only.
```

- [ ] **Step 3: Write `caseload/pella.md`**

```markdown
---
type: plan
key: pella
case_number: QK63283-00052
status: active
scope: full
day1: 2026-03-01
effective_date: 2026-05-01
asset_transfer_date: 2026-05-01
tags: [adp, deferral-workaround, ramping]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Pella SC

### 2026-04-22

- Ramping. Population already largely in our system, so records side
  should be easy.
- ADP is the source — their records are clean for census but
  historically broken for deferrals (can't tell if a participant is
  truly still in default within the min/max window). Known workaround:
  load everyone out of default at their current %, hand the client the
  ambiguous in-window group so they can notify those participants
  directly.
- New contact: ADP rep replaced the previous one. She seems ready.
  Need: her name + email logged.
- **Next moves:** email ADP rep to get deferral report ahead of time;
  confirm whether a base file is needed.
- **Off-ramp:** deferral report in hand + liquidation complete → load
  and move on.
```

- [ ] **Step 4: Write `caseload/canam-br.md`**

```markdown
---
type: plan
key: canam-br
case_number: QK63283-00099
status: future
scope: full
day1: 2026-04-22
effective_date: 2026-10-01
asset_transfer_date: 2026-10-01
tags: [future, monitor]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Canam Bridges

### 2026-04-22

- 10/1. Way out. No active work. Monitor only.
```

- [ ] **Step 5: Write `caseload/stoutco.md`**

```markdown
---
type: plan
key: stoutco
case_number: QK63283-00120
status: future
scope: full
day1: 2026-04-22
effective_date: 2026-07-01
asset_transfer_date: 2026-07-01
tags: [excel-wise, future, monitor]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Stoutco

### 2026-04-22

- 7/1. Excel-wise plan — historically simple. No meeting yet.
- Monitor only.
```

- [ ] **Step 6: Write `caseload/bonnier.md`**

```markdown
---
type: plan
key: bonnier
case_number: QK63283-00105
status: future
scope: full
day1: 2026-04-01
effective_date: 2026-07-01
asset_transfer_date: 2026-07-01
tags: [excel-wise, future, monitor]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Bonnier Corporation

### 2026-04-22

- 7/1. Excel-wise. Way out. Monitor only.
```

- [ ] **Step 7: Write `caseload/leibys.md`**

```markdown
---
type: plan
key: leibys
case_number: QK63283-00112
status: future
scope: full
day1: 2026-04-22
effective_date: 2026-08-01
asset_transfer_date: 2026-08-01
tags: [excel-wise, future, monitor]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Leiby's Dairy

### 2026-04-22

- 8/1. Excel-wise. Meeting canceled today. Effectively off the radar.
- Monitor only.
```

- [ ] **Step 8: Write `caseload/canam-bd.md`**

```markdown
---
type: plan
key: canam-bd
case_number: QK63283-00113
status: future
scope: full
day1: 2026-04-01
effective_date: 2026-10-01
asset_transfer_date: 2026-10-01
tags: [future, monitor]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Canam Buildings

### 2026-04-22

- 10/1. Way out. Monitor only.
```

- [ ] **Step 9: Write `caseload/haverhill.md`**

```markdown
---
type: plan
key: haverhill
case_number: TA069932-00051
status: active
scope: full
day1: 2026-03-01
effective_date: 2026-05-01
asset_transfer_date: 2026-05-01
tags: [contact-change, coverage-needed, ramping]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Haverhill (Havern School)

### 2026-04-22

- Cracking soon. 5/1 assets inbound.
- Contact change: original client contact had brain surgery; backup
  has been with the plan the whole time, so continuity is fine.
- Base file loaded. Beth got everyone in; AQT fix explained; flags
  placed.
- **Next moves:** follow up with Damien on the participants who didn't
  land in the loaded set — add them if needed, note dates; plan
  coverage during OOO (4/27–5/1) because assets arrive mid-window.
- **Off-ramp:** assets loaded post-wire, post-wire reconciliation clean.
- Hand-off required during OOO.
```

- [ ] **Step 10: Write `caseload/isd.md`**

```markdown
---
type: plan
key: isd
case_number: TA069932-00056
status: active
scope: partial
day1: 2026-04-01
effective_date: 2026-07-01
asset_transfer_date: 2026-07-01
tags: [startup, partial-scope, deferral-debate]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# International School of Denver (ISD)

### 2026-04-22

- Partial scope: census + elections + deferrals only. No asset transfer.
- Merger being treated as a startup. Limited-access window waived —
  nothing loaded at go-live; participants get a window to self-elect;
  client then sends a deferral file that imports over the top (existing
  self-elected entries override via effective-dated precedence).
- **Strategic debate:** one camp says startup = clean start with no
  blackout; other camp says the delayed-load workflow sets a bad
  precedent. Expect a broader policy discussion — multiple incoming
  plans will hit the same shape.
- **Question worth raising:** if the client already has the deferral
  file, ask for it now rather than after — no stated reason to wait.
- **Next moves:** deliver clean analysis package on the deferral
  workflow; raise the "get the file earlier" question.
- **Off-ramp:** live date hits, deferrals imported, no overrides
  stomp on self-elections.
```

- [ ] **Step 11: Verify all 10 files exist**

Run:
```bash
ls -la "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/caseload/"
```
Expected: `callidi.md`, `prime.md`, `pella.md`, `canam-br.md`, `stoutco.md`, `bonnier.md`, `leibys.md`, `canam-bd.md`, `haverhill.md`, `isd.md`.

---

## Task 5: Write Project Files (7 projects)

**Files:**
- Create: `projects/data-parser.md`
- Create: `projects/mapping-validator.md`
- Create: `projects/nbi-search.md`
- Create: `projects/ta-brain.md`
- Create: `projects/ta-brain-tc-section.md`
- Create: `projects/app-portfolio-site.md`
- Create: `projects/excel-to-app-pipeline.md`

- [ ] **Step 1: Write `projects/data-parser.md`**

```markdown
---
type: project
key: data-parser
status: flight
tags: [data, parser, test-coverage, function-first]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Data Parser

Purpose: parse and validate incoming data files for plan conversions.

### 2026-04-22

- Functionality ~100%. No more style/design iteration — function only.
- Open questions:
  - Keep the secondary summary report (sub-analysis) or kill it?
    If keep → tighten the logic.
  - Either path requires better test files to prove correctness.
- Rule set for this project now: no design work. Only correctness.
- The Mapping Validator lives inside this project — do not extract.

### Open Items

- build a golden-set of test input files.
- decide on summary report: kill or tighten.
- verify mapping-validator behavior is covered by the existing parser
  tests (or add coverage).
```

- [ ] **Step 2: Write `projects/mapping-validator.md`**

```markdown
---
type: project
key: mapping-validator
status: shipped-folded
tags: [decision-logged, folded-into-data-parser]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Mapping Validator

Decision (2026-04-22): **killed as a standalone project.** Lives inside
Data Parser. Do not revisit extraction — the functionality is already
there and 2-for-1 is the point.

### 2026-04-22

- status: idea → shipped-folded.
- This file exists only to log the decision and prevent the idea from
  silently re-surfacing later.
```

- [ ] **Step 3: Write `projects/nbi-search.md`**

```markdown
---
type: project
key: nbi-search
status: ready-to-ship
tags: [ui, search, ship-blocker-file-path]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# NBI Search

Purpose: fast search UI over NBI data. Design is locked and loved.

### 2026-04-22

- Design complete. Do not iterate on it.
- One ship-blocker: the file picker. Replace with a hard-coded path
  (or config-loaded path) and a refresh button.
- **Next moves:** remove file-picker; wire refresh button; decide
  whether path is literal or config.
- **Off-ramp:** path-free launch experience → shippable.
```

- [ ] **Step 4: Write `projects/ta-brain.md`**

```markdown
---
type: project
key: ta-brain
status: flight
tags: [ui, search, animations, always-on]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# TA Brain

Always-on project. Never fully off the list.

### 2026-04-22

- Animations: current placement is OK for now — table "better placement"
  work. Priority is **more** animations for other processes that don't
  have them.
- Concepts page: bloated, reads as one blur. Explore lighter
  categorization without over-categorizing (sidebar should stay open,
  not collapsed).
- Search: works, but could be more visible. Add a backslash command
  palette (same pattern as NBI Tracker).
- Anthony: aim for a Friday demo this week to open up the TC section
  conversation. Demo must prove data-safety (sanitizer agent).

### Open Items

- find more places to add animation.
- redesign concepts page layout.
- add `/` command palette to search.
- schedule Anthony demo; prep data-safety walkthrough.
```

- [ ] **Step 5: Write `projects/ta-brain-tc-section.md`**

```markdown
---
type: project
key: ta-brain-tc-section
status: idea
tags: [depends-on-anthony, data-safety, expansion]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# TA Brain — TC Section

### 2026-04-22

- Depends on: Anthony buy-in, which depends on a clean data-safety
  demo.
- Value: unlocks a large section that's currently off-limits. "Blow it
  out" once approved.
- **Next move:** land the Anthony demo (tracked in ta-brain.md).
```

- [ ] **Step 6: Write `projects/app-portfolio-site.md`**

```markdown
---
type: project
key: app-portfolio-site
status: idea
tags: [infra, dashboard, future]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# App Portfolio Site

### 2026-04-22

- Future dashboard hosting all side-apps under one roof.
- Not active. Surfaces when enough apps are shipped to warrant it.
- **Next move (when ready):** pick hosting + a minimal index shell.
```

- [ ] **Step 7: Write `projects/excel-to-app-pipeline.md`**

```markdown
---
type: project
key: excel-to-app-pipeline
status: idea
tags: [pattern, ongoing, conversion]
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Excel → App Pipeline

### 2026-04-22

- Ongoing pattern, not a single project: take old Excel one-offs and
  rebuild them as proper small apps.
- This file exists to catch candidate conversions as they come up. Each
  time a specific one starts, spin up its own `projects/<key>.md`.

### Candidates (add as surfaced)

- (none logged yet)
```

- [ ] **Step 8: Verify all 7 files exist**

Run:
```bash
ls -la "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/projects/"
```
Expected: all seven project files present.

---

## Task 6: Extend `weekly-brief-spec.md` with §10 and §11

**Files:**
- Modify: `weekly-brief-spec.md` — append sections 10 and 11 after §9.

- [ ] **Step 1: Open the spec and locate the end**

Run:
```bash
wc -l "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/weekly-brief-spec.md"
```
Confirm total line count so you append cleanly at the end.

- [ ] **Step 2: Append §10 — The `resurface.` Slide**

Append the following to the bottom of `weekly-brief-spec.md`:

```markdown

---

## 10. The `resurface.` Slide

Sits between Monitor (§8) and The Lab (§9). Same retro-console theme.
No emoji, no camp. Surfaces quiet corners and gaps so nothing rots.

### Structure

Up to three stacked `.panel` blocks. Render only panels with rows.
If all three are empty, the slide is omitted entirely.

```
[eye + h-display + lede]
  § 0X / TRANSMISSION 0NN / RESURFACE
  haven't heard about these.
  quiet corners worth a glance before the next cycle closes.

PANEL · HAVEN'T HEARD FROM
  row: <name> · <type chip> · LAST MENTIONED: <date> · <N weeks>

PANEL · YOU MENTIONED, I DIDN'T CATCH
  row: <name> · "you said: <short quote>" → "need: <missing piece>"

PANEL · WORTH A LOOK
  row: <name> · <specific one-liner> · <chip: extend | revive | pattern>
```

### Panel Routing

- **`Haven't Heard From`** = silence only (dormancy).
- **`You Mentioned, I Didn't Catch`** = mentioned but left hanging
  (no next move, no date on a new contact, status without support,
  decision implied but not made).
- **`Worth a Look`** = Claude's own suggestions. Max 3 per cycle.
  Must be specific (e.g. "TA Brain search — add `/` command palette,
  pattern from NBI Tracker"). Sources: cross-project patterns,
  extensions for shipped work, follow-through on half-promises.

### Dormancy Thresholds

| Entity type                | Soft nudge  | Loud nudge (rust chip) |
|----------------------------|-------------|------------------------|
| Project                    | 6 weeks     | 12 weeks               |
| Plan (`status: active`)    | 2 cycles    | 3 cycles               |
| Plan (`future` / `on_hold`)| never       | never                  |

Input: `last_mentioned` in the note's frontmatter. Day1 math is
forbidden here (same rule as telemetry §9).

### Authoring Rules

- Max 3 rows per panel; overflow → `+N MORE` in mono caps.
- Lede is always: "haven't heard about these. / quiet corners worth a
  glance before the next cycle closes."
- No repetition with Telemetry (§9). If telemetry already surfaced a
  milestone for a plan, don't duplicate it on `resurface.`.

## 11. Intake Loop (braindump → files)

Authoritative process is documented in
`docs/superpowers/specs/2026-04-22-persistent-brain-design.md §4`.
Summary here for the deck-author's reference:

1. Archive raw dump to `braindumps/YYYY-MM-DD-<topic>.txt`.
2. Classify each chunk into: plan / project / profile / gap.
3. Update the target file:
   - bump `last_mentioned`
   - move `status` if shifted
   - append body under a new `### YYYY-MM-DD` sub-head
   - never rewrite old content
4. Update `plans-registry.csv` when a plan's status/date shifts.
5. Detect gaps (feeds §10 `resurface.`).
6. Compile the deck.

Keys (`data-plan` / filename / registry row) are permanent. Never
rename mid-cycle — it breaks checklist `localStorage` and file history.
```

- [ ] **Step 3: Verify append**

Run:
```bash
tail -20 "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/weekly-brief-spec.md"
```
Expected: the last lines show the §11 intake loop content, ending with the "keys are permanent" line.

- [ ] **Step 4: Verify no duplicate or overwritten sections**

Run (Grep, not bash):
- Grep pattern: `^## 10\.` in `weekly-brief-spec.md` — expect 1 match.
- Grep pattern: `^## 11\.` in `weekly-brief-spec.md` — expect 1 match.
- Grep pattern: `^## 9\.` in `weekly-brief-spec.md` — expect 1 match.

If any section count is off, do NOT retry the append — read the file, diff against the original, and fix by hand.

---

## Task 7: Final Verification

- [ ] **Step 1: File count check**

Run:
```bash
find "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/profile" \
     "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/caseload" \
     "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/projects" \
     "C:/Users/mabac/OneDrive/Desktop/Weekly Digest/braindumps" \
     -maxdepth 1 -type f | wc -l
```
Expected: `19` (1 profile + 10 caseload + 7 projects + 1 braindump).

- [ ] **Step 2: Frontmatter sanity check**

Grep pattern: `^---$` across `profile/`, `caseload/`, `projects/` — expect exactly 2 per file (open + close). A count of (files × 2) confirms every file has a well-formed frontmatter block.

- [ ] **Step 3: Key/filename match check**

For each caseload file, confirm the `key:` value in the frontmatter matches the filename stem. Same for projects. Same for `profile/identity.md` (key should be `joel`).

- [ ] **Step 4: Registry consistency check**

Open `plans-registry.csv`. For each row, confirm a matching
`caseload/<key>.md` exists and `status` in frontmatter matches the CSV
column. If anything drifts, fix the markdown (CSV is authoritative for
dates; markdown is authoritative for narrative).

- [ ] **Step 5: Spec cross-reference**

Open `weekly-brief-spec.md`. Confirm the new §10 and §11 are present and
well-formed. Confirm the cross-reference from §11 to
`docs/superpowers/specs/2026-04-22-persistent-brain-design.md` is
accurate.

- [ ] **Step 6: Report done**

Output a summary to the user listing:
- folders created
- files created (grouped by folder, with counts)
- spec sections appended
- any inconsistencies found and resolved
