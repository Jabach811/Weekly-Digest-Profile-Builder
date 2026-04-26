# Brain Dumps — Joel Batchley

## BD 1

Calidus is moving along. Right now we have a major blocker: Informatica is down. Looks like a company-wide network issue. That's going to stop me from being able to post the participant balances and loans. Both are ready to be imported — we just need Informatica to open up and then we can process those.

The plan for Calidus: once Informatica opens up, we post participant balances, process all that, process the loans, then process elections, and we'll be good to go. That's all contingent on when Informatica opens up. Once it does, everything can go pretty quick because we're going to have everything prepped and ready to go. I'm almost there — going to finish that today and get all prepped. Once Informatica goes, we can pull the trigger.

Havern's good. Everybody's loaded. We just need to send the NBI status email — I'll have Beth do that.

New one on the books: **Banyan**, also called Cutting Edge Staffing. Not sure how it was set up. This is an active plan. We've got the limited access census file ready to be loaded. Based on what I was just given, the early access window for this plan is **May 14th to May 27th**. Asset transfer is going to be 6/1. The blackout ends on a Wednesday — early access ends, then the wire comes in and we'll process that. So I have a couple days to check everything out. That's usually how limited access goes — it closes and then we have a little bit of time to check to see everything that came in, see if it's all good, monitor everything, make sure nobody made changes they weren't supposed to make.

**Flag this as a takeaway:** after limited access is complete, it's good to monitor it. There are queries to run to check activity that the participants did. Because they're only able to do certain things during early access — sometimes change deferral elections, sometimes change beneficiaries, but most of the time it's mainly for investment elections because these are going to be cash conversions. Their current balance and current account is going to be completely different once it comes over to TransAmerica. If you don't put any elections, you'll get defaulted to a subscription service that creates the investment elections for you, and you're subscribed to their investment advice deal and get charged a fee. So they give participants a chance to go in and change their elections to whatever they want. Once the wire and account is processed, the money gets distributed based on whatever elections they picked.

**Important to remember:** there are other things they're not supposed to be able to change, but participants can call the call center and say "hey, I'm trying to change my deferral elections and I'm not able to, I want to put it at 10%." The call center isn't privy to the restrictions they're under — they hear a participant call up, check them, they're in the system, so they make the changes for them, even though they're not supposed to. Not the end of the world, doesn't happen often, and they had to do all the verifications, so it's not like it got hacked. There are flags and timestamps on every single thing, so it can all be verified, back-traced, audited, confirmed. Usually catch it when you're loading data and see there's existing data and you're like, "oh no, there shouldn't be because it's all brand new people," and you go back and see there was activity, very likely through the call center.

So: **Banyan Treatment Centers / Cutting Edge Staffing — QK 63283-00099**. Beth is on that one. She's going to enter the census, default the elections, run the awesome query Julius put together. Julius is the SQL master — go-to guy for all things SQL. He created a really awesome query that combines all the tables we need into one row per participant. Makes auditing the Excelsior plans so much easier.

Everything else is wrapping up later. Bridge and Can-Am are still way down the road. The Denver International one is down the road. Levy's still nothing — no changes from anything else.

Plus we have **TMG**. This one popped up out of nowhere. It's an older plan I converted before. Looks like we need to do a fix on vesting. We need years of service entered. We had originally thought it was going to be okay and we'd just be able to use their hire dates to calculate years of service, but that did not happen. People are not showing the correct vesting. So we need to go to the client and ask for years of service, load them, then flip from elapsed time to hours or something to fix their vesting. Need to get years of service from the client — Meredith, one of our old clients.

Add TMG to the plan list. It's a January 1st plan, old one I already completed long before. Lot of lessons learned from that one. I can build up the spec on it.

**WTW** — that's an advisor group. Add that entity to this profile. Excelsior is an investment group with a big group of plans that all have the same kind of rules. WTW is another advisor group with their own ways of doing business. WTW are sticklers, big time. They go after really big-money clients and are very hands-on. Meetings they create are unique, very hands-on, very thorough. They expect a lot.

