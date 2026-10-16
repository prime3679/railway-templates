# Speedtest Tracker on Railway
Scheduled internet speed tests with history and alerts (alexjustesen/speedtest-tracker, ~6k★). No Railway template (deploy slug 404).
Note: tests measure Railway's datacenter link, so pitch it for monitoring remote/ISP endpoints via custom servers, or as an uptime/latency dashboard.
- Port 80, volume `/config`, healthcheck `/api/healthcheck`, SQLite by default (optional Postgres plugin).
## Variables
| Var | Value |
|---|---|
| APP_KEY | `base64:${{secret(32)}}` (Railway template secret generator; must be base64: prefixed 32-byte key) |
| APP_URL | `https://${{RAILWAY_PUBLIC_DOMAIN}}` |
| DB_CONNECTION | `sqlite` (or `pgsql` + DB_HOST=${{Postgres.PGHOST}}, DB_PORT, DB_DATABASE, DB_USERNAME, DB_PASSWORD) |
| PUID / PGID | `1000` / `1000` |
| SPEEDTEST_SCHEDULE | `0 */6 * * *` |
| TZ | `America/New_York` |
| PORT | `80` |
