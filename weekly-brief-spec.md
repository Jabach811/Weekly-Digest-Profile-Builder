# Weekly Brief — Design System & Template Spec

A working spec for the recurring two-week caseload brief. Captures the design
language, slide templates, and the data shape needed to drive the brief from
a braindump.

---

## 1. Concept

- **Cadence:** new brief every two weeks. Wednesday → Wednesday.
- **Inputs:** a voice braindump (transcribed) covering current plans, blockers,
  side-project context, and OOO windows.
- **Outputs:** a single-file HTML deck — masthead, triage board, plan details,
  monitor strip, side-project lab, day-by-day plan, live checklist.
- **Aesthetic:** warm retro-future console. Calm, dense where useful, opens up
  where you need to think.
- **Mental model:** it should feel like a printed cartridge for the next two
  weeks. Pick it up, run it, swap it.

---

## 2. Design Tokens

### Palette — warm console (default)

| Token            | Hex       | Use                                |
|------------------|-----------|------------------------------------|
| `--bg`           | `#ede6d3` | page background                    |
| `--bg-panel`     | `#e3dac3` | panels, masthead, beacon bg        |
| `--bg-panel-2`   | `#d9cfb4` | nested panels                      |
| `--ink`          | `#1a1f1d` | primary text, dark strips          |
| `--ink-soft`     | `#3c4541` | secondary text, body in panels     |
| `--ink-mute`     | `#6a6f69` | UI labels, muted meta              |
| `--rust`         | `#c65a2e` | accent / structure / urgency       |
| `--rust-deep`    | `#a04620` | accent border depth                |
| `--teal`         | `#2d8e8a` | data-positive, "in motion"         |
| `--teal-deep`    | `#1f6562` | data-positive deep                 |
| `--butter`       | `#b8882e` | warning, waiting-on-other          |
| `--butter-soft`  | `#d4a854` | strategic / pending decision       |
| `--rule`         | rgba 18%  | hairline rules                     |
| `--rule-strong`  | rgba 32%  | structural rules / panel borders   |
| `--rule-faint`   | rgba 8%   | table row separators               |

A `data-theme="deep"` palette is reserved (not yet wired into this deck).

### Typography

Three faces, each does one job. Don't mix.

- **Major Mono Display** — display headlines. Always lowercase.
  Used for: `.cover-h1`, `.h-display`, `.plan-h1`, `.mast-title`, `.sigil`.
- **Space Mono** — UI labels, metadata, table data. Always uppercase, wide
  tracking (`.18em`–`.32em`).
- **DM Sans** — body copy, lede, panel content, table cell text.

Fallback chain keeps Fraunces / JetBrains Mono available for legacy elements.

### Shape language

- **Rust accents.** Underlines, separators, side bars on important panels,
  corner brackets on all data cards.
- **Corner brackets.** 10×10 px L-corners on every card-shaped element
  (gauges, panels, lab cards). Top-left + bottom-right only — diagonal
  emphasis, not boxed in.
- **Border rule.** 1.5px rust on accents, 1px `--rule-strong` on neutral
  containers, 1px dotted `--rule` for in-card separators.
- **Scanline overlay.** Fixed, 1px repeating, `~3.5%` opacity. Multiplied.
  Whisper-level CRT. Don't crank it up.
- **Lowercase displays + uppercase mono.** This contrast is the signature.
  Don't break it.

---

## 3. Component Library

### Eye label (`.eye`)

```
[rust 22×1 tick] §0X / TRANSMISSION 0XX / SECTION TITLE
```

Mono, 12px, 0.32em tracking, uppercase, ink-mute. The tick is rust 22×1px.
Use at the top of every non-cover slide.

### Display heading (`.h-display`, `.plan-h1`, `.cover-h1`)

Major Mono Display, lowercase, weight 400, letter-spacing `.02em`.
Sizes: cover `clamp(42, 6.4vw, 88)`, plan `clamp(40, 5.4vw, 72)`,
section `clamp(36, 5.2vw, 64)`. Use `<em>` for the rust word — keeps
it inline, not italic.

