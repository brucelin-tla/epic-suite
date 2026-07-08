# EPIC Suite — changelog (newest first)

- **1.52.10** — **Epic Life Ledger (owner-only), b43 — BETA ONLY, not yet promoted**: one policy
  now does both jobs. The Accumulation IUL carries a built-in chronic illness rider, so the
  separate Protection IUL topic is retired entirely — same death benefit clears the debt AND is
  the chronic-illness pool, and because the policy is issued increasing (DB Option 2, verified
  against the carrier's own ledgers: net death benefit = face + policy value to the dollar),
  the cash value stacks on top of the face, growing both the coverage and the rider pool every
  year. New: ALIS computes the optimal increasing→level switch year (same resolve-and-rerun
  pattern as the loan-type pick; objective = the member's chosen goal; manual override + "never"
  available), shown with the honest delta vs. never switching. Recalibrated to six fresh 2026
  carrier illustrations of this exact design (30/45/60 × M/F, CI rider included, $50k/yr):
  new face-per-premium and target-premium tables, and the cost chassis re-fit on the Bronze
  no-Vitality scale — one COI scalar per sex (male ~70%, female ~57% of guaranteed max; the
  default now follows the sex toggle until the agent moves the slider), reproducing all six
  within 1–7% MAPE across every illustrated year, erring conservative early. Chronic-illness
  draws now cap at the real IRS per-diem limit ($430/day 2026, Rev. Proc. 2025-32 ≈ $13,079/mo —
  the old $30K/mo cap belonged to the retired product), the elected monthly rate defaults to the
  illustrated 1%, and every draw reduces the death benefit AND the cash value proportionately,
  per the contract. Estate math now credits the policy's real net death benefit (face + stacked
  CV − loans − rider draws) instead of just cash-value-net-of-loans. Also fixed from the
  contract: the overfunding (advance contribution) charge stops after policy year 20.

