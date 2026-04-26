---
name: weekly-digest
description: Use whenever the operator is working inside the Weekly Digest folder and drops a braindump, asks to update a plan/project, requests a weekly brief, or asks for a snapshot/handoff/out-of-office export. Routes voice braindumps into the persistent brain (profile/caseload/projects), flags pending items, cross-checks for contradictions, and compiles the weekly brief or on-demand HTML exports. Trigger on phrases like "new brain dump", "process this dump", "ingest", "update Callidi", "compile the brief", "Monday brief", "snapshot", "caseload snapshot", "handoff pack", "out of office export", "I'm going to be out", or any time raw transcript text shows up near the `braindumps/` folder.
---

# Weekly Digest — Brain Intake + Export Skill

The operator (Joel) does voice braindumps between calls. This folder is his
persistent brain: markdown notes per plan, per project, plus an identity file.
The weekly brief and on-demand exports (snapshot, handoff pack) compile from
those markdown files. This skill is the intake loop and the compile loop.

**The prime directive: keep track of everything.** Nothing the operator says
explicitly gets dropped. Everything either lands in a dated entry, an open-item
flag, a registry row, or a contradiction report. No silent loss.

## Canonical references (read when needed)

These specs already exist in this folder and are authoritative. Don't duplicate
their content — read them when you need the full detail:

- `weekly-brief-spec.md` — deck design system, slide templates, brief flow
- `docs/superpowers/specs/2026-04-22-persistent-brain-design.md` — folder
  layout, file shape, intake flow, `resurface.` slide
- `docs/superpowers/specs/2026-04-22-exports-design.md` — the four on-demand
  export types (caseload / projects / project / handoff)
- `plans-registry.csv` — one row per plan; drives telemetry and date math
- `profile/identity.md` — who Joel is, how to talk to him, honest observations

When this skill and a spec disagree, **the specs win** for anything about file
shape, slide templates, or design tokens. This skill adds the intake policies
(pending flags, contradiction checks, briefing overrides) that sit on top.

## Folder map (source of truth)

```
Weekly Digest/
├─ profile/identity.md             single file, natural language
├─ caseload/<plan-key>.md          one per plan
├─ projects/<project-key>.md       one per side-project
├─ braindumps/YYYY-MM-DD-<topic>.txt   raw transcripts, read-only
├─ contradictions/YYYY-MM-DD.md    intake contradiction reports (this skill)
├─ exports/                        handoff/snapshot HTML dumps
├─ plans-registry.csv              plan dates + status
├─ weekly-brief-spec.md            brief design spec
├─ docs/superpowers/specs/         design specs
└─ caseload-deck-NNN.html          weekly brief output (also exports/)
```

Keys (`callidi`, `ta-brain`, etc.) are permanent. They're the bridge across
filename, registry row, deck `data-plan`, and checklist `data-id` prefix.
**Never rename a key mid-cycle** — it silently breaks checklist `localStorage`.

## How to route a prompt

