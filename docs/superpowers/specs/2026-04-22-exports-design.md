# Exports — Design Spec

A family of on-demand HTML dumps compiled from the same markdown brain
that feeds the weekly brief. Each dump is a single-page view cut for a
specific purpose, audience, and time window.

Companion to `weekly-brief-spec.md` and
`docs/superpowers/specs/2026-04-22-persistent-brain-design.md`. This spec
does not change the brief or the intake loop.

---

## 1. Purpose

The weekly brief is one view of the brain. It serves the operator at the
start of a cycle. Other moments want other views:

- A clean snapshot of the caseload to hand a manager, or to look at it
  without the cycle chrome.
- A portfolio view of side-projects — no deadlines, no cycle, just the
  board.
- A deep read on a single project when it needs attention or a demo.
- A coverage pack tuned to an out-of-office window — what someone else
  needs to know to keep things from rotting while the operator is gone.

All four ride on the same markdown files in `caseload/`, `projects/`,
and `profile/`. No new data. New views.

Goals:

- Reuse the weekly brief design system verbatim. Zero new components,
  zero new tokens.
- Keep the artifacts simple: one HTML file per dump, single scrolling
  page, no pagination, no build pipeline.
- Make each dump legible on its own — shareable, printable, openable on
  a phone.

Non-goals:

- No generator script. The operator asks in chat; Claude compiles the
  file by reading the markdown set.
- No templating engine. Each dump is written directly as HTML, styled
  with the existing stylesheet.
- No new design language. If it's not already in the brief, it doesn't
  go in a dump.
- No recurring / scheduled exports. These are on-demand.

---

## 2. Scope — Four Outputs

| Key         | Filename pattern                                   | What it is                              |
|-------------|----------------------------------------------------|-----------------------------------------|
| `caseload`  | `exports/caseload-YYYY-MM-DD.html`                 | All-plans snapshot                      |
| `projects`  | `exports/projects-YYYY-MM-DD.html`                 | All-projects portfolio                  |
| `project`   | `exports/project-<key>-YYYY-MM-DD.html`            | Single-project deep dive                |
| `handoff`   | `exports/handoff-YYYY-MM-DD-to-YYYY-MM-DD.html`    | OOO coverage pack, scoped to a window   |

Each filename is self-describing and sortable. The date segment is
always the date the dump was compiled — a dump is a snapshot, so the
date is the artifact's identity.

Future candidates not in this spec: single-plan dump (trivial extension
of `project` pattern), end-of-week wrap-up, profile/identity export.

---

## 3. Folder Layout

```
Weekly Digest/
├─ caseload/                 unchanged — source for caseload + handoff
├─ projects/                 unchanged — source for projects + project
├─ profile/                  unchanged — source for handoff
├─ plans-registry.csv        unchanged — source for handoff window scoping
├─ weekly-brief-spec.md      unchanged
├─ caseload-deck-NNN.html    unchanged — shares CSS with exports
└─ exports/                  NEW — all dump outputs land here
   ├─ caseload-YYYY-MM-DD.html
   ├─ projects-YYYY-MM-DD.html
   ├─ project-<key>-YYYY-MM-DD.html
   └─ handoff-YYYY-MM-DD-to-YYYY-MM-DD.html
```

The `exports/` folder is append-only in practice. Old dumps are kept —
they're snapshots, they age on purpose. Manual cleanup when it gets
noisy. Don't overwrite a dump from the same day; append a suffix
(`-v2`) if a re-compile is needed.

---

## 4. Design System Reuse

All four dumps use the stylesheet and component vocabulary defined in
`weekly-brief-spec.md §2-3`. No new tokens, no new classes.

Components used as-is:

- `eye` (section ticks)
- `h-display`, `plan-h1` (display headings, lowercase Major Mono)
- `sigil`, `mast-stencil` (masthead furniture)
- `panel`, `panel-label` (standard container)
- `state` pills — plans palette for caseload/handoff, lab palette for
  projects
- `transmission` (rust-bordered finding/note panel)
- `ledger` (console table)
- `thread-list`, `trigger-seq`
- `off-plate`, `handoff` (footer strips)
- Corner brackets, scanline overlay, rust accents

Masthead variation per dump type (same language, different label):

