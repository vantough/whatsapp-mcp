---
name: whatsapp-bridge-local-viewer
description: Set up or maintain a local WhatsApp bridge from bundled source, optionally with a searchable message viewer and contact directory. Use for QR login, bridge startup, local viewer setup, or contact sync without requiring GitHub access.
---

# WhatsApp Bridge Local Viewer

Kit version: 1.2.

This skill is self-contained. It includes the tracked WhatsApp MCP project source, bridge, lockfiles, frontend, local contact-directory sync, and setup references. Do not clone, fetch, or require GitHub login. First inspect the current workspace and reuse a compatible existing project if present, preserving user changes. Otherwise install from `assets/whatsapp-mcp-source/` into the configured install directory.

When setup is invoked and the requested mode is unspecified, ask one focused question before changing files: “Backend only (WhatsApp bridge and QR login) or backend plus the optional local message viewer?” Honor a mode already provided in the request or config and do not ask again. Use `references/setup-config_v1.2.example.json` as defaults, not as a questionnaire. Its install path, bridge port, autostart, and optional MCP server switch are configurable. Defaults: use a safe local install directory, port 8080, user-level autostart on login, and do not configure the MCP client unless explicitly enabled.

After that mode choice, complete setup in one run: preserve any existing files, prepare the selected components from bundled source, install required Go dependencies through `proxy.golang.org` and Python dependencies through the configured package index only when needed, build, start, and verify. Never contact GitHub or require a GitHub account. Ask for input only for required owner actions such as scanning the WhatsApp QR code or granting macOS file access. Resume the remaining setup after those actions. Do not claim success until the connection, selected HTTP routes, and autostart configuration have each been checked.

The user must authenticate their own WhatsApp account. Never copy or share another user's session database, message database, QR code, media cache, logs, contact export, or launch-agent plist containing personal paths.

Read [the install and privacy guide](references/INSTALL_v1.2.md) before applying changes. The bundled complete project is in `assets/whatsapp-mcp-source/`. The viewer is optional and uses `assets/frontend/` as the standalone frontend asset set. Use `references/bridge-enhancements.patch` only to update a compatible existing checkout at its stated base revision. Never force-apply over local changes.

Keep contact names and identity mappings in the local SQLite database only. Prefer the user's saved address-book name over WhatsApp profile or business names, and label automatic names distinctly in the UI. Keep phone JIDs and LIDs linked as identities so lookup by either resolves to the same contact. Refresh the directory after connect and periodically, and make refresh overlap-safe. The bridge port is configurable with `WHATSAPP_BRIDGE_PORT`; the optional Python MCP server endpoint is configurable with `WHATSAPP_API_BASE_URL`.

Validate a Go build, JavaScript syntax, `git diff --check`, the viewer route, and a read-only `/api/contacts` query. Do not send messages or pair an account during setup without the account owner's direct request. Do not push changes to a shared repository unless explicitly asked.
