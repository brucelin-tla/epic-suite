# Nightly Research — 2026-07-11

**Tonight's focus:** Ground Dan Martell's delegation/automation frameworks and Alex Hormozi's sales/offer/lead-gen frameworks against EPIC Suite's actual current code, to find the highest-leverage next steps for making real sales calls happen faster/better — and flag anything that's tool-building instead of revenue work.

This is the first run of this nightly routine — no prior `research/` reports existed to build on.

## Key findings

### Repo state (read directly: CHANGELOG.md, index.html, live/index.html, setter/index.html, referral/index.html, success/index.html, admin/index.html, and `git log`)

1. **The Closer's core advertised feature — a live AI calling co-pilot — does not work on real calls.** The hub (`index.html`) markets Closer as "Live AI calling co-pilot — work the ranked queue, get the next line, qualify, handle objections, and close on the call." But in `live/index.html`, the AI co-pilot experience (suggested lines, BANT auto-fill, objection rebuttals) only exists on the **scripted demo path** — fake prospect, pre-baked audio clips (`live/index.html:995-1001`, `:1264`). Real leads get "a plain connecting/connected call-timer UI with **no scripted transcript**" (`live/index.html:1001`) and the live-listening/transcription pipeline is explicitly labeled "Prototype — simulates the live-listening loop for the team demo. Real version: Twilio audio → Deepgram → Claude → this UI" (`live/index.html:568`) — i.e. not built.

2. **Real integrations that DO work today, and are genuinely revenue-relevant:** Calendly booking is real and live for Setter, Referral, and Closer (real availability, real cancel/reschedule via Supabase Edge Functions `calendly-book`/`calendly-cancel`/`calendly-availability`), and Stripe payment links are real with webhook-confirmed paid status auto-marking deals Won (`live/index.html:1794-1823`, CHANGELOG 1.19.0-era entries). Leads/prospects are pulled from real Supabase data, not demo names, once signed in.

3. **No speed-to-lead automation exists anywhere in the funnel.** Grepped Referral, Setter, and Admin for any notification/alert/push/SMS trigger on a new lead or referral landing — none found. A new referral or setter-qualified lead sits in a queue until a rep manually checks it; nothing pings a rep in real time.

4. **The engineering effort over the last ~2 weeks is heavily concentrated on the Epic Life Ledger, an owner-only tool, not the rep-facing funnel.** Of 50 total commits in the repo's history, 32 (64%) mention "Ledger" by keyword match; of the 141 commits touching `beta/` in the last 30 days, 25 touch `beta/ledger/` specifically (second only to `beta/live/`'s 36, which is the Closer). CHANGELOG.md shows ~19 point releases (b29 → b47) in under two weeks, all refining insurance-illustration math (chronic-illness rider mechanics, 401(k) glide paths, wizard UX, printable reports) for a tool gated to `profiles.role='owner'` — i.e., Bruce alone uses it. It is not part of the Referral → Setter → Closer → Client Success funnel this task is scoped to prioritize.

5. **Admin's commission/payout figures are explicitly still fake pending real data wiring:** "Commission figures elsewhere on this page are still illustrative pending live data wiring" (`admin/index.html:254`). Lower priority than #1 and #3 since it's internal reporting, not blocking a live call, but worth knowing before anyone makes a decision off those numbers.

### Dan Martell — delegation/automation frameworks (see full citations below)

6. **The Buyback Principle:** hire/automate to return time to the founder, not just to add headcount — "Don't hire to grow your business. Hire to buy back your time." *(Dan Martell, "Buy Back Your Time," Portfolio/Penguin 2023.)*

7. **The Buyback Loop** — audit low-value tasks → transfer them via documented playbooks → refill freed time with high-leverage work, and the resulting revenue growth funds buying back more time. *(buybackyourtime.com, corroborated by readingraphics.com / hireinsouth.com summaries.)*

8. **Effective Hourly Rate (EHR):** calculate your own $/hour value, then eliminate/automate/delegate anything paying below that threshold. *(Aggregated "Buy Back Your Time" summaries, shortform.com / hireinsouth.com.)*

9. I could **not** verify a Martell-specific claim about sequencing *software* automation (e.g., "automate lead intake before closing") — his documented sequencing (the "Replacement Ladder": Admin/EA → Delivery → Marketing → Sales ops → Leadership) is about human hiring order, and that ladder itself is only secondary-sourced, not confirmed against his own primary text. Do not treat "automate intake before closing" as a Martell quote.

### Alex Hormozi — sales/offer/lead-gen frameworks (see full citations below)