| Dump       | Mast-stencil format                          |
|------------|----------------------------------------------|
| caseload   | `CASELOAD / SNAPSHOT / REV. YYYY-MM-DD`      |
| projects   | `PROJECTS / PORTFOLIO / REV. YYYY-MM-DD`     |
| project    | `PROJECT / <KEY> / REV. YYYY-MM-DD`          |
| handoff    | `HANDOFF / <START>–<END> / REV. YYYY-MM-DD`  |

No cycle number. Exports are not cycle-scoped — they have a compile
date, not a cycle id.

---

## 5. Per-Output Spec

### 5.1 Caseload Snapshot (`caseload`)

**Source:** every file in `caseload/` whose frontmatter `status` is
`active`, `waiting`, or `final`. Skip `future` and `on_hold`.

**History depth:** today only. No dated history rendered.

**Layout (top to bottom):**

```
Masthead          sigil · title · mast-stencil · compile date
Eye + heading     § 00 / SNAPSHOT / CASELOAD
                  h-display: "caseload as of <date>."
                  lede: one-line count — e.g.
                        "<N> active · <N> waiting · <N> final."
Active Ledger     console table, one row per plan:
                  PLAN · STATE · BLOCKED BY · NEXT MOVE · OFF-RAMP
                  Mono headers, sans body. Tabular numerals.
Plan Sections     stacked, one per plan, in ledger order:
                  - eye: § 0N / SNAPSHOT / <PLAN KEY>
                  - plan-h1 (lowercase, second word in <em> rust)
                  - state pill · blocked-by · effective date meta strip
                  - lede (one sentence from MD)
                  - transmission (if the MD has a current finding)
                  - panel-grid: Open Threads + Next Moves
                  - footer: off-plate OR handoff strip
Footer strip      compile date · operator name · source note
                  ("compiled from caseload/ · persistent brain")
```

**Rules:**

- No cycle window, no OOO beacon, no 14-day plan, no checklist, no
  monitor strip. A snapshot is timeless structurally.
- Plan order in ledger and sections: `active` first (by urgency the
  operator calls out), then `waiting`, then `final`.
- If a plan has no current transmission or finding in the MD, omit the
  transmission — don't invent one.

### 5.2 Projects Portfolio (`projects`)

**Source:** every file in `projects/`. All statuses included.

**History depth:** today only.

**Layout:**

```
Masthead          sigil · title · mast-stencil · compile date
Eye + heading     § 00 / PORTFOLIO / PROJECTS
                  h-display: "seven side tracks."
                  lede: one line — what this is.
State Counters    mono strip:
                  FLIGHT · N | READY-TO-SHIP · N | SCAFFOLD · N
                  | IDEA · N | SHIPPED-FOLDED · N
Project Cards     grouped by state, ordered:
                  flight → ready-to-ship → scaffold → idea →
                  shipped-folded
                  Each card (corner-bracketed, panel-bg):
                    - state pill top-left
                    - № 0N top-right
                    - title (sans 600, 19px)
                    - pitch (sans 14px, ink-soft) — one-line
                    - ↳ derived from: <source> (or "standalone")
                    - current state: 2-4 bullets from latest dated
                      entry in the MD
                    - deliverables chips (mono caps)
                    - open questions (only if present in MD)
Footer strip      compile date · project count · source note
```

**Rules:**

- Cards are the same visual language as The Lab slide (brief spec §4),
  but content-expanded per card and grouped by state into sections
  rather than a flat 2-column grid.
- Today-only depth — no dated sub-heads rendered. Seven projects with
  arcs would bloat.
- Deliverables chips pull from whatever the MD file lists under
  `deliverables:` or inline.

### 5.3 Single Project (`project`)

**Source:** one file — `projects/<key>.md`. The `<key>` argument is
required.

**History depth:** today + short arc. This is the deep one.

**Layout:**

```
Masthead          sigil · title · mast-stencil (PROJECT / <KEY>)
Eye + heading     § 00 / PROJECT / <KEY>
                  h-display: project display name (lowercase)
                  lede: one-line pitch from MD
Identity Strip    state pill · last mentioned · first logged ·
                  derived from
Current State     panel. 3-5 bullets summarized from the most recent
                  dated entries. Written as a short arc — "where it
                  stands today, and the 1-2 moves that got it here."
Deliverables      chips strip (mono caps)
Open Questions    panel — bullet list. Only if present in MD.
                  Imperative phrasing.
Next Moves        panel — trigger-seq format (01 → 02 → 03).
                  Only if present in MD.
History           panel-bg panel, timeline format. Most-recent first.
                  Each dated sub-head rendered as:
                    ### 2026-04-22
                    - collapsed to ~2 lines (first 1-2 bullets of that
                      cycle's note, or a 1-sentence summary)
                  Full bullet lists NOT rendered — the MD is the
                  source of truth for detail.
Footer strip      compile date · source file · source note
```

