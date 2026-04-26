# Persistent Brain — Design Spec

A folder-native knowledge system that sits next to the Weekly Brief deck,
turns voice braindumps into tagged markdown, and feeds the deck
(including a new `resurface.` slide) with accurate, rolling state.

Companion to `weekly-brief-spec.md`. This spec covers the **intake,
storage, and new slide**. It does not change existing slide templates.

---

## 1. Purpose

Joel does weekly braindumps — usually phone, usually between calls. The
brief compiles cleanly but the *memory* between cycles has lived only in
the braindumps and his head. This spec adds persistent, tagged memory in
the form of markdown files the deck can read every cycle.

Goals:

- Capture identity, caseload, and side-project state as durable notes.
- Route any braindump into the right note without ambiguity.
- Surface things the operator didn't mention but should have, and things
  that have gone quiet.
- Keep the whole system human-readable, diff-friendly, phone-authorable.

Non-goals:

- No app, no database, no web UI. Markdown + frontmatter only.
- No generator pipeline yet — compile by hand. (Future: see §8.)
- No "personal" folder (explicit user carve-out). Personal apps live
  elsewhere.

---

## 2. Folder Layout

```
Weekly Digest/
├─ profile/
│  └─ identity.md               single file, natural language
├─ caseload/
│  └─ <plan-key>.md             one per plan (10 today)
├─ projects/
│  └─ <project-key>.md          one per side-project (~7 to start)
├─ findings/
│  └─ findings.md               rolling log of one-off things to remember
├─ ledger/
│  ├─ wins.md                   explicit wins, declared by operator
│  └─ losses.md                 explicit losses, declared by operator
├─ braindumps/
│  └─ YYYY-MM-DD-<topic>.txt    raw transcripts, never edited
├─ docs/superpowers/specs/
│  └─ YYYY-MM-DD-<topic>-design.md   design specs (this file)
├─ plans-registry.csv           unchanged (drives telemetry)
├─ weekly-brief-spec.md         unchanged
└─ caseload-deck-NNN.html       unchanged
```

Key rules:

- **Keys are stable.** `callidi`, `nbi-search`, `ta-brain`. Same key
  used as filename, registry row, deck `data-plan`, checklist id prefix.
- **Filenames are the key.** `caseload/callidi.md`, not
  `caseload/callidi-tas.md`.
- **One entity per file.** Don't merge plans into themes.
- **Braindumps are read-only archives.** Never edit the raw text.

---

## 3. File Shape

Every note — profile, plan, project, findings, win/loss log — uses the
same frontmatter shape. Status fields apply to plans and projects only;
profile/findings/ledger files omit them.

```yaml
---
type: profile | plan | project | findings | win-log | loss-log
key: callidi                    # matches plans-registry.csv key
status: active | waiting | on_hold | final
                | idea | scaffold | flight | shipped | ready-to-ship
                | shipped-folded
                # omit on profile / findings / ledger files
tags: [excel-wise, client-blocked, adp]   # free-form, lowercase-hyphen
last_mentioned: 2026-04-22
first_logged: 2026-04-22
---

# Display Name

Natural-language body. Dictated → transcribed → appended under a dated
heading.

### 2026-04-22

- what's true today
- next move
- blocker
- off-ramp (for plans) / open questions (for projects)

### 2026-04-08

- prior cycle's notes, untouched
```

Status vocab:

- **Plan statuses** (match registry): `active`, `waiting`, `on_hold`,
  `final`, `future`.
- **Project statuses**: `idea`, `scaffold`, `flight`, `ready-to-ship`,
  `shipped`, `shipped-folded` (shipped but folded into another project).
- **Profile** has no status field — it's never "done."

Tags are free-form. Conventions:

- `client-blocked`, `self-blocked`, `coverage-needed` — blocker class
- `adp`, `excel-wise`, `startup`, `merger` — plan shape
- `ui`, `data`, `infra`, `agent` — project axis
- `demo-ready`, `needs-test-files`, `design-complete` — gates

History rule: **append, don't rewrite.** New cycle = new dated heading.
Past content stays visible. This is how the file becomes the arc of the
plan/project over time.

---

## 4. Intake Flow

One loop every time a new braindump lands:

