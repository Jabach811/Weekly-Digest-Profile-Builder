---
type: project
key: slide-creator
status: flight
tags: [ui, slides, side-app, pipeline]
flags: []
people: [beth]
depends_on: []
last_mentioned: 2026-04-24
first_logged: 2026-04-24
---

# Slide Creator

A tool Joel has already stood up that turns a worksheet's information
into a slide deck. Built to convert step-by-step screen captures from
real conversion runs into shareable walkthrough decks.

## Invariants

- Each conversion produces input; each input sharpens the tool.
- Output shape is operational takeaway: numbered steps, callouts,
  "important to remember" pull-quotes.

## Off-ramp

None — ongoing. Lives as long as conversions keep producing input.

## Next move

Run the 4/24 Calidus screenshots through the current pipeline
end-to-end this cycle to surface gaps.

## Log

### 2026-04-24

- Joel: "Now that I've got the slide creator based on that worksheet,
  we'll have to pump that up. Another thing to make sure we do by end
  of week — really good tool. As long as we stick the information on
  there, we can turn it into an awesome slide deck really easily. I
  really want to do that. Want to tune that skill up — we can really
  do a good job of it."
- **Pipeline this cycle:** Joel captured a full set of screenshots
  during the Calidus walkthrough with Beth (post-Informatica balance
  load: parameter-file flip, Bill Remit Detail, P3 → ROC reversal,
  conversion-creation, loan-header / loan-source matching). COCC
  residual-dividend run next week is teed up to feed it too.
- Status: **flight** — already working; investing more time to
  upgrade output quality and ergonomics.

## Open Items

- [ ] Run the 4/24 Calidus screenshots through the current pipeline
      to surface gaps.
- [ ] Tune the worksheet → slide template (callouts, "important to
      remember" pull-quotes, numbered procedure steps).
- [ ] Capture COCC residual-dividend run screenshots next week and
      feed them through the pipeline.
- [ ] Decide whether this becomes its own dashboard tile under
      [app-portfolio-site](projects/app-portfolio-site.md), or stays a
      private operator tool for now.
