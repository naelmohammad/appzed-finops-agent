# AppZed FinOps Agent

Spend findings across clouds with a findings dashboard (findings, monitoring, config, admin). Tables self-initialize.

[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/naelmohammad/appzed-finops-agent)

Click **Deploy to Cloudflare**: it clones this repo to your GitHub account, provisions the bindings below on your Cloudflare account, and wires CI/CD with preview URLs on every PR.

## Bindings (auto-provisioned)

```jsonc
{
  "$schema": "node_modules/wrangler/config-schema.json",
  "name": "appzed-finops-agent",
  "main": "worker.js",
  "compatibility_date": "2025-09-01",
  "ai": {
    "binding": "AI"
  },
  "d1_databases": [
    {
      "binding": "DB",
      "database_name": "appzed-finops-agent-db"
    }
  ],
  "triggers": {
    "crons": [
      "15 8 * * 1"
    ]
  }
}
```

## Configure

Set `CF_API_TOKEN` and `CF_ACCOUNT_ID` after deploy. Ingest other clouds via `POST /api/findings`.

```bash
wrangler secret put CF_API_TOKEN
```

Set `CF_ACCOUNT_ID` in `wrangler.jsonc` vars or as a secret.

## AWS live collection

`POST /api/collect/aws` scans EC2/S3 with SigV4 (read-only IAM key). Set secrets first:

```bash
wrangler secret put AWS_ACCESS_KEY_ID
wrangler secret put AWS_SECRET_ACCESS_KEY
```

Optional var: `AWS_REGION` (default `us-east-1`).

## Try it

```bash
curl -X POST https://<your-worker>.workers.dev/api/collect
```

GET /health for a liveness check. Managed by AppZed (https://dev.appzed.com/solutions/finops-agent).
