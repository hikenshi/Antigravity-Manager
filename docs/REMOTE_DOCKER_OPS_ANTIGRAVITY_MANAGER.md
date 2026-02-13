# Remote Ops: Antigravity Manager (Docker Compose)

This guide assumes Antigravity Manager is deployed on the remote server in:

`~/antigravity-manager`

and is managed by `docker compose` with:

- `docker-compose.yml`
- `.env`
- `data/` (persistent volume mapped to `/root/.antigravity_tools` inside the container)

## SSH Into The Server

```bash
ssh cucai@192.168.2.12
cd ~/antigravity-manager
```

If you prefer one-liners from your local machine, just prefix commands with:

```bash
ssh cucai@192.168.2.12 'cd ~/antigravity-manager && <COMMAND>'
```

## Check Status

```bash
cd ~/antigravity-manager
docker compose ps
docker ps --filter name=antigravity-manager
```

## Start

```bash
cd ~/antigravity-manager
docker compose up -d
```

## Stop

Stops and removes the container, but keeps your persistent data in `./data`.

```bash
cd ~/antigravity-manager
docker compose down
```

## Restart

```bash
cd ~/antigravity-manager
docker compose restart
```

If you changed `docker-compose.yml` or `.env`, recreate the container:

```bash
cd ~/antigravity-manager
docker compose up -d --force-recreate
```

## View Logs

Tail logs:

```bash
cd ~/antigravity-manager
docker compose logs -f --tail 200
```

Show recent logs and exit:

```bash
cd ~/antigravity-manager
docker compose logs --tail 200
```

Container-only logs (equivalent):

```bash
docker logs -f --tail 200 antigravity-manager
```

## Upgrade Image

```bash
cd ~/antigravity-manager
docker compose pull
docker compose up -d
```

Optional cleanup (safe, does not delete running containers):

```bash
docker image prune -f
```

## Where Credentials Live

Environment file (generated at deploy time):

```bash
cd ~/antigravity-manager
sed -n '1,120p' .env
```

Notes:

- `API_KEY` is used to authenticate API requests.
- `WEB_PASSWORD` is used to log into the Web UI (if set).

## Change Port / Keys

1. Edit `~/antigravity-manager/.env`
2. Recreate the container

```bash
cd ~/antigravity-manager
nano .env
docker compose up -d --force-recreate
```

Common `.env` keys:

```text
HOST_PORT=8045
API_KEY=...
WEB_PASSWORD=...
```

## Quick HTTP Check (From The Server)

```bash
curl -sS -o /dev/null -w "http:%{http_code}\n" http://127.0.0.1:8045/
curl -sS -o /dev/null -w "v1:%{http_code}\n" http://127.0.0.1:8045/v1
```

## Data Backup

The important state is stored in:

`~/antigravity-manager/data/`

Example tarball backup:

```bash
cd ~/antigravity-manager
tar -czf antigravity-manager-data-$(date +%F).tgz data
```

