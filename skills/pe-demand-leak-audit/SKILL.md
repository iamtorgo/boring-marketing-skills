---
name: pe-demand-leak-audit
description: For advisors and operating partners. Audits several brands of a PE-backed local-services platform (HVAC, plumbing, electrical, roofing, pest) from the outside, finds where each brand leaks demand on Google and on its first screen, and rolls the findings into platform-level fixes a sponsor can deploy once across every brand and every future add-on. Ends with a paid-pilot proposal and a pitch-the-fix email to the sponsor.
when_to_use: audit a roll-up, private equity roll-up, PE-backed platform, pitch the PE sponsor, audit a home services platform, demand leak audit, PE portfolio marketing audit, operating partner pitch, buy-and-build marketing, audit these portfolio companies, value creation marketing audit, pre-close marketing diligence on an add-on
argument-hint: "[platform name + sponsor | list of brand names and towns]"
metadata:
  boring:
    version: 4.1.1
    layer: growth
    reads: [positioning#chosen-angle, voice-profile, learnings]
    writes: []
    optional-keys: [APIFY_TOKEN]
    composes: [first-screen-check, google-business-profile, reviews, speed-to-lead, pitch-the-fix]
---

# PE Demand Leak Audit

A sponsor doesn't buy a fix for one plumber. It buys a fix it can deploy once across 12 brands and every add-on it closes next year. This skill runs the library's single-business audits on three brands of one platform, then rolls up what repeats into platform plays, a priced pilot, and one email to the sponsor. Done means a scoreboard, one lead leak per brand, three to six platform plays, a pilot table with baselines, and an email that passes `pitch-the-fix`'s rubric.

## Phase 1: Pick the platform and three brands

- **Platform:** PE-backed, multi-brand (acquired brands keep their names), 5 to 30 add-ons, lower-middle-market sponsor. Sources: sponsor press releases, PrivSource/PitchBook add-on lists, trade press.
- **Brands:** three residential brands in different towns. Prefer one big-review brand, one mid, one small; the contrast makes the roll-up argument.
- **Centralization signals:** shared website template (same banner wording or web vendor across brands), a shared-services or call-center job posting, a shared CRM or booking vendor. These decide which fixes roll out cheaply. Record each with its URL.

## Phase 2: Per-brand audit (one pass each)

For each brand, run the checks from these skills at their lightest depth:

1. `/google-business-profile`: primary category vs the top 3 for "[main service] [home town]", photos count.
2. `/reviews`: rating, total, new reviews in the last 90 days, owner replies in the last 90 days, unanswered 1-2 star reviews.
3. `/first-screen-check`: number with tap-to-call, the real rating on the first screen, a working way to book, promises that contradict each other (24/7 vs hours), license numbers that disagree.
4. Map pack: is the brand in the top 4 for its home search? Record who is, with rating and count.

With Apify connected: `compass/crawler-google-places` for profiles and packs, `compass/google-maps-reviews-scraper` with `reviewsStartDate: "90 days"` for pace and replies. Pull only the fields you need (`title,stars,responseFromOwnerText`) to keep context small. Without it, read the profiles by hand.

Pick **one lead leak per brand** with the four tests from `pitch-the-fix` (seen in ten seconds, loses customers, fixable in days, provable in 30 days).

## Phase 3: Roll up to platform plays

A play qualifies when the leak repeats across at least two brands **and** a centralization signal lets it ship once. Typical plays:

| Play | Ships through |
|---|---|
| Review reply desk, 48-hour SLA, owner voice per brand | Shared CSR team |
| Real rating on every first screen | Shared site template |
| Category and services sweep, one brand at a time | Central marketing |
| One rule for 24/7, hours, booking links, license numbers | Shared template |
| Copy the best brand's review-ask to the slow brands | Field ops / CRM |
| Cross-sell routing (HVAC reviews mentioning plumbing) | Shared CSR team |

Add an add-on line: the same checklist runs in week one of every new acquisition's 100-day plan.

## Phase 4: Pilot and proof

Three to four fixes, three brands, 30 days, fixed fee. Every row carries a baseline, a proof source the sponsor already trusts (Google profile, homepage calls, rank grid) and a window. Price is the user's; leave a placeholder.

## Phase 5: The email to the sponsor

Use `/pitch-the-fix` with these changes: the recipient is the sponsor partner or platform CEO (confirm from SEC Form D signatories, press-release quotes, the sponsor's team page); the one thing is the brand with the biggest number; the fix names the shared resource that ships it. Never auto-send; never a guessed address.

## Output

One document (a doc or `./campaigns/pe-{platform-slug}/audit.md`):

1. Summary: four bullets, the strongest numbers first.
2. Scoreboard table: brand, rating, total reviews, 90-day new, 90-day replies, in home top 4?, rating on first screen?
3. Brand findings: one lead leak per brand, then two or three supporting points.
4. Platform plays, numbered.
5. Pilot table.
6. Outreach: recipient and how found, email 1, two follow-ups, what to fill in.
7. Method, caveats (rank is location-dependent; scrape caps; page content vs phone render), sources with URLs.

## What's next

- `/speed-to-lead` once the sponsor shares call logs (the biggest leak is usually unanswered calls).
- `/landing-page` when the lead leak is a page.
- Repeat for the sponsor's next platform.
