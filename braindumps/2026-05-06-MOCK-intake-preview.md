# MOCK Intake Preview — 2026-05-06

This file shows what the intake loop would produce if the mock braindump
at `2026-05-06-MOCK-cycle-close.txt` were real. No source files were
modified — this is a preview only.

Source: `braindumps/2026-05-06-MOCK-cycle-close.txt`

---

## 1. Classification Pass

| Chunk                                    | Route                            | Action               |
|------------------------------------------|----------------------------------|----------------------|
| "Callidi is done"                        | `caseload/callidi.md`            | status + append + registry |
| "NBI Search, I actually shipped it"      | `projects/nbi-search.md`         | status + append      |
| "ISD — deferral file came early, policy escalated" | `caseload/isd.md`      | append               |
| "TA Brain — Anthony demo happened, went well" | `projects/ta-brain.md`      | append               |
| "Anthony looping back re: TC section"    | `projects/ta-brain-tc-section.md` | append              |
| "Data parser — didn't really touch it"   | `projects/data-parser.md`        | append (no-op entry) |
| "Pella — deferral report is messy"       | `caseload/pella.md`              | append               |
| "Haverhill — Beth covered OOO, no fires" | `caseload/haverhill.md`          | append               |
| "email inbox triage — seed idea"         | `projects/email-triage.md`       | **new file** (status: idea) |
| "backslash palette — didn't get to it"   | (gap — self-blocker)             | resurface slide row  |

---

## 2. File-by-File Deltas

### `caseload/callidi.md`

**Frontmatter changes:**
```diff
- status: waiting
+ status: final
- tags: [client-blocked, loan-issue, asset-transfer]
+ tags: [client-blocked, loan-issue, asset-transfer, closed]
- last_mentioned: 2026-04-22
+ last_mentioned: 2026-05-06
```

**Body — append:**
```markdown
### 2026-05-06

- **Closed.** Client delivered DOB + DOH. Loaded participants, loaded
  balances. Re-amortized the loan — turned out to be a logging issue,
  not a missed payment.
- status: waiting → final.
- Off the plate.
```

---

### `projects/nbi-search.md`

**Frontmatter changes:**
```diff
- status: ready-to-ship
+ status: shipped
- tags: [ui, search, ship-blocker-file-path]
+ tags: [ui, search, shipped]
- last_mentioned: 2026-04-22
+ last_mentioned: 2026-05-06
```

**Body — append:**
```markdown
### 2026-05-06

- **Shipped.** File picker replaced with a JSON config sitting next to
  the HTML; refresh button pulls fresh data on demand.
- Daily use starts this week; will log any breakage as it surfaces.
- status: ready-to-ship → shipped.
```

---

### `caseload/isd.md`

**Frontmatter changes:**
```diff
- last_mentioned: 2026-04-22
+ last_mentioned: 2026-05-06
```
Status stays `active`.

**Body — append:**
```markdown
### 2026-05-06

- Pushed for early delivery of the deferral file — client agreed.
  (Question asked in 4/22 entry resolved.)
- The broader policy debate (startup-with-blackout vs. clean-start)
  kicked up to a committee. Now on them, not us.
- **Next move:** keep the deferral analysis package ready for whenever
  the committee decides.
```

---

### `projects/ta-brain.md`

**Frontmatter:** only bump `last_mentioned: 2026-05-06`. Status stays `flight`.

**Body — append:**
```markdown
### 2026-05-06

- Anthony demo landed Friday. Walked the sanitizer with a test payload
  (fake socials, addresses) — showed the full strip. He was in.
- He'll loop back this week with a timeline for opening the TC section.
- Two more animations added on dashboard load.
- **Still open:** backslash command palette for search (not touched
  this cycle).
- **Still open:** concepts page redesign (not touched this cycle).
```

---

### `projects/ta-brain-tc-section.md`

**Frontmatter changes:**
```diff
- status: idea
+ status: idea        (unchanged — Anthony hasn't greenlit yet)
- last_mentioned: 2026-04-22
+ last_mentioned: 2026-05-06
- tags: [depends-on-anthony, data-safety, expansion]
+ tags: [depends-on-anthony, data-safety, expansion, demo-complete]
```

