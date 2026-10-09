# WhatsApp Bridge Setup Skill Kit v1.2

A self-contained Codex skill kit for setting up the WhatsApp bridge from bundled source, with the local message viewer as an optional mode. Version 1.2 includes the bridge, MCP server source, viewer, contact directory, and configuration example. It does not require a GitHub account, GitHub login, or repository clone.

## Package contents

- `whatsapp-bridge-local-viewer/SKILL.md`: reusable setup and maintenance skill.
- `whatsapp-bridge-local-viewer/assets/frontend/`: the static viewer resources.
- `whatsapp-bridge-local-viewer/assets/whatsapp-mcp-source/`: complete tracked project source needed for setup, bundled locally.
- `whatsapp-bridge-local-viewer/references/bridge-enhancements.patch`: incremental update for an existing install at the source revision identified in the guide.
- `whatsapp-bridge-local-viewer/references/INSTALL_v1.2.md`: setup modes, configuration, offline-source and privacy notes.
- `whatsapp-bridge-local-viewer/references/setup-config_v1.2.example.json`: editable defaults for install path, mode, port, autostart, and optional MCP integration.

## Install the skill

Import the `whatsapp-bridge-local-viewer/` folder into Codex's skills directory. Keep the folder name unchanged. The skill's source and frontend assets travel with it, so the user does not need to clone a repository or authenticate to GitHub.

## Apply to a colleague's own bridge

Invoke the installed skill with one request. If the mode is not specified, it asks once whether to install the backend only or the backend plus message viewer. It then uses the bundled source, builds, verifies, configures user-level startup, and runs the selected components. Each person scans their own WhatsApp QR code and keeps databases and media local. First-time dependency installation may use Go's module proxy and Python's package index, but no GitHub account, login, or repository access is needed.

## Compatibility

The complete bundled project source corresponds to `vantough/whatsapp-mcp` at revision `b309e7581df2e7af4d5a763048cad3e74b41d487`, plus configurable port handling in v1.2. The incremental patch is for existing installations based on that revision. Check compatibility before applying it and preserve local changes.

The package contains source code and lockfiles only. It does not contain a WhatsApp login/session, contact records, messages, media, credentials, virtual environments, or a personalized LaunchAgent.