**Rules:**

- The `Current State` panel is the one section Claude writes rather
  than quoting. 3-5 bullets, takeaway-first, not a rewrite of the MD.
- `History` panel is compact: one collapsed row per dated sub-head,
  most-recent first. Someone reading the dump can follow the arc
  without reading the full MD.
- If the project has only one dated entry, omit the History panel.

### 5.4 OOO Hand-Off Pack (`handoff`)

**Source:** multiple.

- `profile/identity.md` → operator identity, how to reach, working
  style.
- `caseload/*.md` → plan state for plans in-window.
- `plans-registry.csv` → EFF/WIRE dates for in-window scoping.
- Chat input → window dates, coverage person, coverage scope, any
  operator-flagged plans.

**Scoping (which plans go in):**

Auto-proposed by Claude, confirmed by the operator before compile.

A plan is proposed for inclusion when any of these are true:

- Registry row has EFF or WIRE date inside the window.
- MD frontmatter `tags` include `coverage-needed` (per the tag
  conventions in `2026-04-22-persistent-brain-design.md §3`).
- Operator named the plan in chat when requesting the handoff.

Out-of-scope plans (active plans NOT expected to need coverage) are
listed in a short "not your problem" strip so coverage knows what's
being intentionally left alone.

**History depth:** today only for most plans; short arc (level B) for
the 1-2 plans that actively need action in the window.

**Layout:**

```
Slide 1 · Title   sigil · cover-h1 "coverage pack." · mast-stencil
                  (HANDOFF / <START>–<END> / REV. <compile date>)
                  operator-out line, coverage name, window.
                  Roster / TOC: one row per in-window plan with
                  clickable #plan-<key> anchor, state pill, one-line
                  "what might land" block.
                  Out-of-scope strip below roster.
                  Operator strip at bottom (operator · role · window
                  · coverage).

Slide 2 · Brief   ALWAYS PAGE 2 — the FYI / read-me-first page.
                  Structure is canonical and lives in the skill asset
                  .claude/skills/weekly-digest/assets/brief-slide.html
                  (reference implementation:
                  exports/MOCK-handoff-front-and-brief-2026-04-22.html).

                  Two-column:
                    · Left (1.35fr): rust-bordered transmission panel
                      titled "Briefing · read me first." Natural-
                      language body in the operator's voice — one
                      paragraph per major plan that needs coverage
                      action, plus a sign-off.
                    · Right (1fr): bracketed card titled "Directives ·
                      do · hold · don't." 6-10 rows, each with a three-
                      state mark (✓ teal do, ◌ butter hold, ✕ rust
                      don't) and a .k plan-key chip.

                  Footer strip declares the brief overrides plan
                  sections on conflict.

Plans In Window   stacked, one section per in-scope plan:
                    - eye: § 0N / PLAN / <KEY>
                    - plan-h1 + state pill + blocked-by
                    - lede: "what might land this window"
                    - panel-grid:
                        If this happens → Do this
                        Client contacts → Notes on each
                    - handoff strip (dark, with hand-off deadline if
                      applicable, and current contact for the client)
                    - (for 1-2 priority plans) transmission or short-
                      arc panel: "here's how we got here"
Out Of Scope      strip (ledger-style, one line per plan):
                    PLAN · STATE · NOTE ("not coverage — on hold")
Close             panel — "WHEN I'M BACK"
                    - what to hand back
                    - what to flag if it happened
                    - one-line sign-off
Footer strip      compile date · window · source note
```

**Rules:**

- Tone shift: coverage is the audience, not the operator. Plan sections
  are written in "if X happens, do Y" directive form — not
  self-reminders.
- Client contact info included per plan (pulled from MD if present;
  operator adds in chat if not).
- No cycle window, no 14-day plan. The window is the window.
- No checklist — coverage isn't a cycle of accountability, it's a
  standing order set.

