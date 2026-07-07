# EPIC Suite — changelog (newest first)

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
