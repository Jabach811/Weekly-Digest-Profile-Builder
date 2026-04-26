---
name: profile-builder
description: Use at the START of any Weekly Digest session, before weekly-digest or any brief/snapshot/handoff work, to route the operator to the correct workflow. Asks whether the operator is a DC with a caseload — if yes, hands off to the weekly-digest skill; if no (new hire, onboarding, no official caseload yet), runs the Profile Builder intake flow for people whose braindumps are less plan-specific. Trigger on session start inside the Weekly Digest folder, on phrases like "let's start", "new session", "onboarding dump", "new hire braindump", "profile builder", "build my profile", or whenever a braindump shows up from someone who isn't Joel or isn't running a formal caseload. Always run this router BEFORE defaulting to weekly-digest — it's the front door.
---

# Profile Builder — Intake Router + Onboarding Flow

This skill is the **front door** for the Weekly Digest folder. Before any
braindump gets routed into the caseload/projects/profile structure that the
`weekly-digest` skill expects, this skill checks which kind of operator is
actually at the keyboard.

**Why this exists:** the weekly-digest skill assumes the operator has a real
caseload (DC with plans, registry rows, cycle cadence). A new hire in
onboarding does not. Her braindumps will be about training modules, shadowing,
team intros, systems she's learning, questions she's collecting — not about
plan liquidations or wire dates. Forcing that content through the caseload
intake flow would mangle it. So we fork at the front.

## The routing questions — ask these FIRST, in this order

Before doing ANY other work (no reading files, no classifying, no compiling),
ask these two questions and wait for answers. One at a time. Don't batch.

### Question 1 — caseload check

> **Are you a DC with a caseload?** (yes / no)

- **yes** → stop this skill. Hand off to the `weekly-digest` skill and run its
  normal flow. Say something like: "Routing you to the weekly-digest
  workflow." Then invoke weekly-digest and present the DC-branch menu (below).
- **no** → continue in this skill (Profile Builder flow, below).

#### DC-branch menu (after "yes" → weekly-digest)

Once weekly-digest is loaded, present the operator with a short menu so he
can pick what he actually wants to do this session — don't just wait silently
for him to phrase it. Ask:

> **What are we doing? (pick one, or say what you want.)**
>
> - **intake** — process a new braindump
> - **weekly digest** — compile the Monday/cycle brief (`caseload-deck-NNN.html`)
> - **export** — snapshot, projects portfolio, single project, or handoff pack
> - **targeted update** — change a specific fact on a plan/project without a full dump

Route to the matching weekly-digest flow. If he says something that doesn't
match cleanly, fall back to weekly-digest's own routing table.

If the answer is unclear (e.g. "kinda", "not yet", "I'm new"), treat it as
**no** and continue here. New-hire ambiguity routes to Profile Builder by
default — it's the safer read since it won't try to write to caseload files
that don't exist.

### Question 2 — braindump check

Only after the operator answers Q1 with "no":

> **Attach a braindump if you have one — or say "no" to work from what I've
> already got stored.**

