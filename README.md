# Swarmax infrastructure

`compose.yml` here is the source of truth for running the whole stack on one machine.
Each directory is an independent Docker Compose project for the server deployment
behind Traefik.

## Whole stack

`compose.yml` starts PostgreSQL, Redis, InfluxDB, MinIO, ChromaDB, the API and the
Celery worker on a private network. It needs no external network and no Traefik, and
it needs `../backend` and `../gemini` next to this folder. The `api` service runs the
database migrations before it starts.

```sh
cp .env.example .env
docker compose config -q
docker compose up -d --build
curl http://localhost:8000/api/v1/system/health
```

The `frontend` service is behind the `frontend` profile because `../frontend` has no
Dockerfile yet. Once it has one, start it with `docker compose --profile frontend up -d`.
Do not point `docker compose up` at it before then: the build fails.

`docker compose down -v` deletes the data of the stack.

### Public addresses

`compose.traefik.yml` puts the stack behind the Traefik in `traefik/`:

| Service | Address |
| --- | --- |
| Frontend | `https://geomind.team` |
| API | `https://api.geomind.team` |
| Traefik dashboard | `https://traefik.geomind.team/dashboard/` |

```sh
docker network inspect traefik >/dev/null 2>&1 || docker network create traefik
docker compose -f compose.yml -f compose.traefik.yml config -q
docker compose -f compose.yml -f compose.traefik.yml up -d --build
```

Add `https://geomind.team` to `APP_CORS_ORIGINS` in `.env`.

## Server deployment

| Service | Container | Address |
| --- | --- | --- |
| [Traefik](traefik/README.md) | `swarmax-traefik` | `https://traefik.geomind.team` |
| [PostgreSQL](postgres/README.md) | `swarmax-postgres` | `postgres:5432` |
| [Redis](redis/README.md) | `swarmax-redis` | `redis:6379` |
| [MinIO](minio/README.md) | `swarmax-minio` | `minio:9000` |
| [pgAdmin](pgadmin/README.md) | `swarmax-pgadmin` | `https://pgadmin.geomind.team` |
| [ChromaDB](chromadb/README.md) | `swarmax-chromadb` | `chromadb:8000` |
| [InfluxDB](influxdb/README.md) | `swarmax-influxdb` | `influxdb:8086` |
| [Cloudflare Tunnel](cloudflared/README.md) | `swarmax-cloudflared` | outbound only |

Create the external networks once:

```sh
docker network inspect traefik >/dev/null 2>&1 || docker network create traefik
docker network inspect swarmax >/dev/null 2>&1 || docker network create swarmax
```

Start a service from its directory:

```sh
cp .env.example .env
docker compose config -q
docker compose up -d
```

Never commit `.env` files. Data is stored in named volumes; `docker compose down
-v` deletes that service's data.
