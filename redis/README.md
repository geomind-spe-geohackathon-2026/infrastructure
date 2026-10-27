# Redis

Redis 7 with AOF persistence and ACLs on the private `swarmax` network.

- Address: `redis:6379`
- Volume: `swarmax-redis-data`
- Variable: `REDIS_PASSWORD`

```sh
cp .env.example .env
docker compose config -q
docker compose up -d
```

Create a restricted ACL user for each service and run `ACL SAVE` after changing
users.
