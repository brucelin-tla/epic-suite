# Nightly Research — 2026-07-07

**Tonight's focus:** Apply Dan Martell's automation/delegation frameworks and Alex Hormozi's sales/offer frameworks to EPIC Suite's actual current codebase, to find what most directly speeds up real closing calls.

---

## Key findings

### What EPIC Suite actually does today (read directly from the repo, not assumed)

All five sales-facing tools (`referral/`, `setter/`, `live/` [Closer], `success/`, `admin/`) are client-side HTML/JS backed by a real Supabase project (`widdgynledwpqaqndtvz.supabase.co`); the edge functions they call (`calendly-book`, `submit-referral`, `sales-hub-sync`, etc.) aren't in this repo but are clearly real and deployed, confirmed by working call sites and the changelog history (`CHANGELOG.md`).

**Real and working end-to-end:**
- **Setter and Closer both pull a live lead queue** from Supabase `prospects`/`bookings` (real Sales Hub CRM sync, ~1,000+ leads per `setter/index.html:289`), falling back to demo data only when empty.
- **Setter logs every disposition** to real `call_logs`/`follow_ups` tables (`setter/index.html:427-431`).
- **Calendly booking is real** across Setter, Referral, and Closer (`calendly-book`/`calendly-cancel`/`calendly-availability` edge functions, e.g. `live/index.html:1062-1123`).
- **Referral submission and routing (warm→Closer, cold→Setter) is real**, server-tagged, not simulated (`referral/index.html:403-421`).
- **Payments are real Stripe Payment Links**, with a webhook-fed status poll as a nice-to-have and manual "mark Won" as the documented fallback (`live/index.html:1724-1779`) — the code itself flags the webhook backend as unconfirmed-deployed.
- **The Closer AI co-pilot genuinely calls `api.anthropic.com/v1/messages`** for next-line coaching (`live/index.html:1509-1529, 2049`) — but only if the rep has manually pasted a personal Anthropic API key into `localStorage` (`live/index.html:1559-1567`); without one it falls back to a hardcoded scripted flow.

**UI-only / simulated, despite looking real:**
- **Client Success (`success/index.html`) is almost entirely fake**: a hardcoded `MEMBERS` array, no Supabase read of real client data, onboarding checklist state never persisted — resets on reload.
- **The Closer's "text the prospect" feature is not SMS.** `phSendRep()` just appends to a local chat thread and toasts "Text sent" — there is no SMS API anywhere in the file. Worse: the "AI reply" the rep sees (`live/index.html:2039-2057`) is Claude **role-playing as the prospect** for rep training — in real-lead mode this is indistinguishable in the UI from a genuine reply, which is actively misleading if a rep doesn't realize it's a rehearsal, not the real customer.
- **Live-call transcription is an acknowledged prototype** — the app's own hint text says so: *"simulates the live-listening loop for the team demo. Real version: Twilio audio → Deepgram → Claude → this UI"* (`live/index.html:568`).
- **The Closer never writes call outcomes back to Supabase.** No inserts/updates against `prospects`, notes, or a call-log table exist in `live/index.html` — session stats (dials, connects, notes, BANT tags) are local-only and reset on `resetDemo()`. This is the sharpest contrast in the suite: Setter (a lower-stakes tool) persists every touch; Closer (the highest-stakes tool, carrying the actual revenue conversation) persists nothing.

**Biggest structural gap:** the Closer tool is the least durable of the five. Booking and payment links are real; the record of *what happened on the call* evaporates.

### Dan Martell — automation/delegation frameworks