- **braindump provided** (pasted text, or a file path, or raw transcript) →
  go to [Fresh braindump flow](#fresh-braindump-flow).
- **"no" / no braindump** → go to [Stored-context flow](#stored-context-flow).

Wait for the answer. Don't guess. If the operator sends a long message that
isn't clearly a braindump, ask once: "Is that a braindump, or are you telling
me there isn't one this time?"

## Profile Builder flow (when the operator is NOT a DC with a caseload)

This is a scaffold we'll build out together as the workflow matures. The
shape below is the starting point — treat it as provisional, not canonical.
When in doubt about a detail that isn't spelled out here, ask the operator
rather than invent a convention.

### What the Profile Builder is for

The operator is a **new hire in onboarding**. There is no official caseload,
no plans registry, no cycle cadence yet. What she *does* have:

- Training modules she's working through
- People she's meeting / shadowing
- Systems and tools she's being introduced to
- Questions and gaps she's collecting
- Early observations about how the team actually works
- Goals for the onboarding period

The Profile Builder's job is to capture all of that into a **profile-centric**
structure (not plan-centric), so that when she later graduates to a real
caseload, the history of her onboarding is intact and the profile file is
already rich.

### Fresh braindump flow

When the operator attaches a new braindump:

1. **Archive the raw dump verbatim.** Save to
   `braindumps/YYYY-MM-DD-<topic>.txt`. Same rule as weekly-digest — the raw
   file is forensic, never edit after save. Topic slug should reflect the
   onboarding context (`onboarding-week1`, `shadowing-notes`,
   `training-adp-intro`, etc.).

2. **Digest and apply to the profile presentation.** Walk the dump chunk by
   chunk and route content. The routing targets are different from
   weekly-digest — we're not writing to `caseload/` or `projects/`. Instead:

   - **Identity / background / working style** → operator's profile file
     (path TBD — ask the operator where she wants this stored; default
     proposal: `profile/<her-name>.md`, separate from Joel's
     `profile/identity.md`).
   - **Onboarding progress / modules / shadowing** → an onboarding log
     section inside her profile file (or a sibling file like
     `profile/<her-name>-onboarding.md` — confirm with the operator).
   - **Questions / gaps / things to clarify** → an `### Open Questions`
     section, analogous to weekly-digest's `### Open Items` but framed
     around learning, not action.
   - **People she's met** → a roster section inside the profile.
   - **Systems / tools** → a systems section inside the profile.

3. **Apply the new information to the presentation.** The Profile Builder
   produces a profile presentation (format TBD — likely an HTML deck in the
   same design language as the weekly brief, but shaped around "who she is,
   where she is in onboarding, what she's learned" rather than plans and
   cycles). For v1, propose a structure, show it to the operator, and let
   her react. Do not invent slides she didn't ask for.

4. **Surface gaps.** If the braindump mentions a person, system, or topic
   with no follow-up context, flag it so she can fill it in on the next dump.

5. **Reply with a short summary** of what was captured and what was applied,
   using a format similar to weekly-digest's intake reply but adapted:

   ```
   profile · <YYYY-MM-DD> · <topic>
     raw → braindumps/<filename>

   updated:
     · profile — <1-line change>
     · onboarding log — <1-line change>

   open questions touched:
     · added N, closed M

   gaps noted:
     · <people/systems/topics mentioned without detail>
   ```

### Stored-context flow

When the operator says "no" to the braindump question:

1. **Read what's already stored.** Look at her profile file(s), any previous
   onboarding log entries, any prior braindumps in `braindumps/`. If this is
   truly the first session and nothing is stored yet, say so — don't
   fabricate a profile out of nothing.

2. **Ask what she wants to do next** with the stored context. Options might
   include:
   - Refresh / regenerate the profile presentation from what's on file.
   - Add a specific fact or correction without a full braindump.
   - Review open questions from previous sessions.
   - Something else — let her drive.

3. **Apply whatever she asks for** using the stored data as the source of
   truth. No fabrication. If a piece of info isn't on file, ask — don't
   guess.

## Handoff to weekly-digest

If at any point during this skill the operator clarifies that she IS a DC
with a caseload (or the context shifts mid-session — e.g. Joel is actually
the one typing), stop the Profile Builder flow and route to `weekly-digest`.
Say so explicitly so the operator knows the workflow switched.

## What this skill doesn't do (yet)

This is v1 — intentionally minimal. As we curate the workflow today, expand
this skill with:

- The concrete profile file template (frontmatter + section structure)
- The profile presentation spec (slide list, design tokens, output path)
- A full classification taxonomy for onboarding-shaped content
- The rules for graduating from Profile Builder → weekly-digest when she
  gets her first real caseload

Until those are in, use operator judgment for anything not specified above,
and ask before inventing conventions. The point of today's session is to
discover what should live in this skill by running it on a real braindump.

## The non-negotiables (for now)

1. **Always ask the caseload question FIRST.** No exceptions. This skill's
   only job if the operator is a DC with a caseload is to route them away.

2. **Always ask the braindump question SECOND.** Don't assume; don't skip.
   The answer changes the entire rest of the flow.

3. **Nothing explicit gets dropped.** Same prime directive as weekly-digest —
   every concrete thing the operator says lands somewhere (profile, open
   questions, roster, systems, gaps). Silence is a failure mode.

4. **Don't force onboarding content into caseload shape.** If something
   doesn't fit the profile-centric structure, flag it and ask. Don't invent
   a fake plan just to have somewhere to put it.
