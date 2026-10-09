# Local WhatsApp MCP Skill Kit v1.3

A self-contained Codex skill for installing and managing the WhatsApp MCP bridge from bundled source. It supports named, isolated account profiles, QR or WhatsApp linked-device phone-code login, local contact sync, and an optional WhatsApp-inspired message viewer. It requires no GitHub login, repository clone, or GitHub access.

## Contents

- `local-whatsapp-mcp/SKILL.md`: skill instructions and command-like workflows.
- `local-whatsapp-mcp/agents/openai.yaml`: Codex display name and setup prompt.
- `local-whatsapp-mcp/assets/whatsapp-mcp-source/`: bundled complete project source.
- `local-whatsapp-mcp/assets/frontend/`: standalone viewer assets.
- `local-whatsapp-mcp/references/INSTALL_v1.3.md`: modes, configuration, privacy, and platform notes.
- `local-whatsapp-mcp/references/setup-config_v1.3.example.json`: configurable defaults and account profile layout.

## Install and invoke

Copy the `local-whatsapp-mcp/` skill folder into the Codex skills directory. Invoke it with one request, such as “Set up my local WhatsApp MCP”. If the request does not say whether the viewer is wanted, it asks whether to set up backend only or backend plus viewer. Login can use the default QR flow or WhatsApp's linked-device phone pairing code. The latter is not an SMS OTP.

## Commands

`/setup`, `/login`, `/logout`, `/accounts`, `/status`, `/refresh`, `/sync`, `/viewer`, and `/help` are documented command-like workflows in the skill. Account-specific actions select a named profile and do not affect other accounts. Logout unlinks that device while retaining local message and contact history.

## Dependencies and privacy

The full project source and lockfiles are packaged, so no source clone is needed. First-time Go and Python dependencies may require access to Go's module proxy and the configured Python package index. Each user's WhatsApp session, databases, media, contacts, and logs remain local and are excluded from the kit. See the install guide before setting up.

## Version

The bundled project includes the source revision used for this feature branch plus local configuration improvements for separate ports and data directories, per-profile logout, and phone-code pairing. New setups use the included complete source. For an existing project, inspect and adapt the code carefully instead of applying a blind patch.