### Sigil (`.sigil`)

38×38 circle, 1.5px ink border, inner 1px ring, single Major Mono letter.
The mark of the brief.

### Mast-stencil (`.mast-stencil`)

Mono pill, bordered, uppercase. Slash separators `/` are rust and bold.
Format: `WB.0XX / SECTION / REV. YYYY-MM-DD`.

### Strapline (`.strapline`)

Eye label + display headline + one-line lede with `<b>` words underlined
dotted in rust. Used only on the cover.

### Gauge (`.gauge`)

Bracketed corner card. 4-up grid on the cover.

```
LABEL (mono caps)
VALUE (mono 36px tabular)         ▮▮▮▮▮▯ (6-bar arc)
SUB (mono caps)
```

### State pill (`.state`)

A 5-state taxonomy for plans + a 4-state taxonomy for lab projects.
**Plans:**

| Class      | Visual          | Means                                |
|------------|-----------------|--------------------------------------|
| `waiting`  | butter solid    | blocked on someone else (client/vendor) |
| `coverage` | rust solid      | needs hand-off — you're leaving      |
| `ramping`  | teal solid      | active, you driving                  |
| `open`     | ink outline     | on the board, no fire                |
| `debate`   | butter-soft     | strategic question pending           |

**Lab projects:**

| Class      | Visual          | Means                                |
|------------|-----------------|--------------------------------------|
| `flight`   | teal solid      | actively building                    |
| `scaffold` | butter outline  | shell exists, work in progress       |
| `idea`     | ink outline     | named, not started                   |
| `shipped`  | ink solid       | done, demo-able                      |

### Transmission (`.transmission`)

Rust-bordered panel with 4px rust left bar. The "finding worth raising" or
"the debate" or "first check before anything else". One per plan slide max.

### Panel (`.panel`)

Bracketed card with a `.panel-label` mono-caps header. The standard container
for thread lists, trigger sequences, anything structured.

### Thread list (`.thread-list`)

Stacked items separated by dotted rules. Each item has a `.thread-title`
(sans 600, prefixed with rust `▸`) and optional `.thread-body` (sans 14px,
ink-soft, indented 18px under the title).

### Trigger sequence (`.trigger-seq`)

Numbered ordered list, mono `01 02 03` in rust, arrow `→` in ink-mute,
sans body. Use for ordered next moves or trigger sequences.

### Off-plate strip (`.off-plate`)

Teal-left-bar footer strip on a panel-bg background. The exit condition
for the plan — when can you take it off your list.

### Hand-off strip (`.handoff`)

Dark ink background, rust label, white text. Used when coverage is needed
during an OOO window. Always carries a `Hand-off Deadline` date.

### Beacon (`.beacon`)

Used on the Triage Board only. Surfaces the cycle-spanning fact (currently:
the OOO window). Three-column grid: icon · body · meta.

### Console table (`.ledger`)

Mono cells, tabular numerals, ink-mute uppercase headers. Hover row tints
teal at 6% opacity. Use for the Active Ledger and any tabular plan view.

### Cycle grid (`.cycle-head` + `.cycle-grid`)

Three-column day ledger used on slide 10. Columns: `160px | 1fr | 1fr`
(date · work · lab). `.cycle-row { display: contents; }` so each row is
three independent cells sharing a single border baseline.

Date cell shows: `.cycle-day` (mono caps day), `.cycle-num` (Major Mono
display, tabular, rust `<em>` for the day number), optional `.cycle-tag`.

Row tags:

| Class                | Visual          | Means                        |
|----------------------|-----------------|------------------------------|
| `.cycle-tag.today`   | rust solid      | cursor · current day         |
| `.cycle-tag.handoff` | ink solid       | hand-off doc due this day    |
| `.cycle-tag.ooo`     | butter solid    | out-of-office block          |
| `.cycle-tag.cycle-end` | teal solid    | last day of the 14           |