- **1.52.9** — **Epic Life Ledger (owner-only), b42 — BETA ONLY, not yet promoted**: the
  Accumulation IUL topic reorganized around what matters, in order: death benefit, total annual
  premium, premium expense charge, current cash value, the year's tax-loan, loan available
  (capped at 90% of cash value — the same never-lapse limit the retirement engine already
  enforced, now applied here too), net gain on the loan's interest rate, net gain in dollars, cash
  value remaining, tax bill out of pocket. Agent-only stats (enhanced target premium, year-1 cost
  of insurance, face-amount charges, overfunding charge, the $20/mo admin fee, and a new "Total
  AIUL policy expense" rollup) moved into one collapsed panel, no commission math spelled out. New
  premium-stop-age dropdown for the Accumulation IUL (current age → 120), matching Protection
  IUL's. Clarity fixes: the premium line says "Withholding not selected" outright when off; the
  loan's net cost/gain no longer uses a "negative means gain" convention. Verified headlessly incl.
  a 375px mobile pass: 0 real JS errors, no horizontal overflow, the 90%-cap loan fix and the
  stop-age engine gate both confirmed to have real, correctly-directional effects.

- **1.52.8** — **Epic Life Ledger (owner-only), b41 — BETA ONLY, not yet promoted**: Section 2
  rebuilt around the Protection IUL. Reordered to Protection IUL → Accumulation IUL → rental
  property → velocity banking (0% biz credit + HELOC folded in right after it). Protection IUL is
  now its own topic: coverage amount and premium as direct inputs, plus a new premium-stop-age
  dropdown (both here and on the Accumulation IUL). New Accelerated Death Benefit rider (terminal
  illness, one-time 30–70% lump sum, on by default — real carriers bundle it free) alongside the
  existing chronic-illness/LTC rider (now discrete 1/2/4%/mo instead of a slider); only one of the
  two can be active at a time since both draw the same coverage pool, and whichever is active now
  defaults to being claimed, not preserved. Renamed "Honest long-run" to "Conservative." Default
  legacy now genuinely reflects the new default-on ADB claim ($23,513,442 vs. the pre-ADB
  $23,913,002 — an intentional new-feature change). Verified: stress test, journey, and every prior
  dependency (HELOC↔mortgage, biz credit↔velocity) still work post-reorder; 0 duplicate element
  IDs; 0 JS errors.

- **1.52.7** — **Epic Life Ledger (owner-only), b40 — BETA ONLY, not yet promoted**: Section 1
  cleanup. Retirement age and the age to pass down the estate now live only in Section 3, where
  the retirement plan actually runs — removed the duplicate Section 1 sliders. The 401(k) blend
  explainer is now plain language. Removed three cluttered lines (years it contributes, average
  annual withdrawal, total withdrawn), kept effective tax rate, and added a real early-withdrawal-
  penalty line — which surfaced a genuine engine gap: the plan wasn't charging the actual 10% IRS
  penalty on qualified money touched before 59½, even though early retirement (as young as 40) has
  been supported since b15. Fixed at the engine level, not just displayed: threaded into the draw-
  sizing math so an early-retirement plan correctly draws more to net the same lifestyle. Verified:
  default (retire at 65) byte-identical; a 50-year-old retirement scenario now shows a real
  $201,775 penalty and correspondingly lower legacy; 0 JS errors.

- **1.52.6** — **Epic Life Ledger (owner-only), b39 — BETA ONLY, not yet promoted**: two things.
  (1) Clearer copy on "Or flip tools freely" (Section 3) and the generic tool-off hint — both now
  explain that this row is the exact same switch as each tool's own "In the plan" button, and that
  turning a tool back on resumes its real Section 1/2 numbers where they stand (today's mortgage
  balance, 401(k) balance, policy cash value), not a reset to zero — verified directly: toggling
  mortgage/policy off then back on via the Section 3 toolbar reproduces the exact real number
  again (e.g. $459,872.19 mortgage balance, $1,270,821.60 policy cash value), byte-for-byte. (2)
  The HELOC now depends on the mortgage tool being on, same pattern as 0% biz credit depending on
  velocity chunking — a HELOC needs an actual home mortgage in the plan to mean anything. Turning
  mortgage off greys out HELOC, relabels it "needs the mortgage," and makes it fully inert at the
  engine level (not just visually disabled) — verified `helocEnd`/`refiEnd` both go to exactly 0.
  Turning mortgage back on restores HELOC to whatever the member had it set to (its own TOOLS flag
  is never mutated by the dependency), confirmed byte-identical to the pre-toggle legacy number.
  Confirmed (not changed): 0% biz credit already has zero effect anywhere outside velocity
  chunking — no code change was needed there, just verification. Regression: default numbers
  byte-identical pre/post; 0 JS errors.

- **1.52.5** — **Epic Life Ledger (owner-only), b38 — BETA ONLY, not yet promoted**: the stress
  test's "with policy" vs. "no policy" result is now one side-by-side comparison table instead of
  two stacked cards — all 10 metrics as matched rows, both numbers next to each other, the actual
  winner colored green and the other red (a near-tie stays neutral rather than a false color).
  Purely a display change — same underlying numbers, same 10 metrics; verified the honest split
  still shows through (policy wins legacy/wealth/worst-case metrics, no-policy can win on income-
  extraction in the same run — real trade-offs, not a scripted bias). No horizontal overflow at
  375px. Regression: default numbers byte-identical pre/post; 0 JS errors.

- **1.52.4** — **Epic Life Ledger (owner-only), b37 — BETA ONLY, not yet promoted**: playbook
  cleanup. "Learn it one tool at a time" moved to sit directly above the playbook, so adjusting a
  tool and reading its effect on the projection no longer means scrolling back and forth. The
  playbook tables now hide any column that's zero across the whole phase (a plan not using the
  401(k) or the HELOC no longer carries those empty columns). Renamed for clarity: Phase 1 gained
  a new "Deployed capital" first column (the year's total allocation) plus "Fund policy"/"Fund
  401(k)"/"Tax refunded"/"Rental income"/"Mortgage debt"; Phase 2 gained "Income"/"Rental income"/
  "Tax bill" (now shown as a negative number). Desktop table and mobile cards share one data pass
  as before, so both stay in sync automatically. Regression: default stress-test numbers byte-
  identical pre/post ($23,913,001.86 legacy); 0 JS errors.

- **1.52.3** — **Epic Life Ledger (owner-only), b36 — BETA ONLY, not yet promoted**: three fixes/
  additions from real use. (1) Fixed a real stress-test bug — switching the policy tool off made
  "with policy" and "without policy" silently collapse to the identical plan (the comparison just
  reused whatever the grid search happened to pick under the current toggle, which never included
  policy once it was off). Each side now gets its own honestly re-searched best plan, policy forced
  on vs. forced off, independent of the toggle. (2) "The full picture" journey step now stays open
  when a tool is flipped on/off manually — it used to exit straight back to freeform mode; now the
  same dashboard recomputes live so a member can genuinely customize the combination without losing
  the guided view. Earlier scripted journey steps are unchanged (still exit on a manual flip, since
  those are fixed one-tool-at-a-time lessons). (3) New Section 4 opportunity: flags live whenever
  home or rental debt isn't covered by insurance or equity, with a "buy, borrow, die" explainer.
  Two honest, separately-labeled levers — raising the Protection IUL's death benefit (no invented
  premium number; this tool still doesn't model that policy's cost, same as its existing "Annual
  policy fee" disclosure) and, separately, funding the Accumulation IUL further (a real, already-
  modeled lever that also raises the advance contribution limit). Regression: default stress-test
  numbers byte-identical pre/post ($23,913,001.86 legacy); 0 JS errors throughout.

