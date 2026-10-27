# Swarmax infrastructure

Each directory is an independent Docker Compose project.

| Service | Container | Address |
| --- | --- | --- |
| [Traefik](traefik/README.md) | `swarmax-traefik` | `https://traefik.swarmax.xyz` |
| [PostgreSQL](postgres/README.md) | `swarmax-postgres` | `postgres:5432` |
| [Redis](redis/README.md) | `swarmax-redis` | `redis:6379` |
| [MinIO](minio/README.md) | `swarmax-minio` | `minio:9000` |
| [pgAdmin](pgadmin/README.md) | `swarmax-pgadmin` | `https://pgadmin.swarmax.xyz` |
| [ChromaDB](chromadb/README.md) | `swarmax-chromadb` | `chromadb:8000` |
| [InfluxDB](influxdb/README.md) | `swarmax-influxdb` | `influxdb:8086` |

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