10. **Value Equation:** Value = (Dream Outcome × Perceived Likelihood of Achievement) ÷ (Time Delay × Effort & Sacrifice). *(Alex Hormozi, "$100M Offers," Acquisition.com 2021.)*

11. **Core Four lead-gen methods:** warm outreach, cold outreach, free content, paid ads — his stated sequencing for a service business is to start with warm outreach, layer in content, then scale to cold outreach/paid ads once repeatable. *(Alex Hormozi, "$100M Leads," Acquisition.com 2023.)*

12. **Speed-to-lead:** Hormozi has publicly cited reaching leads within 60 seconds correlating with dramatically higher close rates, and repeats a widely-cited "391% increase" response-speed statistic. **Attribution nuance:** that specific percentage traces to older third-party sales-response-time research (commonly attributed to Velocify/InsideSales-era studies) that Hormozi popularizes rather than originates — cite it as "a stat Hormozi cites," not his own data. *(Alex Hormozi, X post Jan. 2025, status 1879936850959474915; underlying stat via leadangel.com and other industry writeups, not independently primary-verified.)*

13. **C.L.O.S.E.R. framework** for the sales call itself: Clarify, Label, Overview (the pain cycle), Sell the vacation, Explain away concerns, Reinforce. Hormozi: "This is the exact flow I used to build sales teams in our portfolio companies." *(Alex Hormozi, X post, status 1537193249453985792; "The Game w/ Alex Hormozi" podcast Ep. 306.)*

14. **Triple A objection handling:** Acknowledge → Associate (with behavior of successful past clients) → Ask a re-engaging question. *("The Game w/ Alex Hormozi" podcast Ep. 810.)*

15. I found **no** clearly primary-sourced Hormozi framework specifically for "what a CRM/sales tool must do vs. dashboard theater," and no retention/churn framework as rigorously documented as C.L.O.S.E.R. or the Value Equation. Don't attribute either to him without a primary clip.

## Concrete next steps for EPIC Suite (ranked by what most directly speeds up closing real calls TODAY)

1. **Wire the Closer's AI co-pilot onto real calls — even a lightweight, text-only version.** Right now the tool's entire advertised value ("get the next line, handle objections") is unavailable the moment a rep is on an actual live call; it only fires for the fake demo. The full Twilio→Deepgram→Claude pipeline is a real build, but a much smaller first cut is available immediately: key the *existing* C.L.O.S.E.R.-style suggested-line and Triple-A objection-rebuttal logic (already built for the scripted path) off manual stage/BANT-signal taps a rep makes during a real call, instead of requiring live transcription. That gets Hormozi's actual framework (#13, #14) in front of reps on real calls this week, without waiting on the full audio pipeline.

2. **Add speed-to-lead alerting.** No new lead/referral triggers any notification today (finding #3). Given Hormozi's cited 60-second-contact stat (#12) and Martell's own first-automation principle being "own the inbox/calendar before anything fancier" (#6, #7), this is a small, high-leverage build: one more Supabase Edge Function (the pattern already exists for `calendly-book`/`calendly-cancel`) that fires an SMS or push to the assigned setter/closer the instant a `prospects` row is created. This is a same-scale build as what's already shipped for Calendly/Stripe, not a new subsystem.

3. **Finish wiring Admin's commission/payout numbers to real data**, since the page currently self-discloses they're illustrative (#5) — lower urgency than #1/#2 since it doesn't block a live call, but it's a landmine if anyone acts on those numbers believing they're real.

## Scope-creep flags

- **The Epic Life Ledger is the single biggest scope-creep risk found tonight.** It has absorbed the clear majority of recent engineering time (finding #4) — nearly 20 point releases in under two weeks — polishing insurance-illustration mechanics for a tool only Bruce himself can access. It is not part of the Referral → Setter → Closer → Client Success funnel and doesn't move a real lead toward a real close. This matches the exact founder pattern this routine was set up to catch: building/tuning software instead of doing the sales-facing work. Recommend explicitly pausing further Ledger polish until the Closer's real-call gap (next-step #1 above) is closed — Ledger's audience is one person; Closer's is the whole revenue-producing team, today.
- **Admin's "Building next" roadmap** (`index.html` ROADMAP object) lists a referral leaderboard, a testimonial builder, and cheat-sheet resources — all reasonable ideas, but all more tool-building/engagement features rather than anything that fills the calendar or closes a call. Worth deprioritizing behind #1 and #2 above.
- **Continued polish on the scripted/demo Closer call path** (pre-baked audio, keyboard shortcuts, training-mode features) is useful for onboarding new reps, but every hour spent there while the *real*-call path has no AI assistance at all is an hour not spent closing the gap that actually matters for live revenue calls.
