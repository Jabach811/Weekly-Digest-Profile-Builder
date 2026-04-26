---
name: plan-dump-slides
description: Build current-state Weekly Digest plan dump decks from the live caseload markdown and registry. Focus on practical planning, clean status segmentation, and distinctive HTML slide treatments.
version: "0.3"
---

# Plan Dump Slides

Use this skill when the operator wants a current-state deck for active plans,
pending blockers, recently completed work, and upcoming windows.

## Purpose

Turn the Weekly Digest brain into a practical slide cartridge:

- what is complete enough to stop worrying about
- what is actively moving
- what is blocked or contradictory
- what is coming up next
- what is parked on the horizon

## Source Of Truth

Read from:

- `plans-registry.csv`
- `caseload/*.md`
- `contradictions/*.md` when there are unresolved conflicts worth surfacing
- `profile/identity.md` only if operator tone or recurring constraints matter

Do not invent progress. If something is unresolved, say that cleanly.

## Slide Rules

- Single-file HTML
- Every slide fits inside `100vh` / `100dvh`
- Use progress bar + nav dots + keyboard navigation
- Keep slides dense but scannable
- Prefer 7-9 slides over one bloated artifact

## Content Frame

Every plan dump should cover:

1. Portfolio pulse
2. Recently completed or materially de-risked work
3. Active work now
4. Pending blockers / contradictions / TBDs
5. Upcoming dates and windows
6. Roster view for all plans
7. Action checklist

## Writing Rules

- Favor operator language over consultant fluff
- Use short action phrases
- Name dates when they matter
- Separate "done" from "safer now but not closed"
- Call out contradictions as watch items, not resolved facts

## Initial Design Direction

Version `0.1` default: `mission control newspaper`

- warm paper background
- sharp urgency rails
- editorial serif headlines
- mono telemetry labels
- looks like a live ops dossier, not a SaaS dashboard

## Revision Log

### v0.1

- Initial workflow for first current-state plan dump pass
- Emphasis on clear buckets: complete, working, pending, coming up

### v0.2

- Explicitly separate `complete` from `de-risked but still open`
- Always include one roster slide so the whole book is visible at a glance
- Pair the pressure slide with a dedicated contradictions / watch slide
- End on a practical operator checklist, not a generic closing slide
- For dense ops decks, use a two-tier live-work split: `must move` vs `steady work`

### v0.3

- For tactical dark decks, split faults into `mechanical` vs `identity / data`
- Add an explicit `scheduled runs` slide when dates define workload shape
- Only keep a meta slide if it helps explain why that visual instrument exists
- Strengthen status color coding when scan-at-a-distance matters more than prose
- Use route-card framing when the operator needs the whole portfolio to read like a control wall

## Alternate Design Direction

Version `0.2` default alternate: `operations atelier`

- dark graphite workbench
- copper route lines and fault markers
- tactical machine-state framing
- better when the book feels like a live system under load

## Final Design Direction

Version `0.3` alternate: `transit wall`

- modular signal-board layout
- route strips per plan state
- bright but disciplined status coding
- best when the operator wants to scan the whole book in seconds
