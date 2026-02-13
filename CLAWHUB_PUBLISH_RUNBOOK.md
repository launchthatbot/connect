# ClawHub Publish Runbook

This runbook assumes your package is published to npm and listed in ClawHub.

## 1) Pre-flight

- Ensure `README.md`, `SECURITY.md`, and `CLAWHUB_LISTING_TEMPLATE.md` are current.
- Run:

```bash
pnpm --filter launchthat-openclaw-connector check:release
pnpm --filter launchthatbot test:openclaw:e2e
```

- Validate package contents:

```bash
cd packages/launchthat-openclaw-connector
npm pack --dry-run
```

## 2) Publish package

From repo root:

```bash
pnpm --filter launchthat-openclaw-connector build
pnpm --filter launchthat-openclaw-connector exec npm publish --access public
```

## 3) Create/Update ClawHub listing

- Open `https://clawhub.ai/` publisher dashboard.
- Copy content from `CLAWHUB_LISTING_TEMPLATE.md`.
- Fill platform-specific fields:
  - package name/version
  - category/tags
  - support contact
  - changelog/release notes
  - pricing plan metadata (if used)

## 4) Security metadata to include

- Outbound-only network behavior
- Data minimization guarantees
- Secret handling methods (env/file/prompt)
- Signature verification support
- Incident reporting channel from `SECURITY.md`

## 5) Post-publish verification

- Install package in a clean OpenClaw environment.
- Run auth-link flow and confirm:
  - heartbeat accepted
  - events ingest accepted
  - replay endpoint returns events
- Verify dashboard reflects connected instance in LaunchThatBot.

## 6) Rollback plan

- If release has an issue:
  - deprecate affected npm version
  - publish patched version
  - update ClawHub listing notes with affected version range and fix
