# InfluxDB

InfluxDB OSS 2 for time-series data and Flux queries.

- Address: `http://influxdb:8086`
- Organization and bucket: `geomind`
- Volumes: `geomind-influxdb-data` and `geomind-influxdb-config`
- Variables: `INFLUXDB_USERNAME`, `INFLUXDB_PASSWORD`, and `INFLUXDB_TOKEN`

```sh
cp .env.example .env
docker compose config -q
docker compose up -d
```

Initialization runs only on empty volumes. Use scoped application tokens and
reserve the operator token for administration.
