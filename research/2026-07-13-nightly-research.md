# Nightly Research — 2026-07-13

## Tonight's focus

Dan Martell's automation/delegation frameworks and Alex Hormozi's sales/offer/lead-gen frameworks, applied to EPIC Suite's actual current codebase (first run — no prior reports exist yet).

## Repo grounding (read before researching)

The repo layout doesn't match the prompt's assumed file names (no `CLAUDE.md`, `README.md`, or `SALES_COCKPIT_SPEC.md`, no `supabase/` folder) — the actual structure is a static GitHub Pages site with per-role apps at root: `referral/`, `setter/`, `success/`, `admin/`, `ledger/`, and `live/` (this last one **is** the Closer app — CHANGELOG 1.19.0 records the rename off "cockpit"; `DEPLOY.md` is stale and still references the old `cockpit-demo` naming/paths).

What's actually real, not mockup:
- All role apps are Supabase-auth-gated (`widdgynledwpqaqndtvz.supabase.co`) with real tables (`deals`, `prospects`, `bookings`, `call_logs`, `commissions`, `referrals`, etc.), not static HTML demos.
- `referral/` and `success/` are functionally complete and promoted (root/beta byte-identical).
- `live/` (Closer) has **real Stripe payment links and real Calendly booking links**, but no payment-confirmation webhook — closers must manually mark deals "Won" today.
- `setter/` syncs real leads via `fetch`, but **silently falls back to a hardcoded `DEMO_LEADS` array** with only a toast notice whenever the live sync returns nothing or errors.
- `admin/`'s own footer admits commission figures are "illustrative pending live data wiring."
- Of the 20 most recent CHANGELOG entries, **19 are `ledger/` (Epic Life Ledger) releases** — an owner-only wealth-illustration calculator whose own footer states it's "illustrative... [replace] before using with a client," i.e., self-described as not client-ready output. Only one entry in that window touched the sales-cockpit roles.

## Key findings

**Dan Martell — Buyback Loop & DRIP Matrix.** Martell's core framework is the Buyback Loop: **Audit** your time/tasks, **Transfer** low-value ones off your plate, **Fill** the freed space with high-leverage work ([donnapro.com](https://donnapro.com/delegation-leadership/buy-back-your-time/); [buybackyourtime.com](https://www.buybackyourtime.com/)). He sorts tasks into a 2×2 **DRIP Matrix**: Delegation (low money/low energy — administrative, repetitive), Replacement (high money/low energy — valuable but no longer energizing), Investment (low money/high energy — growth activities), Production (high money/high energy — the real leverage work) ([familyebiz.com](https://familyebiz.com/059); [risewithdrew.com](https://risewithdrew.com/the-four-quadrants-of-time-how-to-maximize-energy-and-income-with-dan-martells-buyback-matrix/)). On AI specifically, his stated principle is to **automate the mechanical/routine work and keep humans in high-stakes, relationship-driven moments** — AI handles admin, follow-up, and monitoring so people can spend their freed time on the actual human connection and closing ([danmartell.com](https://www.danmartell.com/how-to-get-so-many-customers-with-ai-it-feels-illegal/)).

**Alex Hormozi — Value Equation, CLOSER, Core Four.** The Value Equation: `Value = (Dream Outcome × Perceived Likelihood of Achievement) / (Time Delay × Effort & Sacrifice)` — maximize the numerator, minimize the denominator ([alexhormozi.wiki](https://alexhormozi.wiki/frameworks/value-equation-explained); [creatoreconomy.so](https://creatoreconomy.so/p/the-value-equation-for-irresistible-products)). His sales-call structure is **CLOSER**: Clarify why they're here, Label their problem, Overview past attempts/pain cycle, Sell the outcome (not the mechanism), Explain away objections, Reinforce the decision ([salescoachpro.io](https://www.salescoachpro.io/blog/hormozi-closer-framework); [skool.com](https://www.skool.com/acceleratoruniversity/what-is-alex-hormozis-closer-framework)). His lead-gen model is the **Core Four**: warm outreach, content, cold outreach, paid ads — the only four channels a person or business has to generate leads ([shortform.com](https://www.shortform.com/blog/100m-leads-alex-hormozi/); LinkedIn/[Tambareni](https://www.linkedin.com/posts/harsha-tambareni_alex-hormozi-just-gave-us-the-core-four-lead-activity-7117637424171675648-1unI)). He also reports that 78% of his own clients had consumed at least two long-form content pieces before booking a call — content nurtures and pre-sells demand ahead of the human conversation ([shortform.com](https://www.shortform.com/blog/100m-leads-alex-hormozi/)).

## Concrete next steps for EPIC Suite (ranked by speed-to-impact on real calls)

1. **Fix Setter's silent fake-lead fallback.** A setter could be working `DEMO_LEADS` believing they're real prospects during a live session — this directly risks wasted or embarrassing outreach. Per Martell's DRIP Matrix this is a Delegation-quadrant task (low-complexity, mechanical) that should fail loudly, not silently — surface a blocking banner, not a toast, and page/alert admin on sync failure. Small fix, highest real-call risk reduction.
2. **Deploy the Stripe payment-confirmation webhook for `live/` (Closer).** Manual "mark as Won" is exactly the mechanical, low-energy, high-value task Martell says to automate first (Replacement quadrant) — and instant confirmation is also the concrete follow-through on Hormozi's CLOSER "Reinforce" step (the moment of commitment should feel immediate and certain, not pending on someone remembering to click a button later).
3. **Wire `admin/`'s commission figures to live data.** Team trust in the numbers they're being paid against is foundational — currently self-flagged as illustrative.
4. **Wire `success/`'s MyFICO/onboarding checklist to a persistent CRM record**, not just gated access — this is the retention leg of the funnel; losing that data breaks the loop Hormozi ties to long-term LTV and referral-worthy outcomes.
5. **Fix stale `DEPLOY.md`** (still says "Cockpit demo" / old paths). Five-minute doc hygiene fix, not urgent, but confusing to anyone onboarding onto the deploy process.

## Scope-creep flags

- **The Ledger dominance is itself the biggest flag.** 19 of the last 20 releases went into an owner-only internal calculator that explicitly disclaims client-readiness, while the Setter→Closer→Success funnel — the thing that actually gates real sales calls — got one cosmetic change in the same window. This matches Bruce's own known pattern of building tools instead of running the funnel. Recommend he explicitly ask: *has the Ledger been used live in a client meeting this week?* If not, further Ledger investment should pause until Setter/Closer reliability items above are closed.
- **Do not build an in-app CLOSER-step UI scaffold.** It's tempting (Hormozi's framework maps cleanly onto `live/`'s screen flow), but closers should already know CLOSER from training — adding UI for it is new tool-building, not revenue work, unless a closer specifically asks for it after using the current flow.
- **Leave `referral/` and `success/` alone otherwise** — both are functionally complete and promoted; further polish there without a specific complaint is scope creep.
