# Nightly Research — 2026-07-04

**Tonight's focus:** Dan Martell's automation/delegation frameworks + Alex Hormozi's sales/offer frameworks, applied to EPIC Suite's actual current codebase (first run — no prior `research/` reports exist yet).

---

## Key findings

### A. EPIC Suite's actual state (read directly from the repo, not assumed)

There is no `CLAUDE.md`, `README.md`, `SALES_COCKPIT_SPEC.md`, `prototype/`, or `supabase/` directory in this checkout — the repo has evolved past that layout. Actual structure: `index.html` (hub) → `referral/`, `setter/`, `live/` (Closer), `success/` (Client Success), each mirrored under `beta/` plus a `beta/admin/` panel. Root-level dirs are the **production/live** build the team currently uses; `beta/` is Bruce's review build (per `DEPLOY.md`'s own promotion process).

1. **The live (production) Closer is 9 versions behind beta and is still a fully scripted demo.** `live/version.json` = `1.19.2` (last touched 2026-07-01); `beta/live/version.json` = `1.28.0` (touched 2026-07-03). Grepping `live/index.html` shows its only Supabase call is the beta-only suggestion-box widget — no auth, no real lead query. Meanwhile `beta/live/index.html` has a full `supabase-js` client, real sign-in, and pulls real prospects. Per the commit log, everything shipped in beta since v1.20.0 — real sign-in, real leads in Setter/Closer, real Stripe/Calendly book-and-charge, commission system, admin + Fathom panels — is sitting unpromoted. This is a large amount of already-built, real functionality that the team is **not using today** because it hasn't been promoted, which is precisely the "build instead of ship/sell" pattern Bruce has flagged in himself.

2. **As of today's commit (`4b0ffe1`), real leads now flow into the Closer queue — but the AI co-pilot is deliberately switched off for them.** `beta/live/index.html` lines 885–945: a same-day code comment explains that running the existing scripted demo call (fake "Renee" audio + auto-filled BANT) against a *real* prospect would put fabricated dialogue in a real person's mouth — an "owner-flagged display-truth risk." The fix shipped today (`runRealCall()`) is a bare connecting/call-timer UI with **no transcript, no BANT auto-fill, and no CLOSER co-pilot** — the rep tracks everything by hand while talking. The valuable AI-suggests-your-next-line CLOSER co-pilot (lines 1312–1331) only fires in demo mode.

3. **The already-built Closer co-pilot already implements Hormozi's C.L.O.S.E.R. framework near-verbatim** (`beta/live/index.html:1320`: system prompt literally walks Clarify → Label → Overview → Sell → Explain → Reinforce) and the Setter tool already gates a "qualified booking" on BANT Budget+Authority (`beta/setter/index.html:168-283`) before it counts for commission. This is good news: the *design* is already aligned with the frameworks researched below. The gap is promotion and live-call plumbing, not framework redesign.

4. **Client Success (`success/`, `beta/success/`) is a one-time onboarding checklist**, not an ongoing retention engine — no activation-point tracking, no at-risk/churn flags, no re-engagement cadence.

### B. Dan Martell — Buyback Principle / delegation-automation framework

