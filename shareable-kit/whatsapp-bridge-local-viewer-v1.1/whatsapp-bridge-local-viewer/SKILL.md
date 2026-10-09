---
name: whatsapp-bridge-local-viewer
description: Set up or maintain a local WhatsApp bridge message viewer with a searchable contact and identity directory. Use when adapting this bundled frontend and directory sync to a colleague's own WhatsApp bridge.
---

# WhatsApp Bridge Local Viewer

Kit version: 1.1.

Use this kit to add a local-only browser viewer and a SQLite-backed contact directory to a compatible `whatsapp-mcp` Go bridge. When invoked for setup, drive the complete compatible setup from that single request without a multi-step questionnaire: discover the repo in the current workspace, inspect branch and working-tree changes, verify compatibility, apply the kit without replacing user edits, build, start, configure user-level startup on login, and verify the viewer and read-only directory API. If the repo is absent or the patch does not apply cleanly, stop before destructive changes and explain the exact blocker. Ask for input only for a required choice or a manual account-owner step such as scanning the WhatsApp QR code or granting macOS file access, then resume the remaining setup after the user completes it. Do not claim success until the live endpoints, connection state, and startup configuration are checked separately.

The user must authenticate their own WhatsApp account. Never copy or share another user's session database, message database, QR code, media cache, logs, contact export, or launch-agent plist containing personal paths.

Read [the install and privacy guide](references/INSTALL_v1.1.md) before applying changes. The bundled `assets/frontend/` files are the viewer. `references/bridge-enhancements.patch` contains the bridge changes against the source revision identified in the guide. Check compatibility and inspect `git diff` before applying. If the source has diverged, adapt the schema, local contact-store API, LID-to-phone mapping, and `/api/contacts` endpoint to the installed whatsmeow version rather than forcing the patch.

Keep contact names and identity mappings in the local SQLite database only. Prefer the user's saved address-book name over WhatsApp profile or business names, and label automatic names distinctly in the UI. Keep phone JIDs and LIDs linked as identities so lookup by either resolves to the same contact. Refresh the directory after connect and periodically, and make refresh overlap-safe.

Validate a Go build, JavaScript syntax, `git diff --check`, the viewer route, and a read-only `/api/contacts` query. Do not send messages or pair an account during setup without the account owner's direct request. Do not push changes to a shared repository unless explicitly asked.