Read the prompt. Decide which of these is happening — usually one, sometimes
the operator asks for both in one breath ("process this dump and give me the
brief"):

| Prompt looks like…                              | Do the…                      |
|-------------------------------------------------|------------------------------|
| raw transcript pasted, or "process this dump"   | **Intake flow**              |
| "update `<plan>`" with specific facts           | **Intake flow** (targeted)   |
| "compile the brief" / "Monday brief" / new cycle| **Weekly brief compile**     |
| "caseload snapshot" / "full snapshot"           | **Export flow** (caseload)   |
| "projects portfolio" / "all projects"           | **Export flow** (projects)   |
| "project dump for `<key>`"                      | **Export flow** (project)    |
| "handoff pack" / "out of office" / "I'm out …"  | **Export flow** (handoff)    |
| "plan extract" / "current plan extract" / "where things stand" / "one-column extract" | **Export flow** (plan-extract) |

Ambiguous? Ask once, default to the conservative read (leave files alone
rather than guess). Joel wants imperatives, not hedging — if it's clear,
just execute.

## Intake flow (the heart of the skill)

Every braindump follows the same six steps. Do them in order; don't skip.

### 1. Archive the raw dump, verbatim

Save the exact text to `braindumps/YYYY-MM-DD-<topic>.txt`. Today's date is
the compile date. Topic is a short slug Joel can recognize later
(`caseload-update`, `projects-status`, `cycle-close`, `intake-redesign`).
Never edit this file after the initial save — it's the forensic record.

### 2. Classify each chunk

Walk the dump chunk by chunk (a chunk is one topic, not one sentence). Route
each to exactly one of:

- **Plan talk** → `caseload/<key>.md`
- **Project talk** → `projects/<key>.md`
- **Identity / goal / working-style / preference** → `profile/identity.md`
- **Gap** — nothing to route; note it for the `resurface.` slide

If a chunk could fit two plans, pick the more specific one and add a single
cross-reference line (`- see also: <other-key>`) in the other file.

### 3. Contradiction check — BEFORE writing to MDs

This is the "linter" pass. For each chunk that lands on an existing file,
read the current frontmatter + the most recent dated entry and ask: does
anything in the new chunk contradict anything already on file?

Look for:

- **Dates that shifted.** "liquidation was March 1st" when the file or
  registry says March 5th.
- **Status mismatches.** Dump implies `ramping`, file says `waiting`.
- **Numeric values** (dollar amounts, participant counts, loan counts).
- **Named parties** (contacts, vendors, covering person).
- **Binary decisions.** "We killed that idea" vs. file still shows active.

For each contradiction, don't guess which is right. Write it to a report:

**File:** `contradictions/YYYY-MM-DD.md` (create if missing; append if
already today).

**Format:**

```markdown
## <plan-or-project-key> — <short label>

- **Old (on file):** <quote from MD + path>
- **New (this dump):** <quote from transcript>
- **Question:** <the specific thing to confirm>
- **Proposed resolution:** <conservative default — usually "hold until confirmed">
```

Surface the report in the reply back to the operator. He decides which
version is truth. Until he confirms, **do not overwrite** the existing MD
with the new value — append the new entry with the contradiction flagged
inline like this:

```markdown
- ⚠ CONTRADICTS FILE: you said <X>, file shows <Y> — see contradictions/YYYY-MM-DD.md
```

Silent correction is the failure mode this guards against. Joel explicitly
asked: garbage-in check happens before garbage-out.

If there are zero contradictions, don't create the file. A clean intake is
a clean intake.

### 4. Update each touched file

For every file you're touching:

1. **Bump `last_mentioned`** in frontmatter to today's date.
2. **Update `status`** in frontmatter if the dump clearly shifted it.
   Also drop a one-liner in the dated body: `- status: <old> → <new>
   (<why>).`
3. **Append a new `### YYYY-MM-DD` sub-heading** with today's content.
   Never rewrite old dated sub-heads. Ever. The history IS the structure.
4. **Update the `### Open Items` section** at the bottom of the file (see
   §5 — the pending-flag rule).
5. For plans: **update `plans-registry.csv`** if any of `status`,
   `effective_date`, `asset_transfer_date`, or `scope` moved. Registry is
   authoritative for date math and telemetry; keep it honest.

Authoring rules for the new entry:

- **Imperative next moves.** "Email the new ADP rep." Not "waiting on."
- **Blocked-by is required on plan updates.** If nothing is blocking, write
  `Self · verify <X>`. There's always a next move owed.
- **Off-ramp on plan updates.** "Off my plate when X happens."
- **Transmission-worthy beats.** If the operator said something that
  belongs on a plan's transmission panel (a finding, a gotcha, a
  data-completeness issue), mark it `- transmission-worthy: <text>`.
