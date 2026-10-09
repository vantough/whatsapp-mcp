# Install and privacy guide v1.0

## What this adds

The static frontend is served by the existing local Go bridge. The bridge syncs the account owner's WhatsApp contact-store snapshot and direct-chat identities into an additive local SQLite directory. Searches use the local endpoint and do not transmit contact data to a hosted service. Phone-number JIDs and LIDs are recorded as identity aliases when WhatsApp exposes their mapping. Saved names take priority over profile and business names.

## Compatibility and safe application

This patch targets `vantough/whatsapp-mcp` revision `06624d6ba750d4f41eda2a7d988c4a008c9ac014` and its current whatsmeow API. In the colleague's clone:

1. Confirm the exact repo and inspect its working tree. Keep any existing changes.
2. Check the patch before applying it: `git apply --check references/bridge-enhancements.patch` (run from the repo root after locating this file).
3. If the check fails, stop and port the focused changes to the checked-out source. Do not force-apply or discard local edits.
4. Copy the three files from `assets/frontend/` into the repo's `frontend/` directory after backing up any existing frontend edits.
5. Build the Go bridge using the version in `whatsapp-bridge/go.mod`. Confirm the SQLite CGO prerequisite for the colleague's OS.
6. Start the bridge in the foreground for the owner's QR pairing, then verify the viewer route and a read-only `GET /api/contacts?q=` response before configuring background startup.

The bridge-side patch also contains supporting local viewer changes present in the source revision. Review the patch and the final diff before build or launch.

## Personal data and sharing boundary

Do not put `whatsapp-bridge/store/`, any `*.db` file, QR image, media directory, logs, `.env` file, or personalized LaunchAgent plist in a shared archive or source commit. Each colleague pairs their own WhatsApp account and generates private data locally. Keep the local viewer bound to localhost unless the account owner explicitly chooses and secures another access model.

Directory lookup is a local convenience, not a substitute for checking the recipient identity before sending. This kit does not authorize sending messages, uploading contacts, or publishing a colleague's changes.

## Verification checklist

- `gofmt` and `go build` succeed for the bridge.
- `node --check frontend/app.js` and `git diff --check` succeed.
- The existing local viewer returns HTTP 200.
- `GET /api/contacts?q=` returns JSON from that machine's local directory.
- Search can match a saved name, a WhatsApp name, a phone number, and a linked LID/phone identity.
- Restart persistence is checked separately for the colleague's OS. A running foreground process does not prove background or reboot persistence.