- **Buyback Principle**: hire/automate to buy back time rather than save money; pair with the **Buyback Rate** (annual income ÷ ~2,000 hrs ÷ 4) as a hard dollar threshold for "automate or delegate this." [Buy Back Your Time](https://www.buybackyourtime.com/) · [Buyback Rate](https://www.agencymastery360.com/podcast/buy-back-your-time)
- **Buyback Loop**: Audit (15-min task/energy audit) → Transfer (hire/automate/delete) → Fill (with high-value work). [Teamly Ch.4 summary](https://www.teamly.com/blog/buy-back-your-time-by-dan-martell-chapter-4/)
- **Buyback Matrix (DRIP)**: plot tasks on Energy × Money — Delegation (low $/low energy, automate first), Replacement (high $/low energy — e.g. onboarding, selling logistics), Investment, Production (protect — the Genius zone). [Four Quadrants](https://risewithdrew.com/the-four-quadrants-of-time-how-to-maximize-energy-and-income-with-dan-martells-buyback-matrix/)
- **Replacement Ladder** (delegation sequence): EA/Admin → Delivery → Marketing → Sales → Leadership — repeatable/draining work first, judgment-heavy work (sales, leadership) later. [Replacement Ladder](https://risewithdrew.com/dan-martells-replacement-ladder-how-to-buy-back-my-time-so-i-can-focus-on-what-energizes-me/)
- **What to automate first vs. keep human**: automate scheduling, inbox triage, initial lead qualification, follow-up sequences; keep closing and leadership judgment calls human, since AI "may not replicate the nuanced understanding and empathy" those need. [Summary](https://www.hireinsouth.com/post/buy-back-your-time-a-book-summary-of-dan-martells-take-on-reclaiming-freedom)
- **3-Level AI Agent Framework**: Level 1 (founder manually using tools) → Level 3 (one orchestrator agent coordinating narrow sub-agents, e.g. an inbox agent that handles routine replies but escalates judgment calls to the human). [3-Level Framework](https://marketingagent.blog/2026/04/09/tutorial-dan-martells-3-level-ai-agent-framework/)
- **Sell By Chat Playbook**: systematize opens/qualifying/follow-up, keep tailored offers and closing human. [Sell By Chat Playbook](https://www.danmartell.com/resources/sell-by-chat-playbook-2/)
- *(No verifiable source found for a "$110k Agreement" framework — dropped as unconfirmed rather than guessed.)*

### C. Alex Hormozi — sales/offer frameworks

- **Value Equation** (`$100M Offers`): Value = (Dream Outcome × Perceived Likelihood of Achievement) ÷ (Time Delay × Effort & Sacrifice) — maximize the numerator, shrink the denominator; it runs on *perception*. [Summary](https://www.beltcourse.com/blog/summary-of-100m-offers-how-to-make-offers-so-good-people-feel-stupid-saying-no-by-alex-hormozi)
- **Core Four + Rule of 100** (`$100M Leads`): warm outreach, free content, cold outreach, paid ads; commit to 100 units/day of one method for 100 days. [Summary](https://www.gregfaxon.com/blog/100m-leads-summary)
- **C.L.O.S.E.R. call framework**: Clarify → Label → Overview → Sell (the outcome, not the mechanism) → Explain away objections → Reinforce the decision. [Hormozi's own post](https://www.facebook.com/ahormozi/posts/the-closer-frameworkc-clarify-why-theyre-herel-label-their-problemo-overview-the/686013167842196/)
- **AAA objection handling**: Acknowledge → Associate with a relatable past success → Ask for the sale again. [3 A's](https://www.jointhefollowup.com/p/hormozis-3-a-sales-framework)
- **Guarantee stacking** (4 stackable types: unconditional, conditional, anti-guarantee, performance-based) + scarcity/urgency stacking. [Guarantee types](https://marloyonocruz.com/2026/05/18/book-notes-100m-offers-by-alex-hormozi/)
- **Retention engineered into the offer**: find the customer's "activation point" and drive them there fast; ~14-tactic churn checklist (onboarding, 1:1 over 1:many, proactive touchpoints). [Churn checklist](https://www.skool.com/acceleratoruniversity/churn-checklist-alex-hormozis-14-strategies)
- **Fast BANT qualification + setter "edify"**: qualify at intake, re-confirm on the call before ever asking for the sale; the setter's job includes pre-building the closer's credibility ("edify") before the prospect ever talks to them. [Notes](https://naveens-organization-10.gitbook.io/naveennotes/alex-hormozi-notes/hormozis-pre-sales-opening-and-closing-playbook)

---

## Concrete next steps for EPIC Suite (ranked by speed-to-closing-real-calls-today)

1. **Feed the existing CLOSER co-pilot from a real live transcript instead of manual typing.** The AI co-pilot logic (CLOSER-stage detection, next-line suggestion, BANT/BANK inference) is already built and already grounded in Hormozi's framework — it just has no input on real calls as of today's commit. Wire the Web Speech API (`webkitSpeechRecognition`, already used in the demo's mic-input feature per the changelog) into `runRealCall()` so it transcribes the rep's actual call and feeds that transcript into the same co-pilot prompt already in production code. This uses only the *real* words spoken — it doesn't reintroduce the fabrication risk that caused today's rollback — and it is the single highest-leverage change: real leads are already flowing in, and reps are currently working them with zero AI assist.
2. **Promote beta to live this week, specifically playtested on the real-call path.** Real sign-in, real leads, real booking/payment are sitting in beta since ~July 1–3 while the team's production tool is a stale scripted demo. Following the existing `/promote` process, verify the real-lead → real-call → real-payment path end to end (not just click-paths) before promoting, per the repo's own review gate — but don't let review sit indefinitely while the team keeps working off (or around) the old build.
3. **Carry the Setter's qualifying notes into the Closer's queue as an "edify" brief.** Setter already gates a qualified booking on BANT Budget+Authority; the `prospects` table already has a `priority_reason` field the Closer queue reads. Populate it with what the setter already established (Hormozi's setter-edifies-the-closer pattern) instead of leaving the closer to re-discover it — small, uses existing infrastructure, no new tool.
4. **Guarantee/scarcity language on the close-and-pay screen.** BANK-personality coaching text already references guarantees ad hoc; systematizing a guarantee/scarcity stack at the actual payment moment is a small, well-scoped addition to an already-built screen.
5. **(Lower priority) Client Success retention tracking** — activation-point + at-risk flags per Hormozi's churn framework. Real gap, but several steps removed from "close more calls today."

---

## Scope-creep flags

- **Item 5 (Client Success retention engine) is the clearest scope-creep risk tonight.** It's a legitimate framework-backed gap, but building it before #1 and #2 ship would repeat Bruce's known pattern — more tooling instead of getting the *already-built* real-lead pipeline in front of the team on actual calls.
- **The Fathom call-recording/admin-sync work (already partly built in `beta/admin/`) is post-call analysis, not on the critical path for today's calls.** Worth finishing since it's mostly done, but it shouldn't absorb time that belongs to #1 (live co-pilot) — recording and reviewing calls after the fact doesn't help the rep close the call happening right now.
- **Any further polish on Closer itself (more email templates, deeper BANK-personality detection, etc.) is polish on an already-strong tool.** The leverage this week is in promotion + live-call plumbing, not incremental Closer features — resist the pull to keep refining a tool that's already good in beta while it sits unused in production.
