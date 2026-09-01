# htoochoon-builder

The single CI delivery source for the active HtooChoon Engram Flutter app.
The private application source stays in `Walker-00/htoochoon-engram`; this
public repository owns its Android build workflow and no longer builds the
retired Flutter client.

## Release flow

```text
Engram rust branch
       ↓
Builder validation + Android APK build
       ↓
nightly
       ↓ Founder approval
beta
       ↓ Founder approval
production (`latest` download compatibility path)
```

`.github/workflows/engram.yaml` runs manually, whenever Builder's `rust`
branch changes, and nightly at 00:00 Asia/Yangon. A successful run uploads only
the universal and per-ABI Android APKs to
`https://backend.htoochoon.com/download/nightly/`.

CI has no route that writes to beta or production. Promotions are authenticated
Founder operations performed by the HtooChoon Control Center. The backend
validates the complete source manifest and non-empty artifacts, snapshots the
previous destination release for recovery, then switches the destination
channel atomically.

## GitHub Actions secrets

| Secret | Required | Purpose |
|---|---:|---|
| `ENGRAM_DEPLOY_KEY` | Yes | Read-only deploy key for `Walker-00/htoochoon-engram` |
| `RELEASE_UPLOAD_KEY` | Yes | Must equal the backend `RELEASE_UPLOAD_KEY` |

The old optional Cloudflare purge secrets are no longer needed in Builder.
Cache purging belongs to the backend promotion operation, where the channel
actually changes.