1. **Archive raw.** Save to `braindumps/YYYY-MM-DD-<topic>.txt` verbatim.
2. **Classify.** Each chunk goes to one of:
   - Plan talk → `caseload/<key>.md`
   - Project talk → `projects/<key>.md`
   - Identity / goal / preference → `profile/identity.md`
   - **One-off observation / gotcha / heuristic** → `findings/findings.md`
   - **"That's a win"** (operator-declared) → `ledger/wins.md`
   - **"That's a loss"** (operator-declared) → `ledger/losses.md`
   - Gap → nothing to route; surfaces on the `resurface.` slide
3. **Update the right file(s).** For each touched entity:
   - Bump `last_mentioned` to today.
   - Update `status` in frontmatter if it shifted.
   - Append body content under a new `### YYYY-MM-DD` sub-heading.
   - Never rewrite old content.
4. **Update `plans-registry.csv`** if a plan's status/date moved.
5. **Detect gaps** (feeds the `resurface.` slide — §5).
6. **Compile the deck** (manual for now — §8 is the future generator).

Ambiguity rule: when a dump is unclear, ask **once**, then default to
the conservative read (usually "leave it alone"). When it's clear,
execute without asking.

**Win/loss declaration is explicit.** The operator has committed to
flagging wins and losses with unmistakable language. Never infer a win
or a loss from tone or context — only route to `ledger/*` when the
operator said "that's a win" / "that was a loss" / equivalent plain
marker. On the rare ambiguous call, leave it unrouted and ask.

**Findings are appended freely.** Any one-off fact, gotcha, vendor
quirk, heuristic, or decision the operator wants remembered lands in
`findings/findings.md`. Don't wait for an explicit marker — if the
operator says "worth remembering" or makes a statement that looks like
a reusable rule, log it. If unsure, log it: findings are cheap; the
cost is missing one.

---

## 5. The `resurface.` Slide

New slide in the deck. Sits between **Monitor (§8 of brief spec)** and
**The Lab (§9 of brief spec)**. Same retro-console theme: `eye` label,
`h-display` heading, panel stack, corner brackets, no emoji, no camp.

### Structure

Up to three stacked panels. Render only if the panel has rows.

```
[eye + h-display + lede]
  § 0X / TRANSMISSION 0NN / RESURFACE
  haven't heard about these.
  quiet corners worth a glance before the next cycle closes.

PANEL · HAVEN'T HEARD FROM
  dormancy nudges — projects/plans past threshold
  row: <name> · <type chip> · LAST MENTIONED: <date> · <N weeks>

PANEL · YOU MENTIONED, I DIDN'T CATCH
  gap report — name drops with no next move; new contacts w/o a date;
  status mentions w/o resolution
  row: <name> · "you said: <short quote>" → "need: <missing piece>"

PANEL · WORTH A LOOK
  claude's own suggestions — max 3 — only when active work is light
  or a shipped project has been quiet
  row: <name> · <specific one-liner> · <chip: extend | revive | pattern>
```

### Rules

- If all three panels are empty, the slide **does not render**.
- Max **3 rows per panel**; overflow collapses to mono-caps `+N MORE`.
- No emoji. No exclamations. Tone matches transmission / monitor slides.
- `Worth a look` suggestions must be **specific** — not "think about
  TA Brain" but "TA Brain search — add backslash command palette
  (pattern from NBI Tracker)."
- Suggestions come from three sources:
  1. Patterns seen in one project that fit another (cross-pollination).
  2. Extension ideas for shipped projects (keep them alive).
  3. Follow-through on things the operator half-promised himself.

### Dormancy Thresholds

Tuneable; these are the starting defaults.

| Entity type     | Soft nudge  | Loud nudge       |
|-----------------|-------------|------------------|
| Project         | 6 weeks     | 12 weeks (rust chip) |
| Plan (`active`) | 2 cycles    | 3 cycles (rust chip) |
| Plan (`future`/`on_hold`) | never | never         |

`last_mentioned` on the file is the sole input. No Day1 math (same rule
as telemetry §9).

### Panel Routing Rules

The distinction between the two gap panels:

- **`Haven't Heard From`** — entity went **silent**. Dormancy only.
  Active plan missed for 2+ cycles, project quiet for 6+ weeks.