Row-level emphasis: `.cycle-row.today` gets a rust inset bar on the date
cell and a rust wash on its work/lab cells. `.cycle-row.ooo-row` gets a
butter wash across all three cells. Items use `.cycle-list` (em-dash
bullets, rust dash); muted items use `<li class="muted">` for italic
ink-mute (coverage/OOO filler, continuation days).

### Live checklist (`.checklist-wrap`)

Slide 11. Structure:

```
checklist-meta   left: eye · h-display · lede
                 right: progress-block (big mono done/total + rust bar)
filter-bar       chips: all · urgent · [plans...] · lab  (with counts)
check-section × N  head (label + count) + check-items
checklist-footer cycle label · reset button
```

Each `.check-item` carries `data-id` (stable storage key), `data-plan`
(filter key), and optional `data-urgent="true"`. Meta slot uses
`.tag-plan` (ink caps), `.tag-urgent` (rust pill), `.tag-handoff`
(ink pill). State is persisted to `localStorage` under
`weekly-brief-checklist-<cycle-number>`.

---

## 4. Slide Templates

### Cover (slide 1)

```
[Masthead band]   sigil · mast-title · mast-stencil · folio
[Strapline band]  eye · theme-h1 · synthesis blurb (3–4 sentences) · gauges 2x2
[Operator strip]  4 cells: Operator · Role · Window · Nav
```

**Theme-of-the-cycle — new convention (cover only).** The cover's headline
(`cover-h1`) and the blurb underneath it are not decoration and they are
not a repeat of the lede pattern used elsewhere. Together they function as
the cycle's **title card** — the single frame that tells the reader what
this two-week stretch is actually about.

- **Theme headline (`cover-h1`).** A short lowercase phrase — 2 to 5
  words — that names the shape of the cycle as revealed by the Friday
  braindump. It is synthesis, not invention: pull it from what the
  operator actually said. The second word gets `<em>` (rust) the same
  way plan names do.

  Examples:
  - `the ooo <em>sandwich.</em>` — a cycle split by an out-of-office.
  - `hold the <em>ambiguity.</em>` — cycle dominated by plans waiting on
    unclear client data.
  - `ramp and <em>hand-off.</em>` — a cycle that starts active and ends
    in coverage.
  - `clean the <em>base.</em>` — cycle focused on tightening a single
    workstream before it scales.

  The theme changes every cycle. No permanent branding line — that's
  what the mast-title is for.

- **Synthesis blurb (cover-sub, 3–4 sentences).** One short paragraph
  under the headline that expands the theme. Written in the operator's
  voice. Three beats, in order:

  1. **Frame.** Name the cycle window + the one fact that shapes it
     (usually the OOO or a big landing date).
  2. **Where the energy is.** 1–2 sentences on the active front — which
     plans are actually moving, what the operator is pushing on, what
     will consume the week.
  3. **What to hold.** 1 sentence on what is deliberately parked —
     strategic debates, blocked plans, future-dated work. This is the
     signal that restraint is a choice, not an oversight.

  Use `<b>…</b>` on the plan names, dates, and key nouns — they render
  with the dotted-rust underline and act as anchors for the eye.

  Example blurb (if we wrote one for cycle 027 today):
  > Two weeks ending <b>5 / 06</b>, with the <b>OOO</b> splitting the
  > middle. The active front is <b>Haverhill</b> — assets land 5 / 01
  > and the hand-off has to hold through the gap. <b>Pella</b> is
  > ramping behind it with a new ADP rep in play. Everything else —
  > Callidi's client wait, the ISD deferral debate, the future-dated
  > caseload — stays parked by design.

  Length target: 55–80 words. Long enough to breathe, short enough that
  the gauges stay in frame on the cover.

- **Why this matters.** The cover is the first (and sometimes only)
  thing the operator sees when he opens the deck on Monday. If the
  theme headline and blurb are honest, the rest of the deck reads as
  evidence for a thesis. If they're generic, the deck reads as a list.

