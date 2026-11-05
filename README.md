# Geomind infrastructure

Shared services only: the proxy, the tunnel and the data stores. The applications
deploy from their own repositories, each with its own `compose.yml` and `.env`:

- `../backend`: API and worker, joins the `geomind` and `traefik` networks
- `../frontend`: web app, joins the `traefik` network

Every service here reaches the applications through those two external networks, so
create them once:

```sh
docker network inspect traefik >/dev/null 2>&1 || docker network create traefik
docker network inspect geomind >/dev/null 2>&1 || docker network create geomind
```

## Public addresses

| Service | Address | Deployed from |
| --- | --- | --- |
| Frontend | `https://geomind.team` | `../frontend` |
| API | `https://api.geomind.team` | `../backend` |
| Traefik dashboard | `https://traefik.geomind.team/dashboard/` | `traefik/` |

Start `traefik/` and `cloudflared/` first, then the data services, then the backend and
the frontend.

## Server deployment

| Service | Container | Address |
| --- | --- | --- |
| [Traefik](traefik/README.md) | `geomind-traefik` | `https://traefik.geomind.team` |
| [PostgreSQL](postgres/README.md) | `geomind-postgres` | `postgres:5432` |
| [Redis](redis/README.md) | `geomind-redis` | `redis:6379` |
| [MinIO](minio/README.md) | `geomind-minio` | `minio:9000` |
| [pgAdmin](pgadmin/README.md) | `geomind-pgadmin` | `https://pgadmin.geomind.team` |
| [ChromaDB](chromadb/README.md) | `geomind-chromadb` | `chromadb:8000` |
| [InfluxDB](influxdb/README.md) | `geomind-influxdb` | `influxdb:8086` |
| [Cloudflare Tunnel](cloudflared/README.md) | `geomind-cloudflared` | outbound only |
| [SearXNG](searxng/README.md) | `geomind-searxng` | `searxng:8080` (internal only) |
| [Maintenance page](maintenance/README.md) | `geomind-maintenance` | fallback for `geomind.team` and `api.geomind.team` during a deploy |

Start a service from its directory:

```sh
cp .env.example .env
docker compose config -q
docker compose up -d
```

Never commit `.env` files. Data is stored in named volumes; `docker compose down
-v` deletes that service's data.
