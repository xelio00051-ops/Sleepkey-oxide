# SLEEPOV STORE &#40;sleepovstore\.cc&#41; — real app foundation &#40;not the old HTML demo&#41;

This is a Next\.js project scaffold for the storefront and backend endpoints\. It includes a purple/Oxide\-style landing page, catalog/search/category filters, local cart, order creation API, Supabase schema, and deployment instructions\. **Payment processing, secure admin authentication/CRUD, Telegram bot, digital\-file delivery and receipt verification are not implemented yet** and must be added and tested before accepting money or selling goods\.

## 1&#41; Create Supabase project

- Create project at https://supabase\.com
- In SQL Editor run `supabase-schema.sql`
- Copy project URL, anon key and service role key &#40;service role key must stay server\-side&#41;\.

## 2&#41; Run locally

- Install Node\.js 20\+
- `npm install`
- Copy `.env.example` to `.env.local` and fill keys
- `npm run dev`
- Visit http://localhost:3000

## 3&#41; Deploy to Vercel

The old single HTML file deployment is not the correct deployment for this app\.

- Create a GitHub repo and upload the **whole project directory**, not just `index.html`\.
- Import repo into Vercel\. Framework Next\.js should be detected\.
- Add environment variables from `.env.example` in Vercel Project Settings → Environment Variables\.
- Deploy\. Connect `sleepovstore.cc` in Settings → Domains when deployment is ready\.

## Safety before production

- Implement Supabase Auth \+ admin role and server\-verified authorization for admin routes\.
- Add strict server\-side product price validation &#40;never trust prices from browser&#41;, immutable order item snapshots, rate limiting, bot protection and audit logs\.
- Connect payment provider and confirm payment only via verified webhook, not user receipt screenshots\.
- Add private storage buckets and one\-time delivery logic; prevent duplicate fulfillment\.
- Set up backups and adult/guardian\-supported payment/business accounts as applicable\.

## Telegram bot foundation &#40;not fully connected yet&#41;

- Added server\-only `lib/telegram.ts` helper for bot messages and owner order notification formatting\.
- Added `/api/telegram/webhook` endpoint for bot `/start` welcome message\.
- Configure `TELEGRAM_BOT_TOKEN`, `TELEGRAM_OWNER_CHAT_ID`, and `TELEGRAM_WEBHOOK_SECRET` in Vercel environment variables\.
- Customer account linking via one\-time tokens, order\-triggered notifications, secure owner admin controls, and payment callbacks are still required before this is production\-ready\. Do not treat this endpoint as a finished bot or use it to process real orders yet\.

## Payment\-method configuration &#40;planned server\-backed admin&#41;

The schema includes `payment_methods` for Platega, manual details, crypto and @send, including `allowed_currencies` per method\. The admin page now provides a currency selector per method and draft configuration UI\. It is NOT persisted yet: secure Supabase Auth/admin role and server\-side admin API must be implemented before settings can be saved\. Do not enter payment secrets in the draft UI\.
