# Cloudflare Tunnel

Publishes Traefik to the internet from a machine without a public IP. The
connection is outbound only, so no port has to be opened.

- Container: `geomind-cloudflared`
- Network: `traefik`
- Variable: `TUNNEL_TOKEN`

## Set up once in the Cloudflare dashboard

1. dash.cloudflare.com > Networking > Tunnels > Create a tunnel, type Cloudflared
   (in Zero Trust: Networks > Connectors). Do not run the install command it shows;
   copy only the token in it (starts with `eyJ`) into `.env` as `TUNNEL_TOKEN`.
2. Under Routes, add one published application per address, each with service
   `https://geomind-traefik:443` and, under TLS, Origin Server Name set to the same
   hostname:
   `geomind.team`, `api.geomind.team`, `traefik.geomind.team` (and `pgadmin`,
   `minio`, `minio-console` if they should be public).
   Cloudflare creates the DNS records.

## Start

Start Traefik first (`../traefik`), then:

```sh
cp .env.example .env
docker compose config -q
docker compose up -d
docker compose logs -f cloudflared
```
