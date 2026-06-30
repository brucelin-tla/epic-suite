# Cockpit demo — deploy process (staging → review → promote)

Two URLs:
- **LIVE** (share with the team): https://brucelin-tla.github.io/cockpit-demo/        (served from repo ROOT)
- **STAGING** (review before live): https://brucelin-tla.github.io/cockpit-demo/staging/ (served from /staging)

## Flow for EVERY change
1. Edit source: `sales-cockpit/prototype/cockpit-demo.html` — bump `APP_VERSION` + `version.json`.
2. **Deploy to STAGING:** copy build → `/staging` (index.html, version.json, audio/), push. Staging is free to push anytime.
3. **Review:** Bruce reviews the STAGING url.
4. **Promote (ONLY on Bruce's OK):** copy `/staging/*` → repo ROOT, add a CHANGELOG entry, push → LIVE updates.

## Rules
- NEVER change ROOT (live) without Bruce's explicit OK. Staging needs no approval.
- Bump the version on the STAGING deploy; promote just publishes it (live tabs then get the "Update now" bar).
- Smoke-test the key flows (Start → consent → close → End) in preview before asking for review.
