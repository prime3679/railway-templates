# Hister on Railway
Self-hosted full-text search over your own browsing history (asciimoo/hister, ~6k★). No Railway template exists (railway.com/deploy/hister = 404).
## Template service
- Source: this folder (Dockerfile pins `ghcr.io/asciimoo/hister:v0.20.0`) or image directly.
- Port: 4433 (set `PORT=4433` / target port 4433 on the public domain)
- Volume: mount at `/hister/data` (image runs as uid 65532; if permission errors, set `RAILWAY_RUN_UID=0` or use `latest-root` tag)
- Healthcheck: `/health`
## Variables
| Var | Value |
|---|---|
| HISTER__SERVER__ADDRESS | `0.0.0.0:4433` |
| HISTER__SERVER__BASE_URL | `https://${{RAILWAY_PUBLIC_DOMAIN}}` |
| PORT | `4433` |
After deploy, install the Hister browser extension and point it at your Railway URL. Est. cost: ~$1-3/mo usage.