Problem with TMG: a couple people are going to want to know how come this took so long to find out, and is anybody impacted? Good thing: no one is impacted — meaning nobody was under-vested who had a full liquidation. That would have been a tough deal. We'd still have their money, but it would have been tough to say "here's an extra check for $100 grand, we screwed up." Luckily that didn't happen. Me and the account manager checked the history and nobody who liquidated their account has had any forfeited funds. The fix is going to be done and no one's going to be impacted. One person noticed it — that's why we were able to see it. The participant went online and saw they were 0% vested when they should be 100% because they've been working at the company for 20 years. That's when we looked back and saw the issue.

So we go back to the client. Hopefully we can just get it as a "weird system thing." But it's got to get fixed. About 300 people who need their vesting turned back to 100. Need to figure out how to do that with the least amount of impact, exposure, and just get it done. I hate TMG. It's a thorn in my side. The account manager Ruby is a nice lady, but she will not leave me alone on this plan.

**Pella Carolina** — getting close to liquidation day. I just need to follow up on ADP and get a response on the deferral reports. Don't know if we need a specific contact because I've been sending things to a group email. Might need to talk to Sally, the new contact at Pella, and have her bang on them and get that report ASAP. Going to follow up on that by end of day today.

That is where I'm at right now. Update everything, then give me a one-week snapshot — for today through Friday — a little digest of where things are.

---

## BD 2

Quick update — something just happened. Looks like **COCC** is having a payroll issue. Part of that was on me, I'll wear that. I didn't set up EDS correctly and one of the sources for the payroll files was routed to the wrong source. Good thing: it's a relatively easy fix — not a financial fix, just an internal source-to-source transfer. But the payroll file is broken a little further because another source is getting funds it's not supposed to have. That needs to get fixed ASAP.

Right now I'm about to test Informatica. Live update incoming.

Test file — yes. Starting the workflow. Let's see if we get an error, because I was told the Informatica issue was a company-wide network problem, and Stacy told me it was fixed. Let's see if this works.

Hasn't been run yet. Now the actual test — start the workflow. Failed. "No such file." I know exactly what the problem is. Got past the first one though, that's what we expected.

Save. Now the test. Start the workflow. Running. Looks like we did it — got past the first one. Succeeded. **4/26, 10:57.** Detail error report — exactly what I expected. Excellent.

So: **Informatica is resolved. Calidus is going to have participant balances posted today. We're going to get loans imported today, and we might even get elections today.** Great news.

This is 10:57 AM. Next one will come a little later.

---

## BD 3 — Walkthrough with Beth

We're good now. Just tested Informatica and it went through. I'll share my screen and we can start the process.

Quick recap of where we are: we've already posted these funds, fund by fund, invested. All that money is invested within the dummy account, that **999-00000** person.

**Important to remember:** anytime you have a mapping plan, when you know for sure it's going to be mapping or transferring, make sure you add your takeover guy right away. Easy. It has to be that social though, because the workflow is set to write to that number. You can't just make up a dummy number — it has to be **999-00-0000**. You'll do this a lot, like for forfeiture accounts too if you ever import one of those. Give him a name — for this one we usually call him "Takeover Account." That's the standard name. Birth date is **12/25/1955**, hire date is **12/25/1985**. It's a number that, since everybody uses it, really stands out — another way to flag this guy as fake, not a real person.

Right now that money is sitting in that guy's account. We're going to apply it to the participant accounts using the same ref numbers, because those ref numbers have the right totals in the right fund. Ref number is mapped to fund and total. So we're going to use the same one — it's going to overlap and we'll see two entries, two totals.

Right now this is **Bill Remit Detail**. This is what we have. After we run participant balances, we're going to see this number double, and participants will equal this number here.

I just did a test run, it went through. Error report is exactly what we expected. Last time had the "Participant is not enrolled" error — now the only error (more of a warning) is "SSN is less than 9 characters." That's good. We're ready to import.

Close, back to my parameter file, turn Y to N. Not testing — straight to production. Save.