- **The Buyback Loop**: Audit time → Transfer the task to a person/process/tool → Reinvest the reclaimed time in higher-value work. [donnapro.com](https://donnapro.com/delegation-leadership/buy-back-your-time/)
- **The DRIP Matrix**: categorize tasks by dollar value × energy drain — low-value/low-energy tasks get deleted or delegated first; high-value/low-energy tasks get systematized (this is automation's sweet spot). [donnapro.com](https://donnapro.com/delegation-leadership/buy-back-your-time/)
- **The Replacement Ladder**: delegate in order **Admin → Delivery → Marketing → Sales → Leadership**; within Sales, what gets delegated/automated first is "Calls & Follow-up," i.e. the mechanical logging and follow-up cadence around a call — not the call itself. [risewithdrew.com](https://risewithdrew.com/dan-martells-replacement-ladder-how-to-buy-back-my-time-so-i-can-focus-on-what-energizes-me/)
- **Explicit human-vs-AI line for sales**: *"Sales is a very human process, and what's going to differentiate you from everybody else is your humanity... Use AI to do all the other stuff."* Martell's own example: AI pre-qualifies leads by phone/chat before they ever reach a human's calendar — "AI is like a spam filter for your calendar." [Coconote summary of Martell content](https://coconote.app/notes/a8cfef17-7572-4adf-9837-2fdfa8c71596)

**Applied to EPIC Suite:** Setter already matches this pattern well (mechanical qualification + real booking, logged). Closer inverts it — the "automation" that got built (AI-roleplay texting, scripted co-pilot lines) touches the *human relationship* part, while the *mechanical* part Martell says to automate first (logging the outcome, triggering the follow-up) was never built at all.

### Alex Hormozi — sales and offer frameworks

- **Speed to lead**: contacting a lead within 60 seconds correlates with a **391% increase in likelihood of closing**; a business owner who reached 100% of leads within 60 seconds hit a **55% close rate**. [Alex Hormozi on X](https://x.com/AlexHormozi/status/1879936850959474915)
- **"The money is in the follow-up"**: Hormozi's stated rule is to contact leads **5–7 times** before giving up; his own data shows campaigns with 3–5 follow-ups get an **8.3% reply rate vs. 4.1%** without. [The Follow Up](https://www.jointhefollowup.com/p/alex-hormozis-closer-sales-framework)
- **The CLOSER framework** (call structure): **C**larify why they're here → **L**abel the core problem → **O**verview past attempts/pain → **S**ell the vacation (future-pace the outcome, not the product) → **E**xplain away concerns (agree, redirect, dissolve) → **R**einforce the decision, including **BAMFAM** ("Book A Meeting From A Meeting" — never end a call without the next step scheduled). [Skool — Accelerator University](https://www.skool.com/acceleratoruniversity/what-is-alex-hormozis-closer-framework)
- **Value Equation** (offer construction): `Value = (Dream Outcome × Perceived Likelihood) / (Time Delay × Effort)` — maximize the dream outcome and the prospect's belief they'll achieve it; minimize how long it takes and how much effort/hassle is required. [Primoz Bozic](https://www.primozbozic.com/value-equation/)
- **Risk reversal / guarantees**: a strong, specific (ideally conditional) guarantee removes the prospect's real fear — not losing money, but making a *bad decision* — and Hormozi's data point is that stronger guarantees tend to *reduce* refund rates, not increase them, because they filter for serious buyers. [LinkedIn — guarantee framework summary](https://www.linkedin.com/posts/benjamin-davis-1010a821_alex-hormozis-offer-framework-is-to-make-activity-7425607970031443968-dMk9)

**Applied to EPIC Suite:** the CLOSER framework's final step (Reinforce/BAMFAM) is exactly what a not-closed disposition on a real call should trigger automatically — book the next touch before the rep moves to the next lead. That can't happen today because there's no outcome record to trigger off of. Hormozi's 5–7 touch follow-up data is the direct business case for fixing the Closer's missing persistence: every call that isn't logged is a lead that silently drops out of the follow-up sequence Hormozi's numbers say wins the deal.

---

## Concrete next steps for EPIC Suite (ranked by what most directly speeds up closing real calls today)

1. **Make the Closer write every call outcome to Supabase** — disposition, notes, BANT tags, Won/Lost, tied to the prospect row — mirroring what Setter already does (`setter/index.html:427-431`). This is boring, not AI-flavored, and is the single highest-leverage fix: nothing downstream (follow-up automation, retention data, Admin visibility into what Closers are actually doing) is possible without it.
2. **Fix or relabel the "text the prospect" feature.** Either wire it to a real SMS provider so reps can actually text real leads from the tool, or — much faster — clearly separate "🎓 Practice with AI" (roleplay, current behavior) from any real-lead messaging surface, so a rep on a real deal never mistakes an AI-generated reply for the prospect's actual words. This is a trust/correctness fix, not a feature build.
3. **Once #1 exists, auto-trigger the next follow-up touch on any non-Won disposition**, using the Calendly/Supabase plumbing that's already real — a direct implementation of Hormozi's proven 5–7 touch cadence and the CLOSER framework's BAMFAM principle, instead of leaving follow-up to rep memory.
4. **Verify (don't rebuild) the Stripe webhook backend.** The code already assumes manual "mark Won" as a fallback; a quick check of whether the webhook is actually deployed determines whether that fallback is the permanent path or a temporary one — this is a 15-minute verification, not a project.
5. **Give Client Success a real data read** (replace the hardcoded `MEMBERS` array, persist onboarding checklist progress). Lower priority than the Closer fixes since it's post-close, but it's the retention/referral loop's weak link — a member whose onboarding silently resets on reload is a worse first-90-days experience, and happy onboarded members are TLA's referral engine.

## Scope-creep flags

- **Building real Twilio/Deepgram live-call transcription is the highest-risk scope-creep item in this whole list.** It's the most technically interesting piece and the most likely thing to absorb engineering time for its own sake — but per Martell's Replacement Ladder, it automates something that isn't the bottleneck (a human closer doesn't need an AI to listen live) while the actual bottleneck (call outcomes vanishing) stays unautomated. Recommend explicitly deferring this.
- **More AI co-pilot polish (fancier next-line suggestions, more scripted flows) is an upgrade on a broken foundation.** Without persistence (#1 above), a smarter co-pilot just produces better advice that still evaporates when the tab closes. Sequence discipline matters: fix the database writes before investing further in the AI layer.
- **The Anthropic API key being pasted client-side into `localStorage` and called directly from the browser** (`live/index.html:1517-1567`) is a real architectural/cost-control gap worth a note — but building a full backend AI-proxy service is itself a multi-day project. A minimal Supabase Edge Function proxy (a few lines, reusing the pattern already used for `calendly-book`) would close this without becoming its own initiative.
- This report itself is research, not revenue work — the value only lands if item #1 above gets built, not from more nightly analysis. Future nights should default to re-checking what actually shipped before generating new frameworks to apply.

---

*Sources cited inline above. Repo facts confirmed by direct reading of `epic-suite` (this checkout, commit `808551d`) — file:line references included where useful for follow-up verification.*
