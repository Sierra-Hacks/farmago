# FarmaGo Feature Matrix

Every requirement from the brief, mapped to where it lives in the code.

| # | Requirement | Where | Status |
|---|---|---|---|
| 1 | Farmers connect directly to buyers and food banks | `server.js` listings API, `public/app.js` buyer view | implemented |
| 2 | Match on distance, budget fit, and expiry | `lib/pricing.js` → `matchScore()` | implemented |
| 3 | Farmers can sell partial lots | farmer listing form and quantity-based bids | implemented |
| 4 | Farmer sets firm ask and bid floor | `askPrice` + `floorPrice` on listing | implemented |
| 5 | Buyers bid against each other | `POST /api/listings/:id/bids` | implemented |
| 6 | Recommend price from MRP, region, quantity, and expiry | `lib/appraise.js` + `baselineValue()` | implemented |
| 7 | Free grocery surplus for farm compost/livestock | rescue rail with free claims | implemented |
| 8 | Reduce food waste | rescue rail + expiry-aware pricing | implemented |
| 9 | Guard against extreme prices | anti-gouge cap at 1.25× fair value | implemented |
| 10 | Photo required before listing | new-listing form + server check | implemented |
| 11 | Screen produce type and visible defects | Gemini adapter; heuristic demo fallback | implemented with limitation |
| 12 | Open bid visibility | bids table on listing detail | implemented |
| 13 | Buyer ↔ farmer messaging | messages API + thread | implemented |
| 14 | Track order payment | `orders.payment` | implemented |
| 15 | Finalized lot cannot reopen | terminal listing states | implemented |
| 16 | Relisting creates a new lot | `POST /api/listings/:id/relist` | implemented |
| 17 | Buyer confirms receipt before paying | order API and buyer-only controls | implemented |
| 18 | Load sample data for presentation | header button → `POST /api/demo/load` | implemented |

**Integration note:** Gemini vision review is enabled by `GEMINI_API_KEY`. Without it, the app uses a colour-statistics heuristic for demonstration only; that fallback is not a food-safety certification. Payment is currently an order-state demo, not a payment processor integration.