- **1.52.2** — **Epic Life Ledger (owner-only), b35 — BETA ONLY, not yet promoted**: fixed a real
  estate-math gap Bruce flagged — the "does the death benefit actually cover the home debt" check
  (`dbShortfall`) was hardcoded to `0` whenever the chronic-illness/LTC rider was toggled off, so an
  undersized death benefit silently showed as "fully covered" and legacy was overstated by the real
  gap. The check now runs unconditionally off the real death-benefit slider value; LTC draws still
  only happen when that rider is actually on. The "Death benefit vs. the debt" line (formerly
  "Still covered after any LTC draws," relabeled since it's no longer LTC-specific) now shows the
  real coverage figure in both states instead of "n/a — LTC not modeled." Verified via an isolated
  engine test (same allocation, only `ltcOn`/`ltcStartAge` varied): pre-fix, LTC-off vs. LTC-on-
  with-zero-draws produced DIFFERENT legacy numbers off the identical inputs (the bug); post-fix
  they're byte-identical, and turning on real LTC draws still deepens the shortfall as expected.
  Regression: at normal (sufficient-coverage) defaults, legacy is byte-identical pre/post fix
  ($23,913,001.86); 0 JS errors.

- **1.52.1** — **Epic Life Ledger (owner-only), b34 — BETA ONLY, not yet promoted**: fixed a real
  UX confusion Bruce flagged in the "flip tools freely" row — 0% biz credit was shown as its own
  independent toggle next to velocity chunking, but it does nothing without velocity on (the
  engine already gates `stacking` on `TOOLS.bizcredit && TOOLS.velocity`; only the UI was
  misleading). Now the button greys out and disables (both the toolbar and its per-topic "In the
  plan" twin) whenever velocity chunking is off, showing "— needs velocity chunking" instead of a
  false active/inactive peer state. The member's underlying 0% biz credit preference is preserved
  and restores exactly when velocity comes back on — verified via direct DOM test (off → disabled
  + hint updated → click-while-disabled no-ops → velocity back on → restores). 0 regressions.

- **1.52.0** — **Epic Life Ledger (owner-only), b33 — BETA ONLY, not yet promoted**: new
  "📄 Generate member report" button in the floating ALIS panel — a printable 7-page report (cover
  w/ prepared-for/by fields, starting point, the plan built, retirement outcome, stress-test
  results incl. income/wealth percentiles, playbook milestones, disclosures) assembled entirely
  from numbers already computed and on screen (`window.EPIC_DEBUG`, `EPIC_MC_RESULT`,
  `LAST_STATS`, the rendered ALIS takeaway paragraphs) — no new financial math. Auto-runs the
  stress test first if it hasn't been run yet, so the report is never generated incomplete.
  Print/Save-as-PDF via a dedicated `@media print` stylesheet + forced-light color theme
  (verified via real CDP print-media emulation: `.wrap`/toolbar/auth-gate all `display:none`,
  overlay stays `block`, `page-break-after:always` on every page). The disclosures page reads its
  9 numbered caveats LIVE off the existing in-tool elements by id (added this batch) — nothing new
  was written, so it can never drift out of sync with the tool's own vetted language — plus a
  compliance-review flag before real client use, per Bruce's explicit call. Regression: 0 JS
  errors, withholding toggle and glide-path math still byte-identical/exact.

- **1.51.0** — **Epic Life Ledger (owner-only), b32**: the 401(k)/qualified account now de-risks
  with age along the standard target-date glide path (Vanguard's published anchors: 90% stocks
  through 40 → 50/50 at 65 → 30/70 at 72, flat after) instead of one flat growth rate forever —
  Bruce's call: nobody in their 50s runs aggressive with retirement on the line, and a dip in
  later years hurts most. The old "projected growth rate" slider is now "Stock market return
  (S&P 500 total)," defaulting to the real 10.02%/yr full-record compound (computed directly from
  the NYU Stern/Damodaran raw 1928–2025 annual series this session), with a one-tap "recent era
  2011–2025 — 13.94%" preset; the bond slice earns the 10-yr Treasury's real record (4.82%/yr avg,
  7.9% swing, same dataset). The Monte Carlo draws both legs with their real volatility, blended
  by age (stock-bond correlation ≈0 over the record, so independent draws ARE the measured
  relationship); stock volatility tightened 19.7%→19.4% (recomputed from the raw series). This
  also RESOLVED the internal inconsistency found earlier today: emergent stress-test crediting now
  lands at 6.42%/yr vs the 6.40% "honest long-run" default (was 5.50% vs 6.40%). New glide readout
  in the 401(k) topic (mix + blended growth today / at retirement / at the 72+ floor); the journey
  now tells the story — the 401(k) trades growth for safety with age, the policy never has to
  because the 0% floor IS its safety. Verified: glide anchors exact, engine growth matches hand
  calculation to the penny, conversion look-ahead compounds along the glide, 0 JS errors.

