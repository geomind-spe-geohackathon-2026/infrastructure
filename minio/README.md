# MinIO

S3-compatible object storage.

- Private API: `http://minio:9000`
- Public API: `https://minio.geomind.team`
- Admin console: `https://minio-console.geomind.team`
- Volume: `geomind-minio-data`
- Variables: `MINIO_ROOT_USER` and `MINIO_ROOT_PASSWORD`

```sh
cp .env.example .env
docker compose config -q
docker compose up -d
```

Create buckets and application users after startup. Give each application its
own restricted credentials.
