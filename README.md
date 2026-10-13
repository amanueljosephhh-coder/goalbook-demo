# GoalBook Sportsbook Demo

A responsive, demo-only sportsbook simulator. It uses fictional events and decimal odds, server-side demo tickets, and virtual credits. It is **not** a real-money betting platform and includes no deposits, withdrawals, payment integrations, or live sports-data feed.

## Requirements

- Node.js 20 or newer
- npm

## Run locally

1. Extract this project.
2. Copy `.env.example` to `.env`.
3. Set `ADMIN_PASSWORD` to a unique password and `SESSION_SECRET` to a random string of at least 32 characters.
4. Install and start:

   ```bash
   npm install
   npm start
   ```

5. Open `http://localhost:3000`.
6. Click **Admin** and sign in with the configured `ADMIN_PASSWORD`.

For a quick random secret, run `node -e "console.log(require('crypto').randomBytes(48).toString('hex'))"`.

## What works

- Responsive public sports interface
- Fictional football, basketball, and tennis events
- Sample odds and selectable markets
- Single and accumulator demo tickets
- Server-side virtual balance and potential-return calculation
- Demo ticket history
- Server-protected admin login with rate limiting
- Admin can settle fictional match results
- Session cookies are HTTP-only and use SameSite=Lax; production cookies require HTTPS

## Current demo limitations

- Event and ticket data live in server memory and reset when the instance restarts. This is intentional for the first prototype, not suitable for real-money use.
- Session records use SQLite for login sessions, but player balances, event results, and tickets are not persistent yet.
- The app uses no real sports data, payment provider, KYC provider, or live odds feed.
- Google Fonts are loaded from a third-party CDN; local system fonts remain available if blocked.
- The in-memory state is for a single-instance demo. Do not scale this version across multiple instances.
- Admin password protection is server-side, but before any production use we would need a stronger identity system, durable database, CSRF strategy, secrets management, monitoring, and independent security review.

## Free public demo hosting (Render)

1. Create a Git repository and push this folder to GitHub.
2. Create a Render account and choose **New → Blueprint** for the repository, or create a Web Service manually.
3. Build command: `npm install`; start command: `npm start`.
4. Set environment variables `ADMIN_PASSWORD` and `SESSION_SECRET` in the Render dashboard. Never commit `.env`.
5. Deploy and test the public site and admin sign-in.

Render's free service availability, sleeping behavior, and filesystem persistence can change. This demo's in-memory balances/tickets reset when the server restarts, and a free instance may sleep. Check the provider's current terms before relying on it. For durable sessions on a host with ephemeral storage, configure a managed session store before public use.

## Project structure

```text
goalbook-demo/
├── public/
│   ├── index.html       # page layout
│   ├── styles.css       # responsive visual design
│   └── app.js           # frontend interactions
├── data/                # local session SQLite directory
├── server.js            # API, demo state, admin auth, session security
├── package.json
├── render.yaml          # Render deployment blueprint
├── .env.example
├── .gitignore
└── README.md
```

## API overview

- `GET /api/health`
- `GET /api/session`
- `GET /api/matches`
- `GET /api/tickets`
- `POST /api/bets`
- `POST /api/admin/login`
- `POST /api/admin/logout`
- `GET /api/admin/overview` (admin session required)
- `POST /api/admin/matches/:id/settle` (admin session required)

## Before any real-money product

Do not add deposits, withdrawals, cash balances, or real wagers until qualified counsel confirms the licensing position for every target market and payment partners authorize the business. A production build would need a durable database, auditable settlement logic, user identity and age checks, responsible-gambling controls, anti-fraud/AML procedures where applicable, operational monitoring, and security testing.
