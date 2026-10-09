# Local WhatsApp MCP installation and privacy guide v1.3

## Choose a mode

- Backend only: the WhatsApp bridge, login, local sync, and optional MCP server integration.
- Backend plus viewer: the same bridge with the bundled local message viewer.

The skill asks which mode only when neither the request nor the config already specifies it. Viewer assets are optional; bridge and login do not depend on running the viewer.

## Source and dependencies

Use `assets/whatsapp-mcp-source/` as the source of truth for new installations. It includes the upstream tracked project, Go module files, Python MCP server, and frontend. No GitHub account, login, `git clone`, or repository access is required. Building may need external dependency access through `proxy.golang.org` and the selected Python package index. Do not fetch source from GitHub as a setup prerequisite.

For an existing checkout, inspect its revision and local diff first. The included patch is based on `b309e7581df2e7af4d5a763048cad3e74b41d487`; do not force it onto a different or dirty checkout. Prefer preserving local modifications and adapt only after reviewing compatibility.

## Configuration and account profiles

Start with `setup-config_v1.3.example.json`. Change install paths, bridge port, viewer mode, autostart, MCP integration, and profile storage as needed. A profile is a named isolated instance with a unique port, session SQLite store, message/contact SQLite store, downloaded-media directory, and background service identity. Multiple profiles can use the same installed source/binary. Never copy a session database between profiles. Never reuse a port. Keep profile names and paths in local config; do not package machine-specific config in the shared skill.

On macOS, use one user LaunchAgent per profile with an explicit working directory and environment variables, including `WHATSAPP_BRIDGE_PORT`, `WHATSAPP_BRIDGE_DATA_DIR`, and, in viewer mode, `WHATSAPP_FRONTEND_DIR`. Set `WHATSAPP_BRIDGE_BIND_ADDRESS=127.0.0.1` (the default) unless the owner intentionally configures a protected network interface. Use user-level startup only. Other operating systems should use their user-level service manager and preserve the same per-profile isolation. Do not elevate privileges just to install a personal bridge.

If the optional Python MCP server is configured, set `WHATSAPP_API_BASE_URL` to the selected profile's loopback API and `WHATSAPP_MESSAGES_DB_PATH` to that same profile's `messages.db`. This keeps MCP queries aligned with the intended account.

## Login methods

QR is the default. Show the QR image or terminal code only to the account owner and avoid recording it in shared logs or documents.

Phone pairing is an alternative supported by the bundled whatsmeow version. It is a WhatsApp linked-device pairing code, not a carrier SMS OTP. Use the phone number in international format. The owner opens WhatsApp on that number, chooses Linked devices, Link a device, then Link with phone number instead, and enters the code printed by the selected profile. Pairing codes expire quickly, so request one only when the owner is ready. The phone number is supplied only to the bridge process for that pairing attempt. Do not expose the code remotely or persist it in the profile registry. On success the bridge stores a session for that specific profile.

## Local routes and operational checks

- `GET /api/chats`: list locally cached chats and latest message metadata.
- `GET /api/messages?chat_jid=...`: read messages for a chat.
- `GET /api/contacts?q=...`: search the locally synchronized directory.
- `POST /api/logout` with `X-WhatsApp-Local-Action: logout`: unlink the selected profile's companion device. Use only on loopback, only after selecting the account. This clears WhatsApp session credentials in the bridge's device store but retains messages, contacts, and media. Verify the response before stopping/disabling that profile's service.

For `/refresh`, check the selected service state, listener port, and `/api/chats` freshness. Restart only the affected profile if stopped or unhealthy. Do not delete SQLite files, clear the data directory, or re-pair as a generic recovery action. If a remote logout cannot be verified, report the limitation instead of implying it succeeded.

## Privacy

The bridge receives personal messages, identifiers, contact names, and media. Keep the API bound to loopback or otherwise appropriately protected; do not expose it to a public network. Keep data directories private to the OS account. Do not upload runtime files, service logs, QR images, profile config, or WhatsApp databases. The shared kit includes source and generic assets only.
