# launchthat-openclaw-connector

Minimal OpenClaw connector for LaunchThatBot MVP.

## Quick start

1. Generate auth link:

```bash
pnpm --filter launchthat-openclaw-connector build
pnpm --filter launchthat-openclaw-connector exec lt-openclaw-connect auth-link \
  --base-url=http://localhost:3000 \
  --workspace-id=default \
  --instance-name=my-openclaw
```

2. Open the returned `authUrl`, copy `instanceId` + `ingestToken`.

3. Run connector:

```bash
export LAUNCHTHAT_INGEST_TOKEN="<token>"
export LAUNCHTHAT_SIGNING_SECRET="<shared-signing-secret>"
pnpm --filter launchthat-openclaw-connector exec lt-openclaw-connect run \
  --base-url=http://localhost:3000 \
  --workspace-id=default \
  --instance-id=<instanceId>
```

The connector sends heartbeat pings and batches events to LaunchThatBot. It keeps a
small local queue on disk if delivery fails and retries with exponential backoff.

## Security defaults

- Prefer `LAUNCHTHAT_INGEST_TOKEN` env var instead of CLI args.
- Optional HMAC request signing is supported with `LAUNCHTHAT_SIGNING_SECRET`.
- Queue file defaults to `~/.config/launchthat-openclaw/queue.json` with restrictive
  permissions.
- Set `--persist-queue=false` if local queueing is not allowed in your environment.

## CLI options

- `run --ingest-token-env=<VAR>` read ingest token from env var (default `LAUNCHTHAT_INGEST_TOKEN`)
- `run --ingest-token-file=/path/to/token` read ingest token from file
- `run --signing-secret-env=<VAR>` read signing secret from env var (default `LAUNCHTHAT_SIGNING_SECRET`)
- `run --signing-secret-file=/path/to/secret` read signing secret from file
- `run --persist-queue=false` disable disk queue persistence

## Publishing assets

- MoltHub listing draft: `MOLTHUB_LISTING_TEMPLATE.md`
- Release checklist: `PRE_PUBLISH_SECURITY_CHECKLIST.md`
- Vulnerability reporting: `SECURITY.md`
