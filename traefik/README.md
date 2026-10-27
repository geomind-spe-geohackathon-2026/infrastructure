# Traefik

Swarmax ingress proxy using Traefik `3.7.16` on ports 80 and 443.

- Dashboard: `https://traefik.swarmax.xyz/dashboard/`
- Network: `traefik`
- Variable: `TRAEFIK_DASHBOARD_AUTH`

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
