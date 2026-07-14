# Nightly Research — 2026-07-14

## Tonight's focus
First-run baseline: map EPIC Suite's actual current state, then apply Dan Martell's delegation/automation frameworks and Alex Hormozi's sales/offer frameworks to identify what most directly speeds up closing real calls today.

*(No `research/` folder existed before this run, so this is the first report — there was nothing prior to build on.)*

## Repo reality check (differs from assumed layout)

The task brief assumed `prototype/`, `supabase/`, `CLAUDE.md`, `SALES_COCKPIT_SPEC.md`. None of those exist in `brucelin-tla/epic-suite`. The actual structure: root `index.html` + `choose.html` (live/beta picker), `DEPLOY.md`, `CHANGELOG.md`, and role apps `admin/`, `setter/`, `success/`, `ledger/`, `live/` (= "EPIC Closer"), `referral/`, each a large single-file app, mirrored under `beta/`. This is noted so future nights don't re-search for paths that don't exist.

## Key findings

**EPIC Suite is further along than a "prototype."** All six role apps connect to a real Supabase project (Auth + Postgres tables `prospects`, `follow_ups`, `bookings`, `deals`, `referrals`, `commissions`, `profiles`) via edge functions (`sales-hub-sync`, `fathom-backfill`, `calendly-book`, `close-deal`, `admin-invite`, `submit-referral`). Data is genuinely shared across roles, not siloed in `localStorage` — a Setter's booked lead is visible to a Closer and flows into commissions. This means the "sales cockpit" concept is real, not aspirational.

**No live telephony exists, and that's correctly scoped as human-owned.** `live/index.html` states outright it's a "Prototype — simulates the live-listening loop"; real leads use a "plain live-call mode" with the rep on an actual phone/Zoom, manually feeding the AI coach, while Fathom syncs the recording afterward. This matches Dan Martell's explicit guidance that AI should handle "all the other stuff" but not replace the human relationship layer — "Buyers don't want to buy from a bot" ([danmartell.com](https://www.danmartell.com/how-to-get-so-many-customers-with-ai-it-feels-illegal/); [martech.org](https://martech.org/ai-can-scale-sales-but-it-cant-build-trust/)). No action needed here — it's already built the right way.

**Two concrete, non-cosmetic gaps block reps from using the cockpit smoothly today:**
1. GoHighLevel→Supabase sync (`sales-hub-sync`) is a manual "Sync now" button in `admin/index.html` — no cron/webhook. If Bruce doesn't click it, Setter/Closer queues go stale.
2. The AI coach in `live/index.html` requires each user to paste their own Anthropic API key into `localStorage` — no shared org key or backend proxy, and the key is exposed client-side.

**Gap #1 collides directly with Hormozi's most concrete, evidence-backed lever: speed-to-lead.** Hormozi popularizes the finding (originating from a ~2013 Velocify/ICE Mortgage Technology study of ~3.5M leads) that contacting a lead within ~60 seconds correlates with a large lift in close rate, and posts his own examples of teams hitting sub-5-minute response with high close rates ([X post](https://x.com/AlexHormozi/status/1879936850959474915); [origin-check](https://www.1minutesales.com/blog/posts/391-higher-conversion-rates.html)). A manual sync button directly undermines the single lever he considers most provable — a lead can sit un-synced (and thus un-contacted) for however long it takes Bruce to remember to click a button.

**Hormozi's "money is in the follow-up" claim** — persist through multiple touches (commonly cited ~5-7) rather than abandoning leads after 1-2 attempts ([nikofischer.com](https://nikofischer.com/alex-hormozi-lead-generation-blueprint); [gregfaxon.com](https://www.gregfaxon.com/blog/100m-leads-summary)) — isn't yet confirmed to be enforced in `setter/index.html`'s New/Callbacks/No-shows tabs; worth verifying whether stale/under-followed leads are surfaced or silently drop off.

**Martell's Replacement Ladder (EA → Delivery → Marketing → Sales → Leadership)** ([buybackyourtime.com](https://www.buybackyourtime.com/); [risewithdrew.com](https://risewithdrew.com/dan-martells-replacement-ladder-how-to-buy-back-my-time-so-i-can-focus-on-what-energizes-me/)) and "Who Not How" ([danmartell.com/who-not-how](https://www.danmartell.com/who-not-how/)) both point the same direction: the manual sync button and per-user API key paste are "how" problems that should be systematized, not worked around by hand each day.

**Compliance flag (not a framework finding, but material):** `live/index.html`'s BANK-personality coaching script literally instructs reps to lead with "the guarantees." No explicit "education, not individualized financial/investment advice" disclaimer was found anywhere across the six role apps, despite that being Bruce's stated hard compliance line. This predates tonight's research but is worth a deliberate look, not a silent pass.

**Epic Life Ledger (`ledger/`, 386KB, owner-only, `TOOL_RANK=5`)** has absorbed the majority of recent CHANGELOG activity (b34 through b45) — it's a sophisticated AIUL/velocity-banking illustration engine, but it is not member-facing and not yet a tool a Closer opens live on a call. See Scope-creep flags below.

## Concrete next steps for EPIC Suite (ranked by speed to impact on real calls today)

1. **Automate the GoHighLevel sync** — replace the manual "Sync now" button with a scheduled job or webhook so `prospects` stays current without Bruce remembering to click it. This is the highest-leverage fix because it directly restores the speed-to-lead lever Hormozi's data says matters most, and it's fixing something already built rather than building something new.
2. **Move the Anthropic API key server-side** — add a thin edge function that proxies Closer AI-coach calls with a shared org key, removing the per-rep manual key-paste step (an actual onboarding blocker to team-wide use today) and closing the client-side key exposure.
3. **Surface follow-up cadence on the Setter dashboard** — flag leads that haven't hit a minimum touch count (per Hormozi's persistence data) instead of letting them silently age out in the Callbacks/No-shows tabs. Small additive field, not a redesign.
4. **Add the "education, not individualized advice, no guarantees" disclaimer** where members/reps actually see it (referral, success, and specifically the BANK coaching script's "guarantees" language in `live/index.html`) — a compliance requirement, not a nice-to-have, and a small copy change.
5. **Automate Fathom call-recording backfill** the same way as #1 (currently also admin-triggered) — same class of fix, lower urgency than the lead-sync gap.

## Scope-creep flags

- **Epic Life Ledger's recent dev volume is the clearest scope-creep risk found tonight.** It's owner-only, not yet used live by a Closer on a real call, and has consumed roughly a dozen recent CHANGELOG entries of design/engineering effort (rider math, COI calibration, wizard UX). Per Martell's Replacement Ladder, this is "Delivery"-tier tool-building competing for the same hours as "Sales"-tier work, and it matches Bruce's self-acknowledged pattern exactly. Recommend pausing further Ledger feature work until items #1–#2 above (which block reps from smoothly using the *existing* cockpit today) are shipped, and until it's explicit whether the Ledger is actually blocking a real client conversation this week or is an interesting problem to keep refining.
- **Building the real Twilio → Deepgram → Claude live-audio pipeline** (referenced as "Real version" in a code comment in `live/index.html`) is a legitimate eventual need but is a multi-week infra project. Don't let it jump the queue ahead of the much cheaper sync/API-key fixes above — those unblock today's calls; live audio doesn't.
- **Any new Setter/Closer dashboard fields (follow-up flags, CLOSER-stage tagging) should stay additive**, not become a UI redesign — the temptation with a working cockpit is to keep polishing it instead of getting on the phone.
