# Kener status page on Railway (v4, official image)
Kener (rajnandan1/kener, ~5.2k★). Existing Railway template has 1 deploy and builds from a community fork; this uses the official image and v4's required Redis.
## Services
1. **kener** (this folder / `rajnandan1/kener:4.1.7`), port 3000, volume `/app/database`, healthcheck `/healthcheck`
2. **Redis** (Railway Redis template)
3. Optional **Postgres** instead of SQLite
## Variables (kener)
| Var | Value |
|---|---|
| KENER_SECRET_KEY | `${{secret(48)}}` |
| ORIGIN | `https://${{RAILWAY_PUBLIC_DOMAIN}}` |
| REDIS_URL | `${{Redis.REDIS_URL}}` |
| DATABASE_URL | `sqlite://./database/kener.sqlite.db` (or `${{Postgres.DATABASE_URL}}`) |
| PORT | `3000` |

## If the deploy fails
Most likely failure: **login, signup or settings return `Cross-site POST form submissions are forbidden`**. `ORIGIN` doesn't exactly match the URL in your browser. Fix: set `ORIGIN` to the public URL with `https://` and no trailing slash (the template uses `https://${{RAILWAY_PUBLIC_DOMAIN}}`). If you add a custom domain, update `ORIGIN` to it and redeploy. Second most likely: the service crash-loops because `REDIS_URL` is empty. Kener v4 requires Redis, so deploy the Redis service and reference `${{Redis.REDIS_URL}}`. Sources: [Kener env docs](https://kener.ing/docs/v4/setup/environment-variables), [SvelteKit adapter-node](https://svelte.dev/docs/kit/adapter-node).

## Expected monthly cost on Railway
Railway bills usage on top of the plan: RAM $10/GB-month, CPU $20/vCPU-month, volume $0.15/GB-month, egress $0.05/GB. The Hobby plan is $5/month and includes $5 of usage ([pricing](https://docs.railway.com/reference/pricing/plans)). Two services: Kener at about 0.25-0.4 GB RAM and Redis at about 0.05-0.1 GB, plus monitor CPU. Roughly $4-6/month, which can run slightly past the Hobby plan's included $5. This is an estimate from those rates, not a measured bill. Check the project's Usage tab after a day.
