# Google OAuth Credentials (No Hardcoding)

Antigravity Manager now reads Google OAuth credentials from environment variables at runtime (instead of hardcoding them in source code).

## Required Environment Variables

Recommended:

- `GOOGLE_CLIENT_ID`
- `GOOGLE_CLIENT_SECRET`

Legacy fallback (supported, but not recommended):

- `CLIENT_ID`
- `CLIENT_SECRET`

If these are missing or empty, the OAuth flow will return an error indicating which variable(s) must be set.

## Docker (docker run)

Use `--env-file` (preferred) so secrets do not appear in your shell history:

```bash
cat > antigravity.env <<'EOF'
GOOGLE_CLIENT_ID=your-client-id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your-client-secret
EOF
chmod 600 antigravity.env

docker run -d --name antigravity-manager \
  -p 8045:8045 \
  --env-file ./antigravity.env \
  -e API_KEY=your-api-key \
  -v ~/.antigravity_tools:/root/.antigravity_tools \
  antigravity-manager:latest
```

## Docker Compose

Add the variables either via an `.env` file or via your shell environment.

Example `docker-compose.yml` snippet:

```yaml
services:
  antigravity-manager:
    environment:
      - GOOGLE_CLIENT_ID=${GOOGLE_CLIENT_ID}
      - GOOGLE_CLIENT_SECRET=${GOOGLE_CLIENT_SECRET}
```

If you use a `.env` file with Docker Compose, keep it private and do not commit it.