- **1.50.0** — **Epic Life Ledger (owner-only), b31**: the Monte Carlo stress test now reports
  income extracted and total wealth (legacy + income) — worst 1-in-10 / median / best 1-in-10 across
  all 1,000 market histories, same treatment legacy already got, so "Max income" plans get the same
  downside-risk visibility "Max legacy" plans always had. Also replaced round marketing-style rate
  defaults with precisely-researched current figures: mortgage 6.43% (Freddie Mac PMMS weekly
  survey), HELOC 7.46% (Bankrate national average), investment-property loan 7.18% (primary +
  typical 0.75pt premium), home appreciation 4.44% (the same FRED Case-Shiller figure already cited
  elsewhere in the tool for its volatility — this also fixes a real internal inconsistency where the
  displayed default didn't match the tool's own sourced data). All sliders still fully adjustable;
  step sizes tightened (0.01) so the precise real values are actually reachable, not just displayed.

- **1.49.1** — **Epic Life Ledger (owner-only), b30**: fixed the ALIS stat-tile output boxes
  (Outcome/Legacy/Total tax paid/etc.) — longer titles like "401(k)→loan payoff" or "Losses carried
  to retirement" were overflowing their tile instead of wrapping to a second line (the base CSS rule
  had `white-space:nowrap`, only overridden under the mobile breakpoint added in b29 — the bug was
  present at every screen width, not just phones). Now wraps cleanly everywhere; verified zero
  overflowing labels at both 375px and 1280px.

- **1.49.0** — **Epic Life Ledger (owner-only), b29**: three fixes from real use. (1) The policy
  premium no longer automatically counted a member's redirected tax withholding, so the "tax bill
  paid out of pocket instead" line was often large even when the intent was to cover it via policy
  loan — added a "+ Add my tax withholding on top" switch that layers the real withholding onto the
  premium and shows the effect on both today's loan coverage and the full retirement projection
  (verified: at defaults, turning it on takes the effective premium from $10K to $42K/yr, drops
  out-of-pocket tax from $6,916 to $0, and lifts projected legacy by ~$1.26M; off is byte-identical
  to b28). (2) Full mobile pass: the year-by-year playbook renders as stacked cards instead of a
  table below 480px (table stays for desktop, same data, one render pass), both line charts now size
  to their real container width instead of shrinking to an unreadable 1000px coordinate space, and
  buttons/sliders/spacing are sized for touch (verified: zero horizontal scroll at 375px, zero
  regression at 1280px). (3) Trimmed wall-of-text: shortened the densest explainer paragraphs
  throughout, and collapsed the long compliance/assumption disclosures behind a "tap to see" summary
  line (same pattern as the existing "Agent settings" blocks) — nothing removed, just tucked away
  until wanted.

