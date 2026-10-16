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

## If the deploy fails
Most likely failure: **permission denied writing `/hister/data`**. The image runs as uid 65532, and Railway volumes mount as root. Fix: set the service variable `RAILWAY_RUN_UID=0` and redeploy, or switch the image tag to `latest-root`. If the healthcheck fails instead, check that the public domain's target port is 4433.

## Expected monthly cost on Railway
Railway bills usage on top of the plan: RAM $10/GB-month, CPU $20/vCPU-month, volume $0.15/GB-month, egress $0.05/GB. The Hobby plan is $5/month and includes $5 of usage ([pricing](https://docs.railway.com/reference/pricing/plans)). An idle single-user instance at about 0.25 GB RAM and near-zero CPU works out to roughly $2.50-3/month plus about $0.15 per GB of volume, so it usually fits inside the Hobby plan's included $5. This is an estimate from those rates, not a measured bill. Check the project's Usage tab after a day.
