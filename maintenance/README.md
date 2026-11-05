# Maintenance page

During a deploy the app's container is replaced and, for a minute, Traefik had no router for
`geomind.team` and answered a bare `404 page not found` (QA-060). This service holds a router for
`geomind.team` and `api.geomind.team` with priority 1, the lowest, so it answers only while the
app's own router is gone:

- the site: `maintenance.html` (503, reloads itself every 10 s);
- the API: `503 {"error": {"code": "unavailable", ...}}`, the backend's error shape.

Both carry `Retry-After: 10` and `Cache-Control: no-store`. No `.env` is needed.

```sh
docker compose config -q
docker compose up -d
```