- **Derivation moments.** If the operator said "I keep doing this
  manually" or "I built a workaround for this once" → this is a lab card
  candidate. Flag it `- lab-candidate: <one-line pitch>` in the project
  file (or in `profile/identity.md` if it isn't tied to an existing project).

### 5. Pending flag — the thing Joel asked for by name

Any item the operator **explicitly** calls out as still open, still needed,
still pending, or "I need to remember to…" gets tracked in an `### Open
Items` section at the bottom of the file. This is separate from the
next-moves of a single dated entry — it's the persistent todo list for
that entity.

**Rules for open items:**

- One line per item, imperative voice. "Nudge client for DOB + DOH."
- Status prefix — pick one per line:
  - `- [ ]` — still open (default for anything Joel explicitly named)
  - `- [x] (YYYY-MM-DD)` — resolved this cycle; keep it for one cycle then
    drop
  - `- [~] <blocker>` — blocked (cite what's blocking)
- When a new dump **resolves** an item already on the list, tick it to
  `[x]` with today's date. Don't delete it on the same dump — Joel wants
  to see what closed out.
- When a new dump **explicitly adds** a pending item (e.g. "make sure I
  remember to confirm the wire window"), add it as `[ ]` even if it's
  tiny.
- When a new dump is silent on an open item, leave it as-is. Silence
  doesn't close an item.
- Don't invent open items from implied work. Only things Joel actually
  called out. Inferred follow-ups go in the dated entry as next-moves.

These open items feed the `resurface.` slide (the "signal / FYI" slide) and
the live checklist. They're the durable flags across cycles.

### 6. Detect gaps and suggestions

After all files are written, note:

- **Silent plans** (active plans not mentioned this cycle) → feeds
  `resurface.` "Haven't Heard From" panel.
- **Name-drops with no next move** → feeds "You Mentioned, I Didn't Catch."
- **Cross-project patterns** (something in plan A that resembles what Joel
  just said about project B) → feeds "Worth a Look," max 3 per cycle.

Don't write these into the MDs. Carry them in memory for the compile step
(§below) or surface them in the reply if the operator isn't compiling
anything right now.

### Intake reply template

When the intake finishes, reply to the operator with a short summary so he
knows what moved. Keep it tight:

```
intake · <YYYY-MM-DD> · <topic>
  raw → braindumps/<filename>

updated:
  · <key> — <1-line change> <status: old → new if shifted>
  · <key> — <1-line change>

open items touched:
  · <key> — added N, closed M
  · <key> — added N

contradictions: <N — see contradictions/YYYY-MM-DD.md>   (omit line if N=0)

gaps noted (for resurface):
  · haven't heard: <keys or "none">
  · mentioned, not caught: <names or "none">
```

That's it. No filler. If he wants more, he'll ask.

## Weekly brief compile

Triggered by: "compile the brief", "Monday brief", start of a new cycle,
"new deck." Usually follows a Friday braindump that fed the intake flow.

Process:

1. **Do the intake flow first** on the Friday dump if it hasn't been run
   yet. The brief reads from the MDs, so they need to be current.
2. **Read everything.** All of `caseload/*.md`, `projects/*.md`,
   `plans-registry.csv`, `profile/identity.md`, and the most recent
   `caseload-deck-NNN.html` for style reference.
3. **Determine cycle frame.**
   - Cycle window: next Wednesday → Wednesday (2 weeks).
   - Cycle number: previous number + 1.
   - Any OOO: pull from the dump; confirm in `profile/identity.md`
     Constraints section.
4. **Derive the cycle theme** — the cover's headline + synthesis blurb.
   See `weekly-brief-spec.md §4 Cover` for the full authoring rules.
   Short version: the theme is a synthesis of the entire Friday
   braindump, not a slogan.

   - If Joel named the theme in the dump ("this cycle is really about…"
     or equivalent), use that phrasing. His line beats an inferred one
     every time.
   - If he didn't, derive one. Scan the dump for: the OOO shape, the one
     plan everyone's orbiting, the dominant blocker class across plans,
     the tension between active work and parked work. Pick the axis that
     explains the most of the cycle. Write a 2–5 word lowercase
     headline (second word in `<em>`), then a 55–80 word blurb in three
     beats: **frame** (window + shaping fact), **where the energy is**
     (1–2 sentences on the active front), **what to hold** (1 sentence
     on what's parked by design).
   - Tone matches the brief-slide tone rules in this skill — casual but
     proper, operator's voice, no slang.
   - If you had to guess at the theme (operator didn't name it), note
     that in the intake reply so Joel can reject it before compile.
5. **Compose the deck** per `weekly-brief-spec.md`. The 11 slides (cover,
   triage, 5 plan details, monitor, resurface, lab, 14-day, checklist).
   **Style is the Ferrari theme** — chiaroscuro editorial, Ferrari Red
   (`#DA291C`) as the sole accent, Inter Tight display, Archivo sans,
   JetBrains Mono labels, no scanline, hairline rules, no corner brackets.
   The canonical structural template is
   `mocks/caseload-deck-028-ferrari.html` (and going forward the most
   recent `caseload-deck-NNN.html`, which should already be ferrari-styled).
   Copy it, bump the cycle number, swap the window, swap in new content,
   replace the cover headline + blurb with the theme from step 4. Same
   stylesheet, no new tokens. The older warm-console (bg `#ede6d3`, with
   scanline + corner brackets) lives in `caseload-deck-027.html` and
   earlier — do **not** pull structure from those decks anymore; they're
   the previous style era.
6. **Populate `resurface.` from the gaps noted during intake** (§6 above).
   Keep panels to max 3 rows each. If a panel is empty, drop it. If all
   three are empty, drop the slide entirely.
7. **Checklist data-ids are permanent.** Format `<plan>-<NN>` (`cal-03`,
   `lab-04`). Never reuse an id for a different task. Storage key rolls
   with the cycle: `weekly-brief-checklist-<NNN>`.
8. **Save** as `caseload-deck-<NNN>.html` in the project root (same place
   as the existing decks).
9. **Reply** with a one-line summary + file path. Joel opens it from
   there.

## Export flow (on-demand snapshots)

Triggered by: "snapshot", "caseload snapshot", "projects portfolio",
"project dump for <key>", "handoff pack", "out of office", "I'm going to
be out <dates>."

Four variants. All follow `docs/superpowers/specs/2026-04-22-exports-design.md`
— don't re-derive the structure, read the spec. Below is the routing and the
intake-specific policy on top.

### Variant selection

| Phrase                                          | Output                        |
|-------------------------------------------------|-------------------------------|
| "caseload snapshot" / "snapshot of plans"       | `exports/caseload-YYYY-MM-DD.html` |
| "projects portfolio" / "all projects"           | `exports/projects-YYYY-MM-DD.html` |
| "project dump for `<key>`"                      | `exports/project-<key>-YYYY-MM-DD.html` |
| "handoff pack" / "OOO" / "out of office"        | `exports/handoff-YYYY-MM-DD-to-YYYY-MM-DD.html` |
| "plan extract" / "current plan extract" / "where things stand" | `exports/plan-extract-YYYY-MM-DD.html` |

### Plan extract — the one-column "where things stand" read

A single-pass scroll-down read of the entire caseload at a single
moment in time. Built for the operator's own quick-glance need ("show
me where everything is"), not for handing to coverage. Lighter touch
than the handoff pack — no directives, no roster, no return-date
strip. The brief is informational, not instructional.

**Trigger phrases:** "plan extract," "current plan extract," "where
things stand," "one-column extract," "give me the read."

**Output:** `exports/plan-extract-YYYY-MM-DD.html` (same-day re-runs
get `-v2`, `-v3`, …).

**Composition rules — these are the intake-specific policy:**

1. **The brief at the top is synthesized from the most recent
   braindump** (or from explicit context in the prompt itself if no
   recent dump exists). It's a 4–6 short paragraphs digest in the
   operator's voice — what just happened, where energy is, what's
   parked. Same override rule as the handoff: brief beats MDs on
   conflict, but the conflict here is informational ("what's the
   current state") rather than directive ("what should coverage do").
   No "do / hold / don't" pull-out — just a narrative read.

2. **Plan blocks pull verbatim state from the caseload MDs.** Most
   recent dated entry → one-paragraph state digest (≤ 64ch lines, ≤ 3
   short paragraphs). Don't dump the whole entry; distill. Open Items
   render verbatim with three states preserved: `[ ]` open, `[x]`
   closed (struck-through; keep for one cycle then drop on next
   extract), `[~]` blocked. `⚠` items get a red-edged checkbox + red
   prefix glyph.

3. **Group plans by attention, not by status.** Three groups, in this
   order:
   - **A · Hot this week** — plans with active fires, today's
     blockers, or breakthroughs in the most recent dump.
   - **B · Imminent · within 2 weeks** — registry `effective_date` or
     `asset_transfer_date` lands within 14 days of compile date,
     OR `early_access_start` is within 14 days.
   - **C · Parked · correctly quiet** — everything else: future-
     dated, on-hold, explicitly "still nothing," limited-scope
     waiting on the scope window.
   Each group has a small section break (`b · count · plans`) so the
   eye can find the boundary without parsing every block.

4. **TOC at the top** with one row per plan: number, name, stage
   chip, next-date column. Color-coded next-date: red = same-cycle
   fire, ink = imminent, light grey = parked. Anchor links jump to
   each plan section.

5. **Per-plan facts strip** — 4 cells: day1 · effective · asset
   xfer · owner (or scope/source if owner isn't meaningful). Pull
   from frontmatter + registry. Render `TBD` in red when missing.

6. **Don't include directives, roster, out-of-scope strip, or "when
   I'm back" panel** — those belong to the handoff pack. Keep this
   lean.

7. **Group attention can flip mid-cycle.** A plan that was Parked
   yesterday can be Hot today (e.g. COCC). Re-derive groups every
   compile from the most recent state — don't cache.

**Theme:** default to the most recent style era. As of cycle 029
that's Ferrari (Inter Tight + Archivo + JetBrains Mono, single red
accent, hairline rules, no scanline). When the operator names a
theme explicitly ("redo it in figma"), pull tokens from
`DESIGN.md` at the project root and rebuild — same composition,
different tokens. Don't mix tokens across themes within a single
extract.

**Reference implementation:** `exports/plan-extract-2026-04-24.html`
(Ferrari) and any `-figma`/`-stripe`/etc. theme variants saved
alongside it.

### Handoff pack — the one with the briefing

This is the one Joel called out as important. Read the exports spec §5.4
for the layout; the **intake-specific policy** is this:

1. **Capture the briefing from the prompt itself.** When Joel asks for the
   handoff, he'll include natural-language context: "I'm out two weeks,
   Callidi is in good shape, please hold off on the Pella workaround, call
   me if Haverhill recon looks off." This is the **briefing**. Capture it
   verbatim and hold every explicit plan mention for the directives list.

2. **The briefing OVERRIDES the MDs** on any point where they conflict.
   If the Callidi MD still shows an open item "nudge client for DOB" but
   the briefing says "it's in good shape, hold any inbound data," the
   briefing wins. Joel is speaking most-recent truth — the MDs are stale
   by definition the moment he spoke.

   In the output, show the briefing at the top, and for any overridden MD
   content in later plan sections, render it with a muted note:
   `· overridden by briefing: "<quote>"`. Don't delete the MD content
   from the snapshot — the covering person might still need the full
   context, just framed correctly.

3. **Slide sequence (fixed):**

   - **Slide 1 / Title.** Masthead (HANDOFF stencil, window, rev date),
     cover headline `coverage pack.`, operator-out line with window and
     coverage person, roster/TOC of in-window plans with clickable
     anchors (`#plan-<key>`), out-of-scope strip, operator bottom strip.
   - **Slide 2 / Brief. ALWAYS PAGE 2. This is the FYI page.** Natural-
     language briefing in a rust-bordered transmission panel, with a
     do / hold / don't directives pull-out on the right. Structure,
     classes, and tone are canonical — **copy from the template asset**:
     `.claude/skills/weekly-digest/assets/brief-slide.html`. Reference
     implementation:
     `exports/MOCK-handoff-front-and-brief-2026-04-22.html`.
   - **Slides 3–N / Plan sections.** One per in-scope plan. Directive
     tone — "If X happens, do Y" — not self-reminders. Honor any
     directives from the brief.
   - **Out-of-scope strip.** Ledger-style, one line per active plan left
     alone so coverage knows what's intentionally skipped.
   - **Close panel.** "When I'm back" — what to hand back, what to flag.

4. **Scoping (which plans go in):** auto-propose, confirm with Joel before
   compiling. A plan is in-scope if any of:
   - Registry EFF or WIRE date falls inside the window.
   - Frontmatter `tags` include `coverage-needed`.
   - Joel named it in the prompt.
   - The briefing mentions it.

   If Joel says "just generate it, I trust you," skip the confirm and go.

5. **Trim long-term projects.** Joel explicitly said the handoff
   shouldn't include every long-term project — only what's relevant to
   the window. Default: include projects in `flight` or `ready-to-ship`;
   omit `idea` and `scaffold` unless the briefing names them.

6. **Keep the design language.** Same retro-console palette, same
   components as the weekly brief. No new tokens. The brief slide adds
   four classes on top (`.brief-frame`, `.briefing`, `.directives`,
   `.directive`) — those live in the template asset and get pasted into
   the deck's stylesheet once per compile.

#### Brief slide rules (slide 2)

- **Structure is locked.** Two-column: briefing (rust-bordered
  transmission panel) left, directives (bracketed card) right. Eye label
  `§ 02 · HANDOFF · FROM THE OPERATOR`, heading `the brief.`, footer
  strip noting the brief overrides plan sections. The template asset is
  the source of truth for markup and CSS.

- **Writing pattern for the briefing body:**
  - Paragraph 1: window + coverage person + the "brief wins on conflict"
    line.
  - One paragraph per major plan that needs coverage action. Use
    `<b>Plan Name</b>` for the plan reference, `<em>…</em>` on the
    actual directive (the thing to do / not do).
  - One short paragraph for out-of-scope / future-dated plans.
  - Sign off with a one-line thanks and the operator's name in bold.

- **Tone: casual but proper.** Plain English, first person, warm but
  professional. You are writing in the operator's voice to a teammate
  who will cover the caseload. This is not a Slack DM and it is not a
  formal memo — it's the note you'd leave on a desk.

  **Acceptable:** "please hold off on reaching out," "give me a call,"
  "park any pings," "enjoy the quiet," "appreciate it."

  **Not acceptable:** "don't trip," "sit on it," "don't chase him,"
  "bubba," "cool," "sweet," any slang or bro-speak. Also no corporate
  filler — no "please be advised," "kindly," "as per."

  If a phrase feels like something you'd say on a podcast, rewrite it.
  If a phrase feels like something HR wrote, also rewrite it. The
  target is somewhere between: a coworker you trust, being respectful
  of their time.

- **Directive taxonomy (three marks, three colors):**

  | Class | Mark | Color  | Meaning                                     |
  |-------|------|--------|---------------------------------------------|
  | `do`   | `✓`  | teal   | Affirmative action coverage should take     |
  | `hold` | `◌`  | butter | Receive but do not act; queue for operator  |
  | `dont` | `✕`  | rust   | Explicit prohibition                        |

  Every directive line starts with a `.k` chip naming the plan key or
  category (`Haverhill`, `Pella`, `ISD`, `Future Plans`, `Escalate`).
  Aim for 6-10 directives. Always close with one `do` directive for
  `Escalate` ("phone is on" or equivalent).

- **Clean hand-back preference.** The closing paragraph should reinforce
  "one clean hand-back email beats a pile of well-intended changes."
  Joel wants coverage to default to caution, not activity.

### Same-day re-compile

If Joel asks for a second pack on the same day (maybe the window changed
or he forgot a plan), save as `-v2`. Don't overwrite. Exports are
snapshots — preserving the prior one is the point.

## Design system rules (don't break these)

These apply to every HTML artifact this skill produces (brief, snapshots,
handoff). The **current style is Ferrari theme** — a token swap on top of
the warm-console structure. Full token set lives in the `:root` of
`mocks/caseload-deck-028-ferrari.html`; pull it from there.

- **Three faces, one job each:** Inter Tight (monumental display, near-
  black with Ferrari Red `<em>`), Archivo (sans body + uppercase wide-
  tracked labels), JetBrains Mono (mono data). Don't mix with the old
  Major Mono Display / DM Sans / Space Mono set.
- **Palette is chiaroscuro editorial.** Pure white `#ffffff` paper,
  near-black `#181818` ink, **Ferrari Red `#DA291C`** as the sole
  accent. Second-accent slots (teal / butter) collapse to monochrome
  greys — no teal, no butter, no warm browns.
- **Hairline rules, no corner brackets.** Every card-shaped element is
  a 1px `#D2D2D2` rule on white. The diagonal corner brackets from the
  warm-console era are explicitly turned off in the ferrari overrides.
- **No scanline overlay.** `--scanline-opacity: 0`.
- **Display is lowercase Inter Tight, not Major Mono.** Tight negative
  tracking (-0.035em), near-1.0 line-height, `<em>` in Ferrari Red.
- **Labels are Archivo uppercase, 0.16em tracking** (not 0.32em Space
  Mono).
- **Header + footer bars are solid black** (`#181818`) with white text
  and red accent separators.
- **`M / DD` effective dates.** Zero-pad the day.
- **Plan name in `<em>`** for the second word (red): `callidi <em>tas</em>`.
- **Tabular numerals on all numeric columns.** `font-variant-numeric:
  tabular-nums;` (already on via `font-feature-settings: "tnum"`).

If something looks off, open the ferrari mock and diff — that file is the
authoritative component library until a new style era is declared.

## Authoring voice (Joel's preferences)

From `profile/identity.md`:

- **Plain English, imperative next moves, no corporate filler.**
- **"Make it tight, stop decorating."** Function over style once something
  works.
- **One slide per plan, one card per project. No walls of text.**
- **If something is missing from a braindump, surface it on `resurface.`
  — don't guess.**
- **Kill ideas cleanly.** When Joel says "stop thinking about it" (like he
  did with the Mapping Validator as a standalone), honor it. Mark the
  status `shipped-folded` or equivalent and move on. Don't keep
  resurrecting killed ideas.
- **The operator gave explicit permission** to sneak in honest
  observations he might not want to see. Log what's evidenced in the
  profile `## Observations` section. Multi-cycle patterns weigh most;
  single-cycle reads are fair game when grounded in a concrete quote.

## The three non-negotiables

If you forget everything else, hold these:

1. **Nothing explicit gets dropped.** Every open item Joel names lands in
   an `### Open Items` block with a `[ ]` checkbox. The moment you find
   yourself thinking "that's probably implied, I'll skip flagging it" —
   stop and flag it.

2. **Contradictions surface, they don't silently resolve.** If the new
   dump disagrees with the file, write to `contradictions/YYYY-MM-DD.md`
   and flag it inline on the dated entry. Joel decides. The skill doesn't.

3. **Briefing context beats MD content.** On exports, the operator's
   natural-language context at compile time is the most recent truth.
   Render it first, let it override, and keep the MD content visible but
   subordinated where there's conflict.

Everything else is mechanics. These three are the skill's reason to exist.
