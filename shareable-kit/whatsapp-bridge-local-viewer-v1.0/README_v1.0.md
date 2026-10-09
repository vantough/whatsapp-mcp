# WhatsApp Bridge Local Viewer Skill Kit v1.0

A shareable Codex skill and frontend add-on for colleagues who want a local browser view of their own WhatsApp bridge, including searchable contacts and linked WhatsApp identities.

## Package contents

- `whatsapp-bridge-local-viewer/SKILL.md`: reusable setup and maintenance skill.
- `whatsapp-bridge-local-viewer/assets/frontend/`: the static viewer resources.
- `whatsapp-bridge-local-viewer/references/bridge-enhancements.patch`: bridge-side API, local contact-directory sync, and the compatible viewer features from this checkout.
- `whatsapp-bridge-local-viewer/references/INSTALL_v1.0.md`: compatibility, installation, and privacy notes.

## Install the skill

Import the `whatsapp-bridge-local-viewer/` folder into Codex's skills directory. Keep the folder name unchanged. The README and patch are supporting kit materials, not required for invoking the skill after installation.

## Apply to a colleague's own bridge

Follow `whatsapp-bridge-local-viewer/references/INSTALL_v1.0.md`. Each colleague must clone the source, authenticate their own WhatsApp account, and keep their databases and media local. This archive intentionally excludes all runtime state and generated logs.

## Compatibility

The bridge patch was generated against repository revision `06624d6ba750d4f41eda2a7d988c4a008c9ac014` of `vantough/whatsapp-mcp`, with the Go dependency versions in that checkout. Use `git apply --check` first. Do not apply it blindly to a different revision or overwrite local changes.

The package contains source code only. It does not contain a WhatsApp login/session, contact records, messages, media, credentials, or a personalized LaunchAgent.
