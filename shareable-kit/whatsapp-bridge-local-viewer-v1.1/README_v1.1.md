# WhatsApp Bridge Local Viewer Skill Kit v1.1

A shareable Codex skill and frontend add-on for colleagues who want a local browser view of their own WhatsApp bridge, including searchable contacts and linked WhatsApp identities. Version 1.1 makes setup a one-prompt end-to-end workflow, pausing only for required login or OS permission actions.

## Package contents

- `whatsapp-bridge-local-viewer/SKILL.md`: reusable setup and maintenance skill.
- `whatsapp-bridge-local-viewer/assets/frontend/`: the static viewer resources.
- `whatsapp-bridge-local-viewer/references/bridge-enhancements.patch`: bridge-side API, local contact-directory sync, and the compatible viewer features from this checkout.
- `whatsapp-bridge-local-viewer/references/INSTALL_v1.1.md`: compatibility, one-prompt setup, and privacy notes.

## Install the skill

Import the `whatsapp-bridge-local-viewer/` folder into Codex's skills directory. Keep the folder name unchanged. The README and patch are supporting kit materials, not required for invoking the skill after installation.

## Apply to a colleague's own bridge

Invoke the installed skill with a single request such as “Set up the local WhatsApp bridge viewer in this repo.” It will inspect the checkout, apply compatible source assets, build and verify, start the local service, and configure user-level startup on login. The colleague must scan their own WhatsApp QR code if asked and grant any required OS file permissions. Each colleague authenticates their own account and keeps databases and media local. This archive intentionally excludes runtime state and generated logs.

## Compatibility

The bridge patch was generated against repository revision `06624d6ba750d4f41eda2a7d988c4a008c9ac014` of `vantough/whatsapp-mcp`, with the Go dependency versions in that checkout. Use `git apply --check` first. Do not apply it blindly to a different revision or overwrite local changes.

The package contains source code only. It does not contain a WhatsApp login/session, contact records, messages, media, credentials, or a personalized LaunchAgent.