This is going to write to Bill Remit Detail.

I knew it was all good once we got past 5 seconds — about 20 seconds is how long this takes. **Succeeded.**

Now Bill Remit Detail has all the ref numbers in there, case number in there. Click refresh — running the background query. Brings in everybody's details. All good.

This is double. Equals this divided by that — beautiful. Participant balances are loaded. But there was also that dummy guy — got to get him off the books.

I like to keep a running total. Copy this, put it in a spot. **I'm going to write a macro to do all this** — don't like all this manual work. After this I'll write one.

Bill Remit Detail — call it BRD. Now we do the reversal.

Go into **P3 → ROC** at the top. Directs you to a new page. This is where we run queries that write or erase. Before we kind of had this in AQT, we'd just run those. This is very limited — you can't put in the wrong script and end up deleting the entire database. Very strict. Whatever you're going to do, this makes sure it's exactly what you say, not going to expand and screw anything else up.

Only a few things you'll do here. Most common: **ROC Reversal Query**. We'll do this in CORP.

Going to take screenshots as we go — it's good to start doing this since we're actually going in. Going from start to finish.

Source — I don't think it matters what you pick, I just click Process. Request ID is your abbreviated name, first initial — yours is BFloor1 or something. Click — if you want to see the script, click here, otherwise straight to Input.

Put your name back in. Case number is whatever your case number is — needs the proper spaces. Enter the SSN and the ref numbers.

Now you've got to get a little fancy. Take these and make a comma-delimited list. It's asking which Social Security person to reverse and which ref numbers. We want them all — completely wiped out. Not a mistake on one fund — reversing everything.

