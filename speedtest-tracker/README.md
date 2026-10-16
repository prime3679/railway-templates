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

## If the deploy fails
Most likely failure: **a 500 Server Error, with `No application encryption key has been specified` in the logs**. The `APP_KEY` variable is missing or malformed. Fix: set `APP_KEY` to a value that starts with `base64:` followed by a 32-byte base64 key (the template generates `base64:${{secret(32)}}`), and don't wrap it in quotes. You can also open a shell in the service, run `php artisan key:generate --show`, paste the output, and redeploy. If an old `.env` exists on the `/config` volume with an empty `APP_KEY`, delete it. Sources: [issue #894](https://github.com/alexjustesen/speedtest-tracker/issues/894), [issue #711](https://github.com/alexjustesen/speedtest-tracker/issues/711).

## Expected monthly cost on Railway
Railway bills usage on top of the plan: RAM $10/GB-month, CPU $20/vCPU-month, volume $0.15/GB-month, egress $0.05/GB. The Hobby plan is $5/month and includes $5 of usage ([pricing](https://docs.railway.com/reference/pricing/plans)). With SQLite and a test every 6 hours, it idles at about 0.2-0.3 GB RAM with short CPU bursts during tests: roughly $2-4/month. Every speed test also downloads and uploads data, and that counts as egress ($0.05/GB), so long tests or tight schedules add cost. Add $5-10/month if you add the Postgres plugin. This is an estimate from those rates, not a measured bill. Check the project's Usage tab after a day.
