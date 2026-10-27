# ChromaDB

ChromaDB `1.3.5` vector database on the private `swarmax` network.

- Address: `http://chromadb:8000`
- Volume: `swarmax-chromadb-data`
- Environment variables: none

```sh
docker compose config -q
docker compose up -d
docker compose ps
```

This service has no public port or authentication. Connect only trusted services
to the `swarmax` network.
