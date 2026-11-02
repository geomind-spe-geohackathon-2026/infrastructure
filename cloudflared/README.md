# Cloudflare Tunnel

Publishes Traefik to the internet from a machine without a public IP. The
connection is outbound only, so no port has to be opened.

- Container: `swarmax-cloudflared`
- Network: `traefik`
- Variable: `TUNNEL_TOKEN`

## Set up once in the Cloudflare dashboard

1. Zero Trust > Networks > Tunnels > Create a tunnel, type Cloudflared. Copy the
   connector token into `.env` as `TUNNEL_TOKEN`.
2. Add one public hostname per address, each with service `https://swarmax-traefik:443`
   and, under TLS, Origin Server Name set to the same hostname:
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
