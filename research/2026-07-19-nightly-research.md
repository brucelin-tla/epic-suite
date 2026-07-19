# Nightly Research — 2026-07-19

## Tonight's focus

First run: apply Dan Martell's Buyback Principle/automation guidance and Alex Hormozi's sales/offer/lead-gen frameworks to EPIC Suite's *actual current code*, to find what most directly speeds up closing real calls today.

## Method note

Repo state was mapped by reading `CHANGELOG.md` and all five role apps (`referral/`, `setter/`, `live/`, `success/`, `admin/index.html`) directly — not guessed. Framework research used WebSearch/WebFetch; every claim below is cited, and claims that could only be corroborated via secondary summaries (not a primary Martell/Hormozi source) are explicitly flagged **moderate/low confidence** rather than stated as fact. Two specific circulating claims ("60-second speed-to-lead rule," "391%/21x" follow-up statistics commonly attributed to Hormozi) could **not** be traced to any primary source and are flagged unverified — do not build anything on top of them.

## Key findings

### A. What's actually real vs. demo in EPIC Suite right now

- **Referral Partner** (`referral/index.html`): fully real — Supabase-backed referrals/commissions/bookings, real Calendly edge functions. No demo fallback found.
- **Setter** (`setter/index.html:269-273,315,325`): hybrid — real Supabase queue and real booking, but silently falls back to a hardcoded `DEMO_LEADS` array whenever the live query returns zero rows or errors, and blocks writes on demo leads.
- **Closer / Live** (`live/index.html`): hybrid and most complex. `loadCloserQueue()` (`live/index.html:1013-1046`) pulls real bookings/prospects/referral deals from Supabase; Setter-booked leads were previously invisible to Closer until a pipeline-stage fix (`live/index.html:1018-1020`). Real leads get a bare manual-entry call mode; the polished "▶ Start call" experience (scripted audio, auto-BANT) is explicitly demo-only (`live/index.html:986-1003`) and never runs against a real lead.
- **Client Success** (`success/index.html:196-201`): **100% hardcoded demo.** `MEMBERS` is a static 4-person array; there is no `supabase.from()` call anywhere in the queue, 3B checklist, or onboarding checklist. No real member — closed today or a year ago — has ever appeared in this app.
- **Admin** (`admin/index.html:162,390-425`): hybrid — team/role management, invites, and the payout tracker are real Supabase reads; but the dashboard's funnel, boards, and interaction metrics are hardcoded under a comment literally titled `/* ===== DEMO DATA ===== */`, with a visible `<span class="demo">Demo data</span>` badge.
- **Auth** is real everywhere (per-user Supabase magic-link/Google sign-in replaced shared demo codes — confirmed in `CHANGELOG.md` and each app's sign-in gate).
- **Payments/booking**: Calendly and Stripe links are real and live-mode-aware. A `payment-status` poll (`live/index.html:1803-1831`) can auto-fire "Won" on a confirmed Stripe payment, with an explicit fallback comment that manual marking still works if the backend poll isn't deployed (`live/index.html:809`).

### B. The single load-bearing gap (confirmed by direct code read)

`logAndNext()` (`live/index.html:1451-1461,1078-1085`) only writes a commission/deal record to Supabase via the `close-deal` function when the queue item is a **referral-sourced** deal (`REAL_DEALS[curId]` exists). For the more common case — a lead sourced from Sales Hub or booked by a Setter — marking 🏆 Won, whether auto-confirmed by the Stripe webhook poll or marked manually, **only updates local UI counters** (`live/index.html:1826-1828`). Nothing is persisted. The sale is invisible to the Admin payout tracker and to every other role the moment the closer's browser tab closes or refreshes. This means the app can already run a real lead through a real call, a real booking link, and a real payment link — and then silently lose the deal at the finish line for the majority of closes.

### C. Dan Martell — Buyback Principle / automation framework

- **Buyback Loop**: Audit (find low-value/draining tasks) → Transfer (hand to a person, process, or tool) → Fill (reinvest the freed time into higher-value work — required, not optional). Source: *Buy Back Your Time*, via [dorinblaga.com](https://dorinblaga.com/the-buy-back-loop/).
- **DRIP Matrix** (2×2 of money value × energy): **D**elegation (low money/low energy → hand off or automate first), **R**eplacement (high money/low energy → next to hand off via system or hire), **I**nvestment (low money/high energy → keep, it's growth), **P**roduction (high money/high energy → your zone of genius, keep and do more). Source: [risewithdrew.com](https://risewithdrew.com/the-four-quadrants-of-time-how-to-maximize-energy-and-income-with-dan-martells-buyback-matrix/).
- **Replacement Ladder** (delegation order): Executive Assistant → Delivery → Marketing → Sales → Leadership. Source: [risewithdrew.com](https://risewithdrew.com/dan-martells-replacement-ladder-how-to-buy-back-my-time-so-i-can-focus-on-what-energizes-me/).
- **AI in sales, explicit stance**: sales stays a human process — AI drafts/assists, a human delivers human-to-human; closing conversations stay human. Source: danmartell.com, "How to Get So Many Customers with AI It Feels Illegal" (direct fetch blocked 403; via search summary, **moderate confidence**).
- Lead qualification named as "the number one constraint" across his portfolio companies, with AI voice-agent qualification recommended as the automatable layer. Same source, **moderate confidence**.
- No verifiable primary-source quote matching the commonly-repeated "you don't need another system, you need to make the calls" line — flagged **unverified**, not asserted as fact.

### D. Alex Hormozi — offer, lead-gen, sales, retention

- **Value Equation**: Value = (Dream Outcome × Perceived Likelihood of Achievement) ÷ (Time Delay × Effort & Sacrifice). Source: *$100M Offers*.
- **Core Four** lead channels: Warm Outreach, Content, Cold Outreach, Paid Ads. **Rule of 100**: sustain 100 outreach contacts/day, 100 minutes of content/day, or $100/day ad spend. Source: *$100M Leads*.
- **CLOSER framework** (Clarify → Label → Overview → Sell → Explain → Reinforce): confirmed via Hormozi's own X/LinkedIn posts, cross-referenced by secondary breakdowns. **This is already implemented almost verbatim** in `live/index.html:1559` (`closerCoach`'s system prompt) and the six-stage `CLOSER_STAGES` UI — the app's coaching logic is already Hormozi-framework-native.
- **Triple-A objection handling**: Acknowledge → Associate (story of a similar past buyer) → Ask (for the sale again). Source: *The Game w/ Alex Hormozi* podcast, Ep. 810.
- **Retention**: aim for <~3% monthly churn (*$100M Money Models*, via secondary summary — primary text not directly accessed); churn is "100 tiny things," not one fix — onboarding activation is the primary lever, and live/personal onboarding beats generic/recorded (*$100M Leads*/*$100M Playbook: Retention*, via secondary summary).
- **Unverified, do not build on**: the widely-circulated "60-second speed-to-lead," "391% increase," "21x within 5 minutes," "80% drop after 5 min" statistics could not be traced to any primary Hormozi source.

## Concrete next steps for EPIC Suite (ranked by how directly they speed up closing real calls TODAY)

1. **Fix `logAndNext()` to persist non-referral closes.** Wire the same `close-deal` Supabase write that already runs for `REAL_DEALS` to fire for ordinary Sales-Hub/Setter-booked leads too. This is a surgical fix to code that's already 90% built (`live/index.html:1451-1461`) — not new infrastructure. Every day this isn't fixed, closed revenue on non-referral deals is being generated and then thrown away the moment a browser tab closes.
2. **Wire Client Success's queue to real Supabase data.** Right now `success/index.html` runs entirely on 4 fake names — meaning even after fix #1, zero real member ever reaches the retention/onboarding loop. Hormozi's own research above says retention is won or lost in onboarding activation; today that stage of EPIC Suite has literally nothing real running through it. Scope this narrowly: read the same `bookings`/deal tables Admin already reads, don't redesign the CS UI.
3. **Make the manual `closerCoach` note-typing flow the default for every real-lead call**, not a hidden fallback the rep has to discover. It already implements Hormozi's CLOSER stages and can surface Triple-A-style objection reframes (`OBJ_BANK`, `live/index.html:1577-1585`) — the coaching value Hormozi's frameworks promise is already coded, it's just not in the primary path for real calls.
4. **Replace or clearly gate the Admin dashboard's hardcoded "Demo data" panel** (`admin/index.html:390-425`) so Bruce isn't making decisions off fake funnel/interaction numbers. Wire only the 2-3 tables already used elsewhere (bookings, commissions, call_logs) — do not build a new BI layer.
5. **Verify (don't build) that a booked-but-not-yet-closed lead gets automatic follow-up**, per Martell's Delegation quadrant (this is a low-energy, should-be-automated task) and Hormozi's "the money is in the follow-up." If it's missing, this is a small cron/edge-function addition, not a new system.

## Scope-creep flags

- **Twilio → Deepgram → Claude real-time live-call transcription** (`live/index.html:568`, explicitly labeled a simulated prototype) is the most tempting "real" build here, and the one most likely to become a multi-week infra project that produces nothing closeable this week. Per Martell's Buyback Loop and Bruce's own known pattern, this should stay parked until items #1–#3 above are shipped and actually being used by a closer on a real call — the manual note-typing coach already gets most of the value without touching telephony infrastructure.
- **A full Admin analytics/BI rebuild** is a natural-feeling next step once the "Demo data" badge is visible, but the actual need is narrower: stop showing fake numbers, using tables that already exist. Resist redesigning the dashboard.
- **Any new AI feature layered on top of the coaching UI** (smarter prompts, more automation) before fix #1 ships is compounding the invisibility problem — a smarter closer coach that still fails to log the sale is strictly worse for revenue visibility than a dumber one that logs correctly.
