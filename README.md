# Railway Templates

One-click Railway deploy templates for open-source self-hosted tools:

| Template | Folder |
|---|---|
| Hister | [`hister/`](hister/) |
| Speedtest Tracker | [`speedtest-tracker/`](speedtest-tracker/) |
| Kener | [`kener/`](kener/) |
| Homebox | [`homebox/`](homebox/) |

Each folder has a `Dockerfile`, a `railway.json`, and a README listing the required variables and volumes. Secrets are generated at deploy time with Railway's `${{secret()}}` helper; no credentials are stored here.

## Usage
Deploy from the Railway template marketplace, or create a Railway project from this repo and set the service root directory to a template folder.