- **1.48.0** — **Epic Life Ledger (owner-only)**: fixed the same blank-screen bug just fixed on the
  other four gated tools (v1.47.0) — a DOM-timing race where the auth check could resolve correctly
  (real sign-in, real floating panel) while the main content stayed permanently hidden. Deferred the
  initial auth check to DOMContentLoaded and added the same 7-second load-failure watchdog. Verified
  the fix directly: reproduced the failure with a microtask-speed stub, confirmed it now passes, and
  confirmed zero regression across every existing Ledger feature (age/sex face sizing, target
  premium, enhanced target premium, the loan-payoff mechanic).

- **1.47.0** — **Fixed the real "blank white screen" bug on Admin (and the same bug, found while
  fixing it, on Setter, Client Success, and Closer).** Root cause: the sign-in check ran before the
  page's own content div had even been added to the page yet, so it could never find it to reveal
  it — even for a real, correctly-signed-in owner, with no error and no way to recover except a
  manual full reload landing on a different page. Fixed on all four by waiting for the page to
  finish loading before that first check runs. Also hardened: Setter, Client Success, and Closer
  were missing the same fail-closed protection Admin already had (start hidden, only ever reveal
  once confirmed signed in) — real data could otherwise stay visible underneath the lock screen if
  the sign-in check failed outright; and if the sign-in check can't load at all (bad connection,
  blocked request), you now see a clear "couldn't load" message with a Reload button instead of an
  unexplained blank screen. Also: EPIC Suite now has a real favicon (⚡) across the hub and all 5
  role tools, instead of the browser's default globe icon.

- **1.46.0** — **Epic Life Ledger (owner-only)**: the 401(k)-into-the-policy move now matches how it's
  actually done in the field. Real designs run a fixed, level premium every year, so extra 401(k)
  money can't simply become new premium without risking the 7-pay MEC test. Instead: when the tax
  math favors an early 401(k) withdrawal (same rule as before), the proceeds now pay down the
  outstanding POLICY LOAN that ordinary lifestyle draws already built up — no premium change, no
  MEC exposure. Verified the mechanic responds correctly to the real trade-off (fires more as the
  loan's carrying cost exceeds the 401(k)'s growth rate, and correctly stays off when growth clearly
  wins); as a greedy year-by-year heuristic (same character as before) it can occasionally net
  slightly negative in a thin-margin scenario — disclosed in the caveat, not hidden.

- **1.45.0** — **Hub redesign**: Admin and the Epic Life Ledger are now full role cards on the hub
  (same style as the other four roles) instead of tiny footer links — same owner-only lock, just
  visible. The 🚀 "Building next" panel is now collapsed by default (tap the header to open it —
  your choice is remembered), so the hub stays compact.

- **1.44.0** — **Epic Life Ledger (owner-only)**: added the enhanced target premium (1.25× target —
  the payout basis when year-1 client premium exceeds target), tucked into a collapsed "Agent —
  commission basis" panel since only agents need it. Shows target, enhanced target, year-1 client
  premium, and which basis actually applies. Structural finding worth knowing: because the death
  benefit auto-sizes to whatever premium is entered, Enhanced applies at essentially any funding
  level above $0, not just occasionally — that's a consequence of the auto-sizing design, not a bug.

- **1.43.0** — **Epic Life Ledger (owner-only)**: policy design now knows the insured's age AND sex,
  calibrated from six real AIUL 26 illustrations (Standard non-smoker, both sexes, ages 30/45/60).
  The auto-sized death benefit uses the real face-per-premium curve (shrinks with age, higher for
  women) instead of a flat age-30-male ratio; new Male/Female toggle; new Target premium line (the
  commission basis) exposed for commission tooling. Confirmed the cost-of-insurance sex difference is
  <0.5% of cash value once the correct face is used, so no separate female mortality table was needed.

