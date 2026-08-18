# htoochoon-builder

Builds HtooChoon apps that don't carry their own CI. Each app is checked out
here via a read-only deploy key instead of hosting the workflow itself.

## engram (new-ui-ux)

`.github/workflows/engram.yaml` builds `git@github.com:Walker-00/htoochoon-engram.git`
(branch `rust`) — Android, Linux, Windows, macOS, iOS, Web — and uploads to the
backend's `new-ui-ux` release channel, served at
`https://backend.htoochoon.com/download/new-ui-ux/<file>`. Separate from the
production app's `latest` channel (built by `htoochoon-flutter`'s own
workflow) — both install side by side, different Android `applicationId`.

### Secrets required (Settings → Secrets and variables → Actions)

| Secret | What |
|---|---|
| `ENGRAM_DEPLOY_KEY` | Read-only SSH deploy key for `htoochoon-engram` (add the matching public key as a Deploy Key on that repo) |
| `RELEASE_UPLOAD_KEY` | Same value as `RELEASE_UPLOAD_KEY` on the backend |
| `CF_API_TOKEN` / `CF_ZONE_ID` | Optional — Cloudflare cache purge after each release |

Trigger: `workflow_dispatch` (manual) or a push to this repo's `rust` branch.
Pushing to `htoochoon-engram` itself does NOT trigger a build — there's no
webhook between the two repos yet.
