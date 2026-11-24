# AZAM EARN

Arabic-first ad and rewards platform source.

## Project structure
- `index.html` — root redirect to the static app.
- `src/index.html` — Supabase-backed user dashboard.
- `src/telegram/index.html` — Telegram Mini App identity verification UI.
- `telegram/index.html` — Telegram route redirect.

## Supabase integration
The browser uses only the project's public publishable key. No service-role key or provider signing secret is shipped to the client.
- Email/password sign-up and sign-in through Supabase Auth.
- Active offers are read from `public.offers`, filtered to approved/connected incentive providers that support Yemen.
- Balances and history are read from the user's own `public.ledger_entries` rows.
- Wallet/network preferences are stored in the user's own `public.profiles` row.
- USDT payout requests are inserted as `pending`; the database trigger validates the saved destination and available balance while serializing requests on the user's profile row. This records a request only; it does not send USDT.
- Telegram Mini App sends Telegram `initData` to the `telegram-initdata-verify` Edge Function. The endpoint verifies the Telegram signature and rejects replayed initData.

## Deployment
Vercel serves the `src` directory as the static output directory. The current GitHub-linked preview was built from `main`.

## Production readiness limitations
- No eligible ad provider or active offer is currently registered in the database.
- There are no real reward conversions or ledger credits yet.
- No Binance/USDT payout provider is connected; payout requests require manual review and payment outside this application.
- Telegram identity verification is not the same as linking Telegram to an authenticated email account.
- Supabase Auth leaked-password protection is currently disabled; enable it in the Supabase Auth password/security settings.
- Test sign-up, email confirmation, sign-in, offer visibility, and payout-request validation in the preview before promoting it to production.

Never create fake offers, balances, conversions, or payout confirmations.
