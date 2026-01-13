# AI Site Builder — Simple SPA (Vanilla JS)

یہ scaffold ایک سادہ سنگل‑پیج ایپ ہے جو user-side generation، cost estimation اور Stripe Checkout کے ذریعے ادائیگی ظاہر کرتی ہے۔  
مقصد: کم سے کم infra، کم رسک، اور آپ کی 10% کمیشن حکمتِ عملی کے ساتھ فوری ریونیو شروع کرنا۔

فائلز شامل:
- `index.html` — SPA UI
- `styles.css` — سادہ styling
- `app.js` — client logic (estimate, generate, checkout)
- `api/create-checkout-session.js` — serverless function (Vercel/Netlify) to create Stripe Checkout session
- `templates/` — sample templates (embedded in app.js in this scaffold)

Quick start (recommended)
1. Create a public GitHub repo and push these files.
2. Deploy `index.html` (and the repo) to GitHub Pages (Settings → Pages → branch: main).
3. Deploy serverless function to Vercel:
   - Create a new project in Vercel linked to the repo.
   - Add environment variables:
     - `STRIPE_SECRET_KEY` = your Stripe Secret key
     - `DOMAIN` = `https://<your-site-domain>` (used for success/cancel URLs)
   - Vercel will detect `api/` and deploy functions. (If using Netlify, adapt handler format.)
4. Configure Stripe:
   - Create a Stripe account and get Publishable & Secret keys.
   - In Stripe Dashboard → Developers → Webhooks, add an endpoint if you want to handle advanced flow.
5. Open the GitHub Pages URL (or the domain) and test the flow:
   - Choose template → Estimate → Click Pay → Stripe Checkout → Complete payment.
   - After successful payment you'll be redirected to success page; then click "Generate & Download" to get ZIP.

Important security notes (read!)
- This scaffold trusts the client for the amount. In production you MUST:
  - Recalculate/validate amount server-side before creating Checkout session.
  - Never trust client‑sent `amount_cents` without verification.
- Prefer client‑side provider API usage (user provides their API key) so platform doesn't pay provider bills.
- Never store user API keys unencrypted; better approach is browser-only usage.

How the 10% commission works (flow)
- The UI estimates provider cost (client-side) and computes platform fee = provider_cost * 0.10.
- Total = provider_cost + platform_fee. The Checkout session charges the total.
- You receive the entire payment in Stripe; you should set up bookkeeping so platform_fee (10%) is accounted as your revenue and the remainder is considered customer's provider cost (documented in invoice/receipt). If you later bill provider separately, implement appropriate flows (Stripe Connect etc).

Next steps & enhancements
- Integrate server-side validation to prevent tampering.
- Add optional "use my provider key" flow to call OpenAI/HF from client and insert dynamic content.
- Add admin dashboard (pull transactions from Stripe API).
- Add more templates and on-the-fly customization.
 
اگر آپ چاہیں تو میں یہ scaffold آپ کے لیے GitHub ریپو میں commit کر دوں یا Vercel deploy سے متعلق step‑by‑step guide بھی دے دوں۔ بتائیں آپ کونسا کرنا چاہیں گے
