# pixel-gel

Pixel art project powered by [Aseprite](https://www.aseprite.org/) and the `aseprite` MCP server in Devin.

## Setup

- Aseprite must be installed (default: `C:\Program Files\Aseprite\Aseprite.exe`). Override with the `ASEPRITE_PATH` env var.
- The MCP server is `aseprite_mcp` (Python module). It is declared in `.devin/mcp_config.json` for this project.
- A `pixel-art` skill (`.devin/skills/pixel-art/`) describes the agent workflow: canvas → palette → layers → draw → animate → verify → export.

## Conventions

- `.aseprite` source files are committed.
- Exported PNGs and spritesheets live next to their sources.