- **1.42.0** — **Epic Life Ledger** added as a new owner-only tool (🔒 Ledger on the hub, next to
  Admin) — the full life-cycle wealth calculator, gated by the same real Supabase Auth +
  `profiles.role='owner'` check as the Admin dashboard. Also fixed a real gap in that shared gate
  pattern: the "hide the dashboard until authorized" step could silently never fire (a DOM-timing
  race), leaving the real content present underneath the lock overlay for anyone who inspected the
  page — now fails closed by default (hidden via CSS first, only ever revealed by script), applied
  to both Admin and the new Ledger.

- **1.19.1** — **← EPIC Suite** back link added to the Closer. **Referral Partner & Setter** got a mobile-first
  pass (bigger tap targets, no zoom-on-tap forms, full-width primary buttons, one-column). **Client Success**
  got mobile-safety touches. Closer & Client Success stay desktop-priority.

- **1.19.0** — EPIC Suite promote. **🧪 Beta toggle** across the whole suite (top-right chip: Live ⇄ Beta,
  code-gated, remembered per browser; hub routes the Closer card to the right build). **💡 Suggestion box
  (Beta only)** on every role tool → writes to Supabase for central review. **Per-role What's-new** badges on
  Setter / Referral Partner / Client Success. Closer promoted 1.16.0 → **1.19.0** (brings in 1.17–1.19: auto-select
  outcome, auto-read BANK personality, header version badge + movable BANT dock, email templates, etc.). Renamed
  off "cockpit" (repo/URL → **epic-suite**; source file → `closer.html`). Hub order: Referral → Setter → Closer → Client Success.
- **1.16.0** — Big release (promoted from beta). The cockpit is now a full closing workstation:
  focus-driven queue (Hot/New/Callbacks/Follow-ups/Overdue + Hawaii-only), a working **rep phone**
  (call timer, two-way AI texting, quick-send dock: 💳 Pay / 📅 Booking / 📎 Content / 🎥 Zoom+Teams,
  mute + drop-VM, confirm-before-text), **real EPIC LIFE pricing + inclusions** with a pitch-deck pay
  popup that recommends the right tier, **calendar booking** (2-week grid) + **Schedule callback**,
  **email templates** on the contact card (proposal / event invite / play-the-game), a **CLOSER
  co-pilot** that suggests your next line + advances the stage from your notes, the **BANK personality**
  close card, closed-deals + revenue on the momentum bar, and a movable/collapsible floating BANT card.
- **1.1.0** — Live-call redesign: **SAY THIS** band up top (AI's suggested line + objection rebuttal, always front-and-center), Signals + BANT merged into one always-visible strip, **keyboard-first log→next** (press 1–4 then Enter to log and auto-advance the queue), and "Not qualified" pre-fills its reason from the signals actually heard. Added an in-app **"What's new"** changelog that auto-opens on each new build.
- **1.0.9** - Renamed the tool to **EPIC Closer** (was Sales Cockpit); landing menu + all builds updated.
- **1.0.8** — Wired the End button = stop everything + full reset, so the demo restarts cleanly.
- **1.0.7** — Mic input (Web Speech API): talk to Renee, it transcribes live and the AI reacts.
- **1.0.6** — Booking is a "3B call" with two outcome paths (closed → onboarding, not closed → 3B + closer); auto-defaults from payment.
- **1.0.5** — Prospect's-phone view (incoming call, SMS payment link, payment received, calendar invite); full local verification.
- **1.0.4** — Quiet-hours/best-time guard, local-presence caller ID, daily-mission gamification.
- **1.0.3** — "Close the deal" panel: book appointment + collect payment (Stripe-link simulation).
- **1.0.2** — Persistent bottom chat bar (bigger, always-accessible typing).
- **1.0.1** — Live prototype: AI-ranked queue, scripted live call with realistic pre-baked voice, consent chip, BANT autofill, CLOSER script, objection rebuttals, signal tags, notes, disposition, interactive AI chat; update detection.
