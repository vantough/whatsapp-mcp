---
name: local-whatsapp-mcp
description: Set up and operate a self-hosted WhatsApp MCP bridge on this computer, with QR or phone-code pairing, isolated multiple accounts, contact sync, and an optional local message viewer. Use for local WhatsApp login, bridge status/recovery, account management, or viewer setup. No GitHub account or clone is required.
---

# Local WhatsApp MCP

Kit version: 1.3.

This skill contains the complete tracked WhatsApp MCP source, bridge, Python MCP server, viewer, lockfiles, and setup references in `assets/whatsapp-mcp-source/`. Never clone or access GitHub. Read [the setup and privacy guide](references/INSTALL_v1.3.md) when installing or changing the runtime. Preserve existing files and local data.

## Setup

For `/setup`, use a mode already specified by the user or config. Otherwise ask one question: “Backend only (bridge and login) or backend plus the optional local message viewer?” Use the configurable defaults in `references/setup-config_v1.3.example.json`, then install from bundled source. Build dependencies may download from Go's module proxy and the configured Python package index, but never require GitHub credentials. Default to user-level startup, port 8080, QR pairing, and no optional MCP-client configuration unless requested. Verify connection, selected routes, and startup configuration. Pair only the account owner's own number.

## Command-like requests

Treat these as workflow aliases when the user types them, not as shell binaries. Ask only for a missing choice that matters, then carry out safe local steps and verify the result.

- `/help`: explain available actions and current mode.
- `/setup [backend|viewer]`: install or repair the selected components. If no mode is supplied, ask backend only or backend plus viewer.
- `/login [account] [qr|phone]`: select an existing account or create a named isolated profile. If more than one exists and none is specified, ask which profile; for a new profile ask its name. Each profile must use its own session database, message/contact database, media directory, port, and user-level service. Default to QR. For `phone`, ask for the international-format phone number and use the bridge's linked-device pairing code. This is not an SMS OTP. Show the generated code from the selected profile's startup log and tell the owner to enter it in WhatsApp > Linked devices > Link a device > Link with phone number instead. Do not echo or persist the phone number beyond the process environment/config needed for that one pairing attempt. Never copy session state across profiles.
- `/logout [account]`: if multiple accounts exist and none is specified, ask which one. POST to the selected profile's loopback `/api/logout` with `X-WhatsApp-Local-Action: logout`, verify the response, then stop/disable only that profile's autostart service. This unlinks that companion device and clears its WhatsApp session, but retains its message/contact databases and downloaded media. Never delete the profile directory. If the service is not running, report that WhatsApp unlink could not be verified; do not claim it logged out remotely.
- `/accounts`: list configured profile names, ports, service state, and local data locations. Do not expose secrets or message content.
- `/status [account|all]`: inspect service manager, listener, bridge status, and read-only API health for the requested profile(s). Ask which one when ambiguous and multiple profiles exist.
- `/refresh [account|all]`: check service and message freshness, start or restart only the selected stopped/unhealthy bridge using its existing profile configuration, wait for readiness, and verify `/api/chats`. Do not reset databases, re-pair, or replace the session to recover stale data. If recovery fails, report the layer and logs needed.
- `/sync [account]`: request/verify the local contact directory refresh for a connected profile; retain its data locally.
- `/viewer [account]`: start, stop, or verify the optional viewer for a selected profile. Avoid port collisions; bridge serves the bundled viewer when viewer mode is selected.

If a request does not name a profile and several accounts exist, ask which one before any profile-specific change. Never stop, log out, or restart every profile as a shortcut.

## Privacy and validation

Keep each account's WhatsApp session, SQLite stores, contacts, logs, and media local and separate. The skill kit contains only source and generic assets, never runtime data. Do not send messages unless the user explicitly asks. Do not push repository changes unless explicitly asked. Validate Go build, Python syntax if used, viewer JavaScript syntax if selected, `git diff --check` where applicable, and the selected local endpoints. Do not claim a phone-code login succeeded until the bridge reports authenticated.