- **`You Mentioned, I Didn't Catch`** — entity **was mentioned**, but
  something was left hanging:
  - Name-drop without a next move ("you mentioned Anthony — need:
    confirmed day + demo outline").
  - New contact / vendor without a date or email logged.
  - Status claimed but not supported ("ready to ship" with open
    blockers).
  - Decision implied but not made ("maybe we push this" → which is it?).

---

## 6. Bootstrap — Day-One File Set

On approval of this spec, create:

**`profile/identity.md`** — Joel, Senior Data Consultant. Working style,
goals, how-to-talk-to-me, constraints, observations section (grows over
time).

**`caseload/*.md`** (10 files, seeded from First Brain Dump.txt):

- `callidi.md` — waiting · client DOB/DOH · loan re-amort
- `prime.md` — on_hold · pushed to 8/1
- `pella.md` — active · ADP deferral report · new rep
- `canam-br.md` — future · 10/1
- `stoutco.md` — future · Excel-wise · 7/1
- `bonnier.md` — future · Excel-wise · 7/1
- `leibys.md` — future · Excel-wise · 8/1
- `canam-bd.md` — future · 10/1
- `haverhill.md` — active · 5/1 assets incoming · coverage needed during OOO
- `isd.md` — active · partial scope · deferral debate open

**`projects/*.md`** (seeded from today's voice dump):

- `data-parser.md` — `flight` · functionality 100% · tighten summary or
  kill it · need golden-set test files · **no more style work**
- `mapping-validator.md` — `shipped-folded` · decision logged: lives
  inside data-parser, do not extract
- `nbi-search.md` — `ready-to-ship` · remove file-picker · hard-code
  path + refresh button · design locked
- `ta-brain.md` — `flight` · always-on · animations, concepts page
  refinement, backslash command palette for search · Anthony demo Friday
- `ta-brain-tc-section.md` — `idea` · depends on Anthony buy-in +
  data-safety demo (sanitizer agent referenced)
- `app-portfolio-site.md` — `idea` · future dashboard for all side-apps
- `excel-to-app-pipeline.md` — `idea` · ongoing conversion pattern

**`braindumps/2026-04-22-intake-redesign-and-projects.txt`** — today's
full voice dump, verbatim.

No changes yet to `plans-registry.csv` or `weekly-brief-spec.md`. Those
stay authoritative. A follow-up edit to `weekly-brief-spec.md` will add
§10 documenting the `resurface.` slide and §11 documenting the intake
loop, so the brief spec stays the single source of truth for deck
structure.

---

## 7. Authoring Conventions

- **Lowercase-hyphen tags.** `client-blocked`, not `ClientBlocked`.
- **One entity per file.** Never merge two plans or two projects.
- **Dated sub-heads only.** `### 2026-04-22` — no topic-based sections
  inside a plan/project file. The history IS the structure.
- **Status moves are explicit.** Change the frontmatter, and drop a one-
  liner in the dated body: `- status: flight → ready-to-ship (file path
  hard-coded today).`
- **Next moves are imperative.** "Email new ADP rep." Not "waiting on."
- **Observations in the profile are honest.** The operator gave
  explicit permission to surface truths he might not want to see. Log
  what's evidenced, not what's guessed. Multi-cycle patterns get the
  strongest weight; single-cycle reads are fair game when grounded in
  a concrete quote.

---

## 8. Not In Scope (Yet)

These are deliberately out of this spec. Each can be its own future
design.

- JSON/YAML generator that compiles the deck from the markdown set.
- Automated `last_mentioned` bumping (currently: I update by hand on
  each intake).
- Search/indexing across the markdown set (grep is fine for now).
- Archive strategy for `shipped` projects and `final` plans after N
  cycles.
- Print stylesheet, theme toggle — tracked under brief spec §7.
- Cycle-number bump helper — tracked under brief spec §7.

---

## 9. Glossary

- **Cycle** — the 2-week Wednesday→Wednesday window of one brief.
- **Braindump** — transcribed voice memo covering plans, projects,
  identity shifts, or OOO.
- **Resurface** — the new slide surfacing dormant/missed/suggested
  items.
- **Gap** — something the operator implied but didn't close out
  (name-drop with no next move, status with no support, new contact
  with no date).
- **Dormancy** — time since `last_mentioned` for a given entity.
- **Key** — the stable slug identifying a plan or project across
  registry, deck, checklist, and filename.