**Body — append:**
```markdown
### 2026-05-06

- Data-safety demo with Anthony complete — received well.
- Anthony to follow up with a timeline this week. Blocker shifts from
  "need demo" to "awaiting Anthony's timeline."
- **Next move:** wait 7 days; if no timeline by 5/13, ping him.
```

---

### `projects/data-parser.md`

**Frontmatter:** only bump `last_mentioned: 2026-05-06`.

**Body — append:**
```markdown
### 2026-05-06

- Not touched this cycle — NBI ship + Anthony demo took the bandwidth.
- All open items from 4/22 still stand: golden-set test files,
  summary-report decision, mapping-validator coverage check.
- **Next cycle:** carve out a day for test files.
```

---

### `caseload/pella.md`

**Frontmatter:** only bump `last_mentioned: 2026-05-06`.

**Body — append:**
```markdown
### 2026-05-06

- ADP deferral report arrived — messy, as expected.
- Workflow is the known workaround: load everyone out of default at
  current %, hand client the ambiguous window group for direct
  notification.
- **Open:** ADP rep's name + email still not logged (gap).
```

---

### `caseload/haverhill.md`

**Frontmatter changes:**
```diff
- last_mentioned: 2026-04-22
+ last_mentioned: 2026-05-06
- tags: [contact-change, coverage-needed, ramping]
+ tags: [contact-change, coverage-clean, ramping]
```
Status stays `active` (post-asset reconciliation still owed).

**Body — append:**
```markdown
### 2026-05-06

- Assets landed during OOO. Beth covered the load — clean, no fires.
- **Next move:** post-wire reconciliation, 30-day review scheduling.
```

---

### `projects/email-triage.md` (NEW FILE)

```yaml
---
type: project
key: email-triage
status: idea
tags: [ui, workflow, inbox, seed]
last_mentioned: 2026-05-06
first_logged: 2026-05-06
---
```

```markdown
# Email Triage

### 2026-05-06

- Seed idea: a small thing to sort client email by "needs response
  today" vs "can wait." Likely a Gmail filter setup plus a minimal
  dashboard.
- Not starting now. Logged so it doesn't slip.
- **Trigger to start:** next cycle where inbox management eats
  visible time.
```

---

### `plans-registry.csv`

One row updated:
```diff
- callidi,JK62945-00001,Callidatas,Callidi Tas,2026-01-01,2026-04-01,2026-04-10,final,full,Final stages in progress. All other tasks complete.
+ callidi,JK62945-00001,Callidatas,Callidi Tas,2026-01-01,2026-04-01,2026-04-10,final,full,Closed 2026-05-06. Loaded participants, balances, loan re-amortized.
```

(note: registry already had `status: final`. After close, this would
move to `plans-registry-archive.csv` next cycle per brief spec §9.)

---

## 3. What The `resurface.` Slide Would Show This Cycle

```
§ 0X / TRANSMISSION 027 / RESURFACE
haven't heard about these.
quiet corners worth a glance before the next cycle closes.

PANEL · HAVEN'T HEARD FROM
  (empty — no entity hit dormancy thresholds yet. next cycle may flag
   data-parser if still untouched.)

PANEL · YOU MENTIONED, I DIDN'T CATCH
  TA BRAIN        "backslash palette — didn't get to it"
                  → need: cycle slot allocated, or explicit deprioritize?
  DATA PARSER     "didn't really touch it this cycle"
                  → need: commit to a cycle for it, or accept drift?
  PELLA · ADP REP "new rep, seems ready"  [carryover from 4/22]
                  → need: name + email logged

PANEL · WORTH A LOOK
  NBI SEARCH       pattern — now that config-file loading works,
                   reuse it on TA Brain's data pull.       [pattern]
  TA BRAIN TC      if Anthony timeline arrives,
                   have a scoped starter task ready.       [extend]
  EMAIL TRIAGE     one-page prototype before writing filters:
                   label "today" / "soon" / "archive."     [extend]
```

---

## 4. Summary

**11 files would change:**
- 1 status transition to `final` (callidi)
- 1 status transition to `shipped` (nbi-search)
- 7 in-place appends with `last_mentioned` bumps
- 1 new project file (email-triage)
- 1 registry row update

**3 gaps surfaced** for the `resurface.` slide.
**3 suggestions** drafted by me for the "Worth a Look" panel.

This preview would be applied for real (modifying the files above) only
after the operator confirms the mock braindump is a good likeness and
the routing decisions make sense. For today it's a pattern demo.
