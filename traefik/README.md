# Traefik

Geomind ingress proxy using Traefik `3.7.16` on ports 80 and 443.

- Dashboard: `https://traefik.geomind.team/dashboard/`
- Network: `traefik`
- Variables: `TRAEFIK_DASHBOARD_AUTH`, `CF_DNS_API_TOKEN`

```sh
docker network inspect traefik >/dev/null 2>&1 || docker network create traefik
mkdir -p certs
touch certs/acme.json
chmod 600 certs/acme.json
cp .env.example .env
docker compose config -q
docker compose up -d
```

Generate dashboard credentials with `htpasswd -nb admin`. The ignored
`certs/acme.json` contains certificate private keys; keep mode `600` and back it up.

Certificates come from Let's Encrypt through the Cloudflare DNS challenge, so the
machine needs no public IP or open port 80. `CF_DNS_API_TOKEN` is a Cloudflare API
token with Zone > DNS > Edit on `geomind.team` only. Traffic reaches Traefik through
the [Cloudflare Tunnel](../cloudflared/README.md).
