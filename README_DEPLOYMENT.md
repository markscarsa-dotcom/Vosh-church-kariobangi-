# VOSH Church Int'l - Kariobangi — STK Push Deployment Package

This package contains the church website and a Node.js backend for Safaricom Daraja STK Push.

## What is included

- `VOSH_Church_Intl_Kariobangi.html` — finished website.
- `server.js` — secure server-side Daraja integration.
- `.env.example` — configuration template; contains NO real secrets.
- `package.json` — Node.js dependencies/start command.
- `Dockerfile` — optional container deployment.
- `render.yaml` — optional Render deployment configuration.
- `.gitignore` / `.dockerignore` — prevents secrets and dependencies being packaged accidentally.

## Before going live

Safaricom's production callback must be publicly reachable over HTTPS. The website and backend should be deployed to a public HTTPS service, and the exact callback URL should be configured in the Daraja/M-Pesa setup.

Example callback:

`https://YOUR-SITE-DOMAIN.example/api/mpesa/callback`

Do not use `file://`, localhost, or a temporary public URL for production.

## Configure the server

1. Copy `.env.example` to `.env`.
2. Put your Daraja Consumer Key, Consumer Secret, shortcode and STK Passkey into `.env`.
3. Set `CALLBACK_URL` to your deployed HTTPS callback URL.
4. Keep `.env` private. Never upload it to GitHub or put it in the website HTML.
5. Run:

```bash
npm install
npm start
```

The site is served at the root URL by the same server, so the browser calls `/api/mpesa/stkpush` on the same origin.

## Health check

After deployment, open:

`https://YOUR-SITE-DOMAIN.example/health`

A healthy server returns JSON similar to:

```json
{"ok":true,"daraja":"production"}
```

## M-Pesa flow

The customer enters a Kenyan M-Pesa number and amount, selects the giving type and taps **Pay with M-Pesa**. The server sends the STK Push request to Daraja. The customer enters the PIN only in the official M-Pesa prompt on their phone.

## Important credential warning

Do not reuse a production Consumer Secret or STK Passkey if it has been exposed publicly or in a shared file. Rotate exposed credentials in the relevant Safaricom/Daraja account, then put the replacement values into the server environment.

## If the shortcode is a Till

This package defaults to `CustomerPayBillOnline`, which is appropriate for a PayBill. If the shortcode is actually a Buy Goods/Till number, set:

`DARAJA_TRANSACTION_TYPE=CustomerBuyGoodsOnline`

before deploying.