### Triage Board (slide 2)

```
[eye + h-display + lede]
[Coverage Beacon — only when an OOO window cuts the cycle]
[Active Ledger — table, one row per active plan, max ~7]
[Monitor strip — chips, one per back-burner plan]
[Closed line — single line]
```

### Plan Detail (slides 3–7) — The five-zone skeleton

```
1. plan-head      eye · plan-h1 · effective pill · meta strip (state · blocked · vital)
2. plan-lede      one-line takeaway, sans 19px
3. transmission   the finding / note / debate worth surfacing (optional but common)
4. panel-grid     left panel + right panel
                  pairs typically: open threads / trigger sequence
                                   done           / remaining
                                   why easier     / next moves
5. footer         off-plate (teal) OR handoff (dark, with deadline)
                  OR transmission below the panels (used when explainer is meaty)
```

Every plan slide MUST have: state pill, blocked-by, effective date, lede,
and an exit condition (off-plate or hand-off).

### Monitor (slide 8)

Console ledger of plans that are on the radar but not active this cycle.

```
[eye + h-display + lede]
[Monitor ledger]   PLAN · TYPE · EFFECTIVE · WATCH FOR
[Closed panel]     one-line strip of plans that wrapped
```

Type pills are mono caps: `pushed`, `prospective`, `pending`, `scheduled`.
Effective dates follow the same `M / DD` format as plan slides. The
`Watch For` column holds the one thing that would move the plan onto the
active ledger.

### The Lab (slide 9)

Reframed: not "show the boss". Personal track, no deadlines, ships when
ready. Cards in a 2-column grid. Each card carries:

- state pill (top-left)
- numbered slug (top-right)
- title (sans 600, 19px)
- pitch (sans 14px, ink-soft) — what it does in one breath
- **`↳ Derived from`** line — the real one-off it grew out of (or `Standalone`)
- deliverables chips (mono caps)

Project-derivation rule: if you find yourself building the same workaround
twice in different plans, it gets a card here.

### 14-Day Plan (slide 10)

Three-column cycle ledger with a `.cycle-head` band above `.cycle-grid`.
One row per relevant day (skip weekends unless intentional). Work items
sit in column 2, lab items in column 3 — both columns move every day.
Use `cycle-tag` chips (`today` / `hand-off` / `ooo` / `cycle-end`) to
mark the pivot days. Fill empty lab cells with muted continuation items
(`continue scaffold`) or an em-dash placeholder rather than leaving them
blank.

### Live Checklist (slide 11)

Flat list, filtered by plan. Structure:

1. `checklist-meta` — eye + h-display + lede on the left; mono
   done/total + rust progress bar on the right.
2. `filter-bar` — `all`, `urgent`, one chip per plan, `lab`. Each chip
   shows a live count.
3. `check-section × 2` — `work · caseload` and `the lab`.
4. `checklist-footer` — cycle label + `reset cycle` button (confirms
   before clearing).

Each `.check-item` has a stable `data-id`. State persists to
`localStorage` under `weekly-brief-checklist-<cycle-number>`, so ticking
an item survives reloads until the cycle resets.

---

## 5. Data Shape (braindump → JSON)

This is what the brief should compile from. Not yet implemented as a
generator — it's the target schema.

