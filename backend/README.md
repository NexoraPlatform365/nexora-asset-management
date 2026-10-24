# Nexora API starter

This is a starter API for account registration/login and public asset information. It is not a production investment platform and must not be used to receive or hold client funds.

## Deploy on Render
1. In Render, create a **New + → Web Service** and connect this repository.
2. Set Root Directory to `backend`.
3. Runtime: Python. Build command: `pip install -r requirements.txt`. Start command: `gunicorn app:app`.
4. Add environment variable `APP_SECRET` with a long random secret. Set `COOKIE_SECURE=true` on HTTPS.
5. Add a persistent disk mounted at `/var/data` before relying on SQLite data persistence. A managed PostgreSQL database is preferable for production; this starter uses SQLite.
6. After deployment, visit `/api/health` to verify the service.

## API routes
- `GET /api/health`
- `GET /api/csrf` (sets a session cookie and returns a CSRF token)
- `POST /api/register` JSON: `full_name`, `email`, `country`, `password` (12+ characters)
- `POST /api/login` JSON: `email`, `password`
- `POST /api/logout` (requires CSRF token)
- `GET /api/me` (requires login)
- `GET /api/assets`
- `GET /api/tiers`
- `GET /api/admin/overview` (admin only; no admin promotion utility included)

For mutating requests, send the token returned by `/api/csrf` in `X-CSRF-Token` and preserve cookies. Registration/login return a new token; use the session cookie and fresh token for subsequent requests.

## Not production-ready
This API has not undergone an independent security review. It lacks email verification, password reset, MFA, rate limiting, detailed audit logs, KYC/AML, transaction ledger, payment integrations, custody, reconciliations, or a real investor portfolio. Do not collect sensitive investor data or accept deposits through it. Before live financial services, obtain legal/regulatory advice and implement professional security, privacy, operational, accounting, and compliance controls. The frontend `index.html` is not yet wired to these API endpoints.
