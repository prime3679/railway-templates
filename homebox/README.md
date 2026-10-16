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