Validate. Be nice if it said something — only thing that happens is this button comes to life. (I've been doing a lot of UI design and seeing a lot of problems with this. I could fix these.) Click Analyze, get the results. Exactly what we expected.

It's going to delete lines from two tables: Bill Remit Detail and Transact Detail. Bill Remit Detail is the staging information — never goes away, which is cool. Transact Detail is the proof that it traded. Bill Remit is what was supposed to go, Transact Detail is what actually did. Need to clear both.

29 rows — 29 ref numbers. Make sure that's true: yes, 29. Perfect. With this, it's all or nothing. Good practice to check before clicking Run, because once you click, it runs — takes the person out and you can't get it back. With this you're only doing one guy, so you're good.

Run. Now that guy has been pulled. Total amount of lines that guy was involved in.

Back to Balances and queries → Bill Remit Detail, refresh. The 999 guy disappears — total goes back to $11M. Refresh. Beautiful.

Copy this — could have done Transact Detail before too, that probably would've been good. Already have a copy somewhere else. **BRD No Dummy.**

Now look at Transact Detail. That tells us these details have been applied to these people.

587-884 — we're missing a little bit. Let me see. Might be a format issue.

Hmm — missing one. Who? Let's find you. Yeah, I think I know. **XKB and XKC are not on Transact Detail.** Why?

Let me see if this magically works. *(Was on mute, coughing.)* Don't know why it didn't pick these two up. Not picking up.

Going to ping Earl real quick — actually, nah. Looking at this, they start having C in front, then BA and BB and all that. There's a ton because there are so many funds; after a certain amount it flips to A, B, C. Maybe these got caught up in that name and that's why the query isn't grabbing them. Don't know — those are the only two missing from this.

Can't go forward. Can't process what's out there. Going to P3 to see what it looks like, check the transactions for those.

Only details are missing for MM. That's the last part — making sure Transact Detail matches up. If it does, we're good. Don't know why it's not in there.

This is the script that's running. Don't see any problems. XKB, same set.

Going to open AQT — only time I really do is to troubleshoot something like this. Trim it down, take one of the bad ones.

XKB. Not there. Damn.

Looking for XKB — it's in Bill Remit Detail. **Why wouldn't it go through to Transact Detail?** Any ideas? Funds frozen or something?

Hope I don't have to reverse anything. Don't know how to reverse this — you'd have to delete the ref number.

Need to put the bat signal out to Earl. It's noon — he's going to need to get hooked in, this is a problem.

Once this is done, everything is over — not like there are 20 more things to do after. We're good. Once we figure that out, we'll do the last refresh.

We can do **loans** right now. Have it pretty much set up.

Jump on P3. I have a walkthrough on this.

In the plan, go to **Convert** → see if loans have been set up. Is it John Hancock? This is **Paychex**. Add a record keeper called Paychex. That's all we need to add. They're in there — Save.

Now go to **Conversions** tab → enter the case number. Just the case number, no affiliate. Search. **New Conversion.** That one was for a different record keeper — we're doing this for Paychex. This is one of those plans with a ton of people, so a bunch of different ones. Select our Paychex one.

Three dates: conversion date received, effective date, assigned date. Effective date is the actual effective date — call this 4/1. Conversion date and assigned date are largely irrelevant — go 3 months before whatever the effective date is. Doesn't really matter, but needs to be filled in for routines to run. Just to be safe, this is what we use. Save.

Got our conversion number — official. Now we can post loans.

When we go into the loan module in Informatica and test, it's going to look for this. If it doesn't see it, it'll say there's nothing set up to grab in P3. So go out there and do that. Since we're good, this number is attached to the output. Run it through Informatica — if everything's good, kicks out the import files. We take those import files and run. Only get the import files if everything's good. When we get those, we take it off test, and it grabs those files and applies it. Then we go to the next part.

Do this ahead of time. Once you do this, you can move on. Now you can actually test and process your loans.

I already did that. Let me show you what it looks like.

We've already gone over the difference between **Loan Header** and **Loan Source**. For me, "header" was hard to understand — didn't land. You could say "loan details" and "by source." Header is your individual information, source is the source breakout. **They have to match** because they're applied at the same time. The system won't let you push anything that doesn't match anyway — that's one of the errors you'll get.

I have the loans folder set up already. Made a small adjustment on that person's loan — I'll show you where.

These are what the input files look like that we're putting into Informatica. Pretty simple. We need to make sure these numbers get applied properly. Right now Matt's altershow doesn't do that.

This here is the **vendor code**. When we have our parameter file set up, that's why this needs to be exact — there's a lookup table that runs through to translate. "Paychex / loans / 5 dash half month equals biweekly for us" — that's where the parameter file kicks in. This is raw vendor data that needs to be translated to ours. **Current value has to match on both sides.** Has to match — not sure if case-insensitive, but needs to match one way or the other.

Glad I saw this — need to fix this too. Want to show you how we do this. Need to figure out how to break it out.

Now we have each loan one by one — 1, 2, 3. The way we number: you have all your people ordered however you want. As soon as it's tagged, it's tagged — Loan 1, 2, 3, 4, 5. Arbitrarily tagged, but once tagged, it's tagged. **This number needs to apply to Loan Source.** Right now it says "we've got these numbers, what loan does this go to?" Not sure yet. Need to figure out how.

Trying to remember how we do this. We're going to need just a little extra information not included. Going to be coming from... not from this one. From there. Need to open this report.

Where things get weird. We need to find a way — normally we like to see it itemized with loan numbers, but right now it's all aggregated. Both are aggregated, so it's hard to figure out for anyone with multiple loans how to apply it to each. Normally we'd tag SSN and loan number.

Did this with Stacy one time, can't remember. If you only have one loan, it's just going to be 1, or whatever's attached. Can't remember how we did it.

Okay, here's how we'll do it — manually. Not the best way. We have a current value of $11,904.13. These two numbers add up to $11,904.13, as does this — Loan 1. So for these two: this is 1, this is 1. When this gets set up, it'll add them together and check: does this equal the one tagged here? Yes — good.

Then 2 and 2. Next one: $197 is only one — he's 3 and 3. $227 only one — 4 and 4. $550 only one — 5.

Now a little math. **$1,307,853.** What number am I looking for? *(I bet there's a much better way of doing this. I'm going to write something to fix this — there is a way to do this better. You're just trying to find which combination adds up to which.)* If we just do the top ones — what are we looking for? 13 — boom. In order. 6 and 7, 6.

Last ones. 14 is 8. Now we're good. Ready for import. Save as a text file.

Forgot which one this is — the source one? Think it is.

Have to drop in a minute. Are you still doing Calidus? Got to get with Earl. Let me see what's over here first. He hasn't yet.

Need to fix this anyway — didn't enter the right numbers. Want to make sure we get this right. 550 — there's the right one. Nice.

We'll pick this up in a minute. This isn't financial, so it could be done late in the day — no financials attached.

---

## BD 4 — TMG Update

Update to the **TMG** issue. Ruby (account manager I know from TMG) and I worked through it.

Quick recap: looks like we needed years of service entered to get everyone's vesting right. But the weird thing is there were other people who were not at 0%. Really weird that some were flagged and some weren't. After we talk about what I'm going to do, the next thing is to figure out why some people were flagged and some weren't. **Hang on to that.**

Good thing: figured out a way to minimize our exposure. Massive thing. Good thing to remember in a job like this — probably any job, but with financials so easy to fix with the stroke of a pen, all of a sudden tons of money are moving around. We don't want that. We also don't want clients to get the wrong idea — like certain issues aren't issues. As a client you're thinking, "I don't know if I really want to take your word for it, buddy."

We were able to reduce our exposure as much as possible. Hopefully my proposal works: simply calculate years of service on our own without asking the client, and verify our numbers against the vendor data. Once I did that, **everyone we expected to be 100% turned out to be 100% on the vendor records.** Great news. We should be able to move forward with my proposal without letting the client know.

The only thing they know — we can't lie and say nothing happened — but we can reduce it down to just the one case. There are 200 people affected by this. Very important to reduce that down.

**Informatica came back up.** Was able to post everything. Having some troubles with the loans.

---

## BD 5 — End of Week

Not sure where we got cut off. Yeah, something weird with the loans — got to figure that out, but not a big deal. Can be done Monday. Good thing: participant balances are in there.

Having troubles with NBI at the moment — not sure what's going on. Probably won't be able to push my update out, but good thing is it's in there. It matches, so we can continue forward.

**Monday plan:**

- **TMG** — figure out what to do if it's not done today. I think it's not. Not financial, so we can do it late. Import years of service, pop that in. Hopefully that's the end of what we need to do for TMG. Really don't want to mess with that plan anymore. Probably not — hopefully yes.

- **Calidus** — figure out what happened with that loan thing. I'll play around with it more today. It did not load — maybe just didn't push right, but we'll figure it out. Easy. Elections on Monday with Beth. Then we're done with that plan, move on.

- **COCC** — going to resurface. Residual dividends coming in probably next week. Very, very touchy client — make sure this gets done right away. Easily done. Get prepared, apply when it arrives. Pretty low impact. Good one to work with Beth on, another good opportunity for screenshots.

Got a bunch of good screenshots now. Now that I've got the slide creator based on that worksheet, we'll have to pump that up. Another thing to make sure we do by end of week — really good tool. As long as we stick the information on there, we can turn it into an awesome slide deck really easily. I really want to do that. Want to tune that skill up — we can really do a good job of it.

That's what we got for work.

**Demo** — didn't get too far, kind of disappointing. Need to better prepare. Got put up against time — would have been able to do it with more prep, but got bogged down with work. Next time we'll do a better job presenting. They got an idea, know what to expect. First time is never the best anyway. Spend the time.

Going to learn more about Informatica — keep tapping Dave Slope. Going to keep tapping on Selina — she seems most open. Smith is too. Everyone's good. Everyone showed up who I invited, pretty cool.

Promise next time will be better. Want to keep on them — ask how things are going, how to fix the onboarding process. Going to shoot an email: "Hey guys, give me a quick thumb — how was it, what could be better, what's good, what's not." Incorporate this into our thing. Build out the system, make it awesome. Keep building it out.

Last brain dump of the week. Goodbye.