```json
{
  "cycle": {
    "number": "026",
    "window": { "start": "2026-04-22", "end": "2026-05-06" },
    "rev": "2026-04-22",
    "operator": "Joel",
    "role": "Senior Data Consultant",
    "ooo": [{ "start": "2026-04-27", "end": "2026-05-01" }]
  },
  "plans": [
    {
      "name": "Callidi Tas",
      "displayName": ["callidi", "tas"],
      "state": "waiting",
      "blockedBy": "Client · DOB / DOH",
      "effective": { "label": "Target Out By", "value": "EOW · 4 / 24" },
      "vital": { "label": "Loan flag", "value": "1 of 1 out of order" },
      "lede": "Blocked on client. Ready to pull the trigger the moment data lands.",
      "transmission": {
        "label": "Finding Worth Raising",
        "body": "..."
      },
      "panels": [
        { "label": "Open Threads",     "type": "threads",  "items": [...] },
        { "label": "Trigger Sequence", "type": "sequence", "items": [...] }
      ],
      "footer": {
        "type": "offPlate",
        "label": "Off Your Plate When",
        "text": "Client delivers data → 1–2 days. Target: end of week."
      }
    }
  ],
  "monitor": [
    { "name": "Prime Healthcare", "effective": "8 / 01", "note": "pushed" }
  ],
  "closed": [{ "name": "Episcopal" }],
  "labProjects": [
    {
      "name": "Deferral Ambiguity Analyzer",
      "state": "idea",
      "pitch": "...",
      "derivedFrom": "Pella SC · ADP deferral workaround",
      "deliverables": ["case study", "reusable", "client-facing"]
    }
  ],
  "days": [
    {
      "date": "2026-04-22",
      "day": "Wed",
      "tag": "today",
      "work": ["Nudge Callidi Tas client — DOB / DOH"],
      "lab":  ["Scaffold Plan Ops Dashboard"]
    }
  ],
  "checklist": [
    {
      "id": "cal-01",
      "section": "work",
      "plan": "callidi",
      "title": "Callidi Tas — nudge client for DOB / DOH",
      "urgent": true,
      "handoff": false
    },
    {
      "id": "lab-01",
      "section": "lab",
      "plan": "lab",
      "title": "Plan Ops Dashboard — scaffold list + detail",
      "meta": "№ 01 · in flight"
    }
  ]
}
```

### Enums

```
PlanState     = waiting | coverage | ramping | open | debate | closed
LabState      = flight | scaffold | idea | shipped
PanelType     = threads | sequence
FooterType    = offPlate | handoff | transmission
DayTrack      = work | lab
DayTag        = today | handoff | ooo | cycle-end | (none)
ChecklistFilter = all | urgent | lab | <plan-key>
```

---

## 6. Authoring Conventions

- **Cover theme + blurb** (slide 1 only): see §4 Cover. The theme headline
  is 2–5 lowercase words synthesized from the braindump; the blurb is a
  3–4 sentence (55–80 word) paragraph that names the cycle's shape,
  where the energy is, and what's deliberately parked.
- **Lede** (every other slide): one sentence, ≤ 18 words. The takeaway,
  not the summary.
- **Plan name in `<em>`:** the second word of the plan name gets `<em>` (rust).
  e.g. `callidi <em>tas</em>`, `pella <em>sc</em>`. One-word plans skip it.
- **Blocked-by is required.** If nothing is blocking, write `Self · [verify thing]`.
  No plan is unblocked — there's always a next move you owe.
- **Imperative "next moves."** Verbs first: "Email new ADP rep", "Build base file",
  "Confirm whether…". Never status-passive ("waiting on", "to be done").
- **Effective dates** display as `M / DD` (single-digit month, zero-pad day).
  Use `EOW`, `~ 2 wk`, `~ 2 weeks`, etc. for fuzzy dates.
- **Mono numerals are tabular.** Every numeric column gets `font-variant-numeric:
  tabular-nums;`.
- **Coverage Beacon only when there's an OOO** in the cycle window. No window,
  no beacon.
- **Plan keys are short + stable.** `callidi`, `haverhill`, `pella`, `ces`,
  `isd`, `lab`. These are the `data-plan` filter keys and the `id` prefixes
  on checklist items. Don't rename them mid-cycle — checklist state is
  keyed off them.
- **Checklist `data-id` is permanent.** Format: `<plan>-<NN>` (e.g.
  `cal-03`, `lab-04`). Never reuse an id for a different task — that's
  what the `localStorage` save reads back.
- **Cycle storage key is per-cycle.** `weekly-brief-checklist-<cycle-number>`,
  e.g. `weekly-brief-checklist-026`. Bumping the cycle number gives the
  next brief a clean slate without touching the previous one.
