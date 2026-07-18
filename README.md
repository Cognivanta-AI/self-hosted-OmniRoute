# Self-hosted OmniRoute for Coolify

<p align="center">
  <a href="https://github.com/diegosouzapw/OmniRoute">
    <img src="https://img.shields.io/badge/OmniRoute-self--hosted-7d7fe9" alt="OmniRoute self-hosted">
  </a>
  <img src="https://img.shields.io/badge/Coolify-one--click-38bdf8" alt="Coolify one-click deploy">
  <img src="https://img.shields.io/badge/Docker%20Compose-ready-111827" alt="Docker Compose ready">
  <img src="https://img.shields.io/badge/OpenAI-compatible-16a34a" alt="OpenAI compatible API">
  <img src="https://img.shields.io/badge/API-/v1-6BA539" alt="OpenAI-compatible /v1 endpoint">
</p>

Deploy [OmniRoute](https://github.com/diegosouzapw/OmniRoute) on Coolify in minutes.

This repository is a small, production-oriented Docker Compose wrapper for self-hosting OmniRoute: a unified AI gateway and OpenAI-compatible router for Codex CLI, Claude Code, Cursor, Cline, Gemini CLI, OpenCode, OpenAI SDKs, and other coding agents.

Use this repo when you want the fastest path from **GitHub repo -> Coolify -> public `/v1` AI endpoint**.

## Why this repo exists

The upstream OmniRoute project already provides Docker images. This repo focuses only on the deployment layer that self-hosters usually need:

- One clean `docker-compose.yaml` for Coolify.
- Public Docker image default: `diegosouzapw/omniroute:latest`.
- Persistent data volume at `/app/data`.
- Coolify-friendly service URL variable: `SERVICE_URL_OMNIROUTE_20128`.
- OpenAI-compatible public API at `/v1`.
- Sensible first-run security variables for `INITIAL_PASSWORD` and `JWT_SECRET`.
- No custom build required for normal deployments.

## Quick Coolify Deploy

1. Fork or clone this repository into your GitHub account.
2. In Coolify, create a new resource from the repo.
3. Select **Docker Compose**.
4. Keep the exposed port as `20128`.
5. Deploy.

After deploy, open your Coolify domain, connect at least one provider in OmniRoute, then create an endpoint key in **Dashboard -> Endpoints**.

Use this in any OpenAI-compatible client:

```text
Base URL: https://your-coolify-domain/v1
API Key:  [copy from OmniRoute Dashboard -> Endpoints]
Model:    auto
```

## Local Test

```bash
cp .env.example .env
docker compose --env-file .env up
```

Open:

```text
http://localhost:20128
http://localhost:20128/v1
```

Verify with an endpoint key created in the dashboard:

```bash
curl http://localhost:20128/v1/models \
  -H "Authorization: Bearer YOUR_OMNIROUTE_ENDPOINT_KEY"
```

## Coolify Environment

The Compose file is ready for Coolify defaults. For most deployments, these values are enough:

```env
PORT=20128
NEXT_PUBLIC_BASE_URL=https://your-coolify-domain
INITIAL_PASSWORD=change-this-password
JWT_SECRET=change-this-long-random-secret
```

Coolify can generate secure values automatically:

```env
INITIAL_PASSWORD=${SERVICE_PASSWORD_64_OMNIROUTE}
JWT_SECRET=${SERVICE_PASSWORD_64_OMNIROUTE_JWT}
```

`NEXT_PUBLIC_BASE_URL` should be your public HTTPS domain without `/v1`.

## Public Endpoints

```text
Dashboard: https://your-coolify-domain
API base:  https://your-coolify-domain/v1
Models:    https://your-coolify-domain/v1/models
```

OmniRoute is OpenAI-compatible, so most clients only need:

```text
https://your-coolify-domain/v1
```

## Docker Image

Default image:

```text
diegosouzapw/omniroute:latest
```

Pin a version or tag when you want reproducible deploys:

```env
OMNIROUTE_IMAGE=diegosouzapw/omniroute:3.8.48
```

## Persistent Data

Runtime data is stored inside the container at:

```text
/app/data
```

This repo maps it to:

```text
omniroute-data
```

Back up this Docker volume before migrating servers or recreating the Coolify resource.

## Use Cases

- Self-host OmniRoute on Coolify.
- Expose one OpenAI-compatible `/v1` endpoint for AI coding tools.
- Route requests across many LLM providers from one dashboard.
- Configure fallback chains for Codex CLI, Claude Code, Cursor, Cline, Gemini CLI, and OpenCode.
- Run a personal or team AI gateway on your own infrastructure.

## Keywords

OmniRoute Coolify deploy, self-hosted OmniRoute, OmniRoute Docker Compose, OmniRoute Docker, OpenAI-compatible proxy, AI gateway, LLM router, self-hosted AI router, Coolify AI gateway, Codex CLI OpenAI base URL, Claude Code OpenAI compatible endpoint, Cursor AI gateway.

## Upstream

This repository is a deployment template only. OmniRoute itself belongs to its upstream authors:

- Upstream project: <https://github.com/diegosouzapw/OmniRoute>
- Docker image: `diegosouzapw/omniroute`

## Support

If this wrapper saves you time, you can support my work here:

<p>
  <a href="https://www.buymeacoffee.com/genildof">
    <img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height:60px;width:217px;">
  </a>
</p>

<p>
  <img src="assets/bmc_qr.png" alt="Buy Me A Coffee QR Code" width="180">
</p>

## License

This deployment wrapper is released under the license in this repository. Check the upstream OmniRoute project for OmniRoute's own license and terms.
