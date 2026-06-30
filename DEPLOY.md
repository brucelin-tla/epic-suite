# Cockpit demo — structure + deploy process (review-before-live gate)

## URLs
- **Landing menu:** https://brucelin-tla.github.io/cockpit-demo/        (root — choose Live or Beta)
- **LIVE** (team uses this): https://brucelin-tla.github.io/cockpit-demo/live/   (served from /live)
- **BETA** (review before promoting): https://brucelin-tla.github.io/cockpit-demo/beta/ (served from /beta)

## Flow for EVERY change
1. Edit source: `sales-cockpit/prototype/cockpit-demo.html` — bump `APP_VERSION` + `version.json`.
2. **Deploy to BETA:** copy build -> `/beta` (index.html, version.json, audio/), push. Beta is free to push anytime.
3. **Review:** Bruce opens the BETA url.
4. **Promote (ONLY on Bruce's OK):** copy `/beta/*` -> `/live/*`, add a CHANGELOG entry, push -> LIVE updates.

## Rules
- NEVER change /live without Bruce's explicit OK. Beta needs no approval.
- Bump the version on the BETA deploy; promote just publishes it (live tabs then get the "Update now" bar).
- The landing page (root index.html) shows each build's version, fetched live.
- Smoke-test the key flows (Start -> consent -> close -> End) in preview before asking for review.