- **One muted continuation per empty lab day.** Don't leave a cycle-grid
  cell visually empty — write `continue scaffold` (italic, ink-mute) or
  an em-dash placeholder.

---

## 7. What's Implemented

- [x] Cover (slide 1) — masthead, strapline, gauges, operator strip
- [x] Triage Board (slide 2) — eye, beacon, active ledger, monitor chips, closed line
- [x] Plan Detail × 5 (slides 3–7) — Callidi, Haverhill, Pella SC, Cutting Edge, ISD
- [x] Monitor (slide 8) — console ledger + closed strip, retro-future
- [x] The Lab (slide 9) — 8-card grid, state pills, derived-from lines
- [x] 14-Day Plan (slide 10) — cycle-grid (date · work · lab) with day tags
- [x] Live Checklist (slide 11) — filter chips, progress bar, `localStorage`, reset
- [ ] Telemetry slide (§9) — registry-driven derivation panel
- [ ] Theme toggle (warm ↔ deep) in deck header
- [ ] JSON-driven build pipeline (schema in §5 is the target)
- [ ] Print stylesheet
- [ ] Cycle-number bump helper (auto-roll storage key on new brief)

---

## 8. Authoring a New Cycle (the braindump)

The brief compiles cleanly when the braindump hits a few specific beats.
You don't need a script — talk normally. Just make sure these things land
somewhere in the audio.

### Open with the frame (30 seconds)

Lead with the boring facts so the cover and beacon have what they need:

- **Cycle window.** "Two weeks starting Wednesday April 22, ending May 6."
- **Any OOO.** "I'm out April 27 through May 1." If none, say "no OOO."
- **The one fact that shapes everything.** The OOO usually is it. If not,
  say what is — a board meeting, a release, a client deadline.

Without these three the cover and Triage Beacon are guesswork.

### Optionally: name the theme (10 seconds)

If the cycle has a clear shape in your head, name it in one breath:

> "This cycle is really about holding the line while Haverhill lands."
> "This one's all about ambiguity — three plans waiting on client data."
> "It's a ramp-and-hand-off — start active, end in coverage."

That sentence becomes the cover's theme headline + blurb synthesis. If
you don't name it, the compiler will derive one from the rest of the dump
— but you naming it is always sharper than an inferred version. One line
is enough; don't over-polish.

### Walk the plans, one at a time

For each active plan, hit these beats in any order. Don't list them — just
make sure they're in there:

1. **Name + state in one breath.** "Callidi Tas, blocked on the client."
   The state words I listen for: *blocked on \_\_\_*, *needs hand-off*,
   *I'm driving*, *on the board*, *strategic question*.
2. **Who or what is blocking.** Always. If nothing is, say "I owe the
   next move — I need to verify X." There is no unblocked plan.
3. **The next move(s).** Imperative. "Email the ADP rep." "Build the
   base file." Not "I should probably…".
4. **The off-ramp.** When can this come off your plate? "Off my plate
   when client delivers." Or the hand-off: "Coverage owns 5/1 asset load."
5. **The one thing worth surfacing.** The finding, the workaround, the
   weird discovery. This becomes the transmission panel. Not every plan
   has one — only mention it if it actually exists.

If a plan is just "still waiting, nothing changed," say exactly that.
That's a one-line ledger row, not a slide.

### Call out the back-burner

After the active plans, sweep the radar:

- **Monitor items.** Plans you're not working but watching. Each one:
  name, what kind (`pushed`, `pending`, `prospective`, `scheduled`),
  rough effective date, and the one thing that would move it onto the
  active board.
- **Closed.** One line: "Episcopal closed." Done.

### Then the lab

Side-project time. Two flavors:

- **In flight / scaffolding / shipped this cycle.** What you did or
  plan to do.
