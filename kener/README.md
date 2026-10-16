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
