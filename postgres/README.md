# PostgreSQL

PostgreSQL 16 using the official Alpine image.

- Address: `postgres:5432`
- Default database: `geomind`
- Volume: `geomind-postgres-data`
- Variables: `POSTGRES_USER`, `POSTGRES_PASSWORD`, and `POSTGRES_DB`

```sh
cp .env.example .env
docker compose config -q
docker compose up -d
```

The initial user and database are created only on an empty volume. Create each
service's schemas and roles separately.