- **New ideas — especially derived ones.** This is the bit to lean
  on. Whenever you say *"I built a workaround for this once"* or
  *"if I had a tool for this it'd take five minutes"*, **stop and
  name it.** That's a lab card. The Deferral Analyzer was born this
  way. Patterns I listen for: *"I keep doing this manually,"* *"I
  ended up writing a script,"* *"this is the third time I've…"*.

### Close with the cycle shape

Optional but useful — talk through the 14 days at a high level:

- "First week is mostly Callidi and the hand-off docs."
- "I'm out the second week."
- "When I get back, Pella deferral report should be in hand."

If you don't say it, I'll infer it from the plans, but a sentence or
two of "shape" makes the cycle grid much sharper.

### What to bang on

- **Blockers and next moves.** This is the spine. If I had to throw
  out half the braindump, I'd keep these.
- **Hand-offs and OOO coverage.** Single biggest source of regret if
  it's missing.
- **Derivation moments.** "I built a one-off for this" → lab card.
  Don't let these slide past.
- **What changed since last cycle.** New blocker, new contact,
  resolution, escalation. The delta is the news.

### What to skip

- **Long backstory.** I don't need the history of the plan, just where
  it stands now and what's next.
- **Status verbs.** "Waiting on", "pending", "to be done" — drop them.
  Tell me *who* is blocking and *what move you owe*.
- **Hedging.** "Maybe I should probably think about possibly…" — if
  it's a real next move, it goes on the deck; if it's not, skip it.
- **Calendar admin.** Standing meetings, recurring 1:1s — not in
  scope unless they actually shift a plan.

### Speed reference (per plan, ~45 seconds each)

> "Callidi Tas — blocked on the client for DOB and DOH on the missing
> participants. Next move is to nudge them this week. Off my plate when
> they deliver. The thing worth raising: the balance file came in
> incomplete, which is a client data-completeness issue we should flag.
> Hand-off doc going to coverage by Friday."

That one paragraph compiles into a full plan slide.

---

## 9. Telemetry — the auto-derived slide

A new slide (call it `telemetry.`) that reads the **plans registry** and
the braindump, then surfaces things the braindump *didn't* mention but
should have. It's the deck second-guessing the operator.

### Source: `plans-registry.csv`

A flat CSV next to the deck. You maintain it. One row per plan,
edit-as-things-change. Columns:

| column                | type   | notes                                    |
|-----------------------|--------|------------------------------------------|
| `key`                 | slug   | stable identifier, used as `data-plan`   |
| `case_number`         | string | official case number                     |
| `canonical_name`      | string | name on the case file                    |
| `display_name`        | string | what the deck shows (`Callidi Tas`, `ISD`) |
| `day1`                | date   | held for reference, **does not drive nudges** |
| `effective_date`      | date   | EFF anchor                               |
| `asset_transfer_date` | date   | WIRE anchor                              |
| `status`              | enum   | `active` \| `on_hold` \| `final` \| `future` |
| `scope`               | enum   | `full` \| `partial` (ISD-style limited scope) |
| `notes`               | string | free text                                |

Statuses guide telemetry behavior:

- **`active`** — full date logic + event-triggered nudges
- **`final`** — only flag wire-window items (Callidatas-style)
- **`future`** — only show in "future plans" panel; no due-date nudges
- **`on_hold`** — hidden from telemetry until status changes

### What drives nudges (the date-anchor rule)

- ✅ **EFF anchors** drive due-now / just-passed nudges. Reliable.
- ✅ **WIRE anchors** drive due-now / just-passed nudges. Reliable.
- ❌ **Day1 anchors do NOT drive nudges.** A plan can land on the desk
  six months in advance — Day1 → milestone math creates fake-late noise.
  Day1 stays in the registry for record-keeping only.

### What drives nudges (the event-trigger rule)

When the braindump mentions a phase event, telemetry walks the
conversion sequence and surfaces *the next thing* — not by date, by
position in the chain.

**Conversion sequence (the chain):**

