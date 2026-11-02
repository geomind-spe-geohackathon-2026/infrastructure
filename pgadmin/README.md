# pgAdmin

PostgreSQL administration UI at `https://pgadmin.geomind.team`.

- Database server: `postgres:5432`
- Volume: `swarmax-pgadmin-data`
- Variables: `PGADMIN_DEFAULT_EMAIL` and `PGADMIN_DEFAULT_PASSWORD`
- Optional IP restriction: `PGADMIN_ALLOWED_CIDRS`

```sh
cp .env.example .env
docker compose config -q
docker compose up -d
```

The initial account is created only when the volume is empty.
