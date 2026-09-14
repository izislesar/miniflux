# miniflux selfhost notes

App + postgres:16-alpine. ~120mb ram together. Manual runs, no autostart.

## up

```bash
cp selfhost/.env.example selfhost/.env
# set DB_PASSWORD and ADMIN_PASSWORD
docker compose -f selfhost/docker-compose.yml --env-file selfhost/.env up -d
```

open http://localhost:8082. migrations run automatically (`RUN_MIGRATIONS=1`).

## backup

db dump is enough, the app is stateless:

```bash
docker exec miniflux-db pg_dump -U miniflux miniflux > backups/miniflux-$(date +%F).sql
```

restore: fresh `up -d`, then `psql` the dump back in.

## update

```bash
docker compose -f selfhost/docker-compose.yml --env-file selfhost/.env pull
docker compose -f selfhost/docker-compose.yml --env-file selfhost/.env up -d
docker logs miniflux | tail -20   # check migrations went through
```
