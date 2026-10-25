# FarmaGo

A zero-dependency Node.js marketplace prototype connecting local farms, buyers, and grocery surplus. Farmers can list produce with photos, MRP, quantity, and use-by dates; buyers can bid or buy at the farmer's ask; stores can post free-to-claim compost/feed loads.

## Design references

The landing uses Motion's JavaScript `inView`/`animate` pattern for reduced-motion-aware reveal effects: [Motion](https://motion.dev/docs/quick-start). Layout and interaction inspiration came from the browsable component registry at [21st.dev](https://21st.dev/community/components); its React components were not added to this vanilla JavaScript project. The five local farm photographs and their licenses are credited beside the homepage collage.

## Run

```powershell
node server.js
```

Open `http://localhost:4173`. To use a different port:

```powershell
$env:PORT = '4174'
node server.js
```

The app creates `data/db.json` automatically. Set `FARMAGO_DB_FILE` to use a different JSON file.

## Google sign-in with Supabase

Copy the values from `.env.example` into your PowerShell environment before starting the server:

```powershell
$env:SUPABASE_URL = 'https://your-project.supabase.co'
$env:SUPABASE_ANON_KEY = 'your-publishable-key'
$env:PORT = '4173'
node server.js
```

In Supabase, enable Google under **Authentication → Providers**, add your Google OAuth web client ID and secret, set the Site URL to the FarmaGo origin, and add that origin to the Auth redirect URL allow list. Google OAuth also needs the Supabase callback URI shown in the Google provider settings. The app sends users through `signInWithOAuth({ provider: 'google' })`; the server verifies each access token with Supabase before serving protected API routes.

Users enroll a TOTP authenticator on first login; protected API routes require Supabase assurance level `aal2`. Enable MFA/TOTP for the project in Supabase Auth. The first user can scan a QR code with an authenticator app and enter a six-digit code before entering the marketplace.

Run [`supabase/schema.sql`](supabase/schema.sql) in the Supabase SQL editor to create the RLS-protected profile table and account-creation trigger. Profile updates are sent with the signed-in user's access token; no service-role key is used.

Only the Supabase publishable/anon key belongs in this app. Never expose a service-role key. Without project settings, the public homepage and policy pages remain available, while marketplace pages stay locked.

Marketplace profiles (role, organization, ZIP) live in Supabase. Listings, bids, messages, and demo transaction records still live in the local JSON file; migrate those to a transactional database with RLS before any production or multi-instance deployment.

## Maps, payments, and image review

Set `GOOGLE_MAPS_API_KEY` to enable the Maps Embed API; restrict the key by HTTP referrer in Google Cloud. Without it, listing maps use the keyless Maps embed URL with the ZIP-derived approximate coordinates.

Set `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `STRIPE_CURRENCY`, and `APP_BASE_URL` to enable hosted Stripe Checkout. Configure Stripe to send `checkout.session.completed` to `/api/stripe/webhook`. The buyer must confirm receipt before Checkout becomes available, and only a verified webhook marks the order paid. Stripe processing fees apply; the prototype itself does not charge a platform fee.

Set `GEMINI_API_KEY` to require Gemini image review. In production, configure `REQUIRE_GEMINI_API=true`; if Gemini is unavailable, appraisal fails closed and the lot cannot be listed. The no-key local development heuristic is only a UI/test aid and is not a food-safety certification. Rotate any key that has been pasted into chat or source control.

## Checks

```powershell
node scripts/smoke-test.js
node scripts/browser-check.js
```

The smoke test resets its configured database. For an isolated run, set `FARMAGO_DB_FILE` to a temporary path before starting the server and running the tests.

For local-only UI tests, `FARMAGO_DEV_AUTH_BYPASS=true` enables seeded demo identities. The server ignores this flag when `NODE_ENV=production`; never enable it on a deployed instance.
