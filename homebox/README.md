# Homebox on Railway
Home inventory & asset tracker (sysadminsmedia/homebox, ~7.5k★). Existing template: 3 deploys, no Postgres option. This one adds stable secrets, registration lock, and optional Postgres.
- Port 7745, volume `/data`, healthcheck `/api/v1/status`
## Variables
| Var | Value |
|---|---|
| HBOX_WEB_PORT / PORT | `7745` |
| HBOX_AUTH_API_KEY_PEPPER | `${{secret(32)}}` (must never change) |
| HBOX_OPTIONS_ALLOW_REGISTRATION | `true` for first signup, then set `false` |
| HBOX_DATABASE_DRIVER | `sqlite3` (or `postgres` + HBOX_DATABASE_HOST/PORT/USERNAME/PASSWORD/DATABASE from ${{Postgres.*}}) |
| TZ | `America/New_York` |

## If the deploy fails
Most likely failure: **data disappears after a redeploy, or the healthcheck fails**. Either no volume is mounted at `/data`, or the public domain targets the wrong port. Fix: attach a volume at `/data`, then set the domain's target port to `7745` (the healthcheck is `/api/v1/status`). If you switched to a `-rootless` or `-hardened` tag and see `attempt to write a readonly database`, the image runs as uid 65532 against a root-owned volume. Set `RAILWAY_RUN_UID=0` or go back to the regular tag. Don't change `HBOX_AUTH_API_KEY_PEPPER` after setup, because that invalidates existing API keys. Sources: [Homebox install docs](https://homebox.software/en/quick-start/install/), [issue #449](https://github.com/sysadminsmedia/homebox/issues/449).

## Expected monthly cost on Railway
Railway bills usage on top of the plan: RAM $10/GB-month, CPU $20/vCPU-month, volume $0.15/GB-month, egress $0.05/GB. The Hobby plan is $5/month and includes $5 of usage ([pricing](https://docs.railway.com/reference/pricing/plans)). A single Go binary with SQLite idles at about 0.05-0.1 GB RAM: roughly $1-2/month plus $0.15/GB for photos on the volume. It fits easily inside the Hobby plan's included $5. This is an estimate from those rates, not a measured bill. Check the project's Usage tab after a day.