```
plan documents received
  → PRD / onboarding package complete
  → PRD sent to testing
  → testing results back
  → contacts confirmed · TOA obtained
  → templates complete
  → templates sent (client + payroll)
  → meetings scheduled
  → base data returned · payroll specs returned
  → wire instructions sent · transfer date confirmed
  → fund mapping done · re-reg funds identified
  → test files received from prior RK
  → source mapping complete
  → FTP contact identified · FTP setup
  → file specs delivered to payroll
  → test payroll file received
  → EDS validation
  → OnePayroll submit · approval
  → kickoff meeting
  → [EFF–21] initial loads start
  → [EFF–14] full loads complete
  → [EFF–7]  Roth elections confirmed
  → [EFF–3]  final verify
  → [WIRE–5] advance vendor notice
  → [WIRE–3] advance Cashiering notice
  → [WIRE]   wire day
  → [WIRE+7] final files received
  → [WIRE+14] post-wire processing complete
  → [EFF+1]  first live payroll
  → [EFF+3]  payroll reconciled · client notified plan is live
  → [EFF+5]  outstanding items documented
  → [EFF+7]  30-day review scheduled
```

**Trigger map (sample — what mentions surface what nudges):**

| You mention…                        | Telemetry asks…                                 |
|-------------------------------------|--------------------------------------------------|
| "got the PRD back from testing"     | Contacts confirmed? TOA obtained?                |
| "templates went out"                | Base data + payroll specs SLA is ~14d. Tracked? |
| "meeting with the client" (early)   | Did you get a base file yet?                     |
| "test files came in from prior RK"  | Source mapping started? Balance prep queued?     |
| "FTP contact identified"            | File specs to payroll? Test payroll requested?   |
| "test payroll received"             | EDS validation? OnePayroll submit?               |
| "EDS validation done"               | OnePayroll submitted? Approval expected when?    |
| "OnePayroll approved"               | Kickoff meeting on the books?                    |
| (silent on a plan with EFF in 30d)  | EFF–21 / –14 / –7 milestones flagged as due      |

### Slide layout

`telemetry.` is structured as up to 5 stacked panels — only render those
that have content. Each panel is a `.panel` with a mono caps label.

```
[eye + h-display + lede]

PANEL · DUE THIS CYCLE
  ↳ rows: <plan> · <milestone> · <date> · <days from today>
  Sourced from EFF/WIRE math + cycle window.

PANEL · JUST PASSED — CONFIRM?
  ↳ rows: milestones in the trailing 14d that should be done.
  Anchored on EFF/WIRE only.

PANEL · OOO COLLISIONS
  ↳ rows: critical milestones falling inside the OOO window.
  Format: <plan> · <milestone> · <date> · severity (rust/butter chip).

PANEL · TRIGGER NUDGES
  ↳ rows: derived from the braindump → conversion-sequence walk.
  Format: <plan> · "you said: …" → "did you: …?"

PANEL · ANCHOR GAPS / SILENT PLANS
  ↳ rows: registered active plans not mentioned this cycle, OR
          plans missing EFF/WIRE, OR new plans without a brief entry.
```

### Non-goals (deliberately not in telemetry)

- No day1-anchored "X days late" warnings. Always wrong, always noisy.
- No nudges on `future` or `on_hold` plans — only `active` and `final`.
- No nudges for tasks outside the plan's `scope`. ISD's `partial` scope
  skips payroll/wire chain entirely.

### Authoring the registry

- One source of truth. Edit when a date shifts, status changes, or a
  new plan lands. ~5 minutes per cycle.
- `key` is permanent. It's the bridge between deck `data-plan`,
  checklist `data-id` prefix, and registry row. Don't rename.
- When a plan finishes, set `status = final` for one cycle (so wire-
  window telemetry still fires), then archive the row by moving it to
  a `plans-registry-archive.csv` next cycle.

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
2. Classify each chunk into: plan / project / profile / finding /
   win / loss / gap. (Wins + losses only when the operator
   **explicitly** declares them — never inferred.)
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
