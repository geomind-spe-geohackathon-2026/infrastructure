# SearXNG

Self-hosted web search for the geomind agents' `web_search` tool (feature spec
`.claude/notes/features/agent-tools-2026-11-05.md`, section 4). JSON output is on; the limiter is
off because only the backend calls it.

It joins only the internal `geomind` network: no published port and no Traefik route, so it is
never public. The backend reaches it at `http://searxng:8080` and sets
`GEOMIND_SEARCH_URL=http://searxng:8080` in `../../backend/.env`.

```sh
cp .env.example .env            # then set SEARXNG_SECRET, e.g. openssl rand -hex 32
docker compose config -q
docker compose up -d
# check from the backend's network:
docker exec geomind-fastapi python -c "import urllib.request,json; print(len(json.load(urllib.request.urlopen('http://searxng:8080/search?q=geothermal&format=json', timeout=20))['results']))"
```

The image is pinned (`searxng/searxng:2026.11.2-cccbc9e15`); `settings.yml` is mounted read-only.
