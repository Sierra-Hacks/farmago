# FarmaGo — Build Progress

Zero-dependency Node.js app. No npm install, no build step, no API keys required.

**Status legend:** `[ ]` todo · `[~]` in progress · `[x]` done · `[!]` blocked

---

## Phase 1 — Foundation
- [x] Project skeleton
- [x] Deterministic pricing engine (`lib/pricing.js`)
- [x] Appraisal adapter with Gemini and demo fallback (`lib/appraise.js`)
- [x] JSON persistence layer (`lib/db.js`)
- [x] Seed data — valley farms, buyers, rescue loads (`lib/seed.js`)

## Phase 2 — API
- [x] Zero-dep HTTP server + static hosting (`server.js`)
- [x] Appraisal endpoint with server-side cache
- [x] Listings: create / read / close / cancel / relist
- [x] Bids: floor + anti-gouge cap + atomic award
- [x] Buy-now with race guard
- [x] Messages
- [x] Orders: receipt-gated payment + fulfillment
- [x] Rescue rail: post / claim
- [x] Match scoring
- [x] Demo data reset endpoint

## Phase 3 — Frontend
- [x] Layout, design system, role switcher
- [x] Home
- [x] Farmer dashboard
- [x] New listing: photo upload → appraisal → MRP-aware price panel
- [x] Buyer browse with match scores
- [x] Listing detail: bids, buy-now, close, relist, messages
- [x] Orders page
- [x] Rescue rail
- [x] "Load sample data" button

## Phase 4 — Verification
- [x] Syntax check all files
- [x] Server boots
- [x] Appraisal rejects non-produce
- [x] Appraisal passes good produce
- [x] Bid below floor rejected
- [x] Bid above cap rejected
- [x] Buy-now double-click race guarded
- [x] Close awards highest qualifying bid
- [x] Closed listing cannot be reopened
- [x] Relist creates new row + audit trail
- [x] Message round-trip
- [x] Buyer must confirm receipt before payment
- [x] Rescue claim
- [x] Demo reset works
- [x] Headless UI checks

## Phase 5 — Handoff
- [x] Run instructions and API key slot documented
- [x] Final progress update

---

## Build log

- Added MRP-aware pricing, with nearer expiry lowering recommendations.
- Enforced buyer-confirmed receipt before payment; farmers cannot self-confirm or mark paid.
- Made rescue loads explicitly free to claim and rebranded the app as FarmaGo.
- Verification: 67 backend smoke checks and 31 headless UI checks pass.
- Gemini is optional; the no-key colour heuristic is a demo-only visual screen, not a food-safety certification.
