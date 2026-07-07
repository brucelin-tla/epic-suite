# EPIC Suite — changelog (newest first)

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