---

## 6. Data Sources & History Depth

Summary table across the four outputs:

| Dump      | Source files                               | History depth            |
|-----------|--------------------------------------------|--------------------------|
| caseload  | `caseload/*.md` (active/waiting/final)     | today only               |
| projects  | `projects/*.md` (all statuses)             | today only               |
| project   | `projects/<key>.md`                        | today + short arc        |
| handoff   | `caseload/*`, `profile/identity.md`, registry | today (+ arc for 1-2) |

History depth vocabulary (shared with brief intake):

- **today only** — latest dated entry's takeaway, state, next moves.
  No timeline rendered.
- **short arc** — today's state plus a 2-3 line "how we got here"
  summary Claude writes from the dated sub-heads.
- **full history** — every dated sub-head verbatim. Not used in any
  dump in this spec.

---

## 7. Workflow — How a Dump Gets Made

1. **Operator asks in chat.** Forms:
   - *"Give me a caseload snapshot."*
   - *"Full project dump."* or *"Projects portfolio."*
   - *"Project dump for `ta-brain`."*
   - *"Handoff pack for April 27 to May 1, Sarah covering."*
2. **Claude reads the source set.** For single-entity dumps, one file.
   For aggregates, the relevant directory. For handoff, registry +
   identity + filtered caseload.
3. **Claude proposes (handoff only).** Before compiling the handoff,
   Claude lists the plans it intends to include based on scoping rules
   in §5.4. Operator confirms or edits.
4. **Claude writes the HTML file** directly to `exports/<filename>`.
   Uses the weekly brief stylesheet (linked or inlined — match the
   existing deck's approach).
5. **Operator opens the file.** Either via `file://` or through the
   local static server (`.claude/launch.json` config `deck-preview`
   runs `python -m http.server 4173` from the project root).

No script. No queue. No scheduler. The brief pattern is a good one and
it carries here.

---

## 8. Authoring Conventions

- **Filename date is compile date.** Not the operator's intent date.
  A caseload snapshot compiled on April 22 is always
  `caseload-2026-04-22.html`.
- **Keys stay stable.** Same keys used in caseload/projects files,
  registry, deck. `exports/project-ta-brain-...` matches
  `projects/ta-brain.md`.
- **Lowercase-hyphen filenames.** Always. No spaces, no caps.
- **Same-day re-compile uses `-v2`.** `caseload-2026-04-22-v2.html`.
  Don't overwrite.
- **No invented content.** If the MD doesn't have a transmission /
  open question / derived-from, don't fabricate one to fill a slot.
  Omit the slot.
- **Current state bullets are summarized, not quoted.** In the
  `Current State` panel of the project dump (§5.3), Claude writes the
  bullets — takeaway-first, imperative, not a rewrite of the MD. For
  every other bullet slot across all dumps, quote from the MD.
- **Handoff plan sections are directive.** "If Pella returns the
  deferral report, forward to the client and flag Joel." Not "Pella
  is waiting on the deferral report."
- **Same CSS as the deck.** Either link the existing stylesheet or
  inline the needed subset — match whatever approach `caseload-deck-
  NNN.html` is using at compile time.

---

## 9. Not In Scope

- Single-plan dump (caseload/<key>). Trivial follow-on; same pattern
  as `project` dump applied to a caseload file. Add when needed.
- End-of-week wrap-up (sibling to the brief, retrospective). Separate
  spec.
- Profile/identity export. Separate spec.
- Generator script. Manual compile is the pattern.
- Print stylesheet. Dumps render fine in browsers; print can come
  later if needed.
- Scheduled or recurring exports. On-demand only.
- Dark-theme variant of the palette. Tracked under brief spec §7.

---

## 10. Glossary

- **Dump / export / snapshot** — used interchangeably. An HTML file in
  `exports/` compiled from the markdown brain at a point in time.
- **Window** — the start+end date pair scoping a handoff pack.
- **Short arc** — a 2-3 line summary of how an entity got to its
  current state, written by Claude from the dated sub-heads in the
  MD file.
- **In-window plan** — for handoff scoping, a plan with EFF/WIRE date
  inside the window, OR flagged `coverage` / `coverage-needed`, OR
  named by the operator in chat.
- **Compile date** — the date the dump was generated. Appears in the
  filename and the mast-stencil.
