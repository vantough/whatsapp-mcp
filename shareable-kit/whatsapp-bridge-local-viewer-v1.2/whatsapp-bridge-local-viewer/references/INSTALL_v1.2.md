# WhatsApp Bridge and Local Viewer installation guide v1.2

This historical kit version includes the complete tracked project source under `assets/whatsapp-mcp-source/`, including the bridge, MCP server, lockfiles, and frontend. New setups do not require GitHub access or a source clone. First-time dependencies may be downloaded from Go's module proxy and the configured Python package index.

Choose backend only or backend plus viewer. If not specified, ask once. The example config records safe local defaults. Keep the WhatsApp session database, message/contact database, media, and logs local and private. Each user must pair their own WhatsApp account. QR login is the v1.2 supported path; use v1.3 for phone-code pairing and multiple isolated profile workflows.

The bridge port can be configured with `WHATSAPP_BRIDGE_PORT`; the Python MCP server can target a configured bridge using `WHATSAPP_API_BASE_URL`. Preserve existing files and never overwrite a local session database during repair.
