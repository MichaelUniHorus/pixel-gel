---
name: pixel-art
description: Create, animate, and export pixel art through the aseprite MCP server
argument-hint: "[what to draw / task]"
allowed-tools:
  - mcp__aseprite__*
  - read
  - exec
---

Create pixel art in Aseprite via the `aseprite` MCP server. Work on files inside this project; always pass absolute paths as `filename`.

## Workflow

1. **Plan first.** Decide canvas size (common: 16×16 icons, 24/32×32 items & small characters, 48/64 for characters, larger only when required), the palette, and the layer structure before touching tools.
2. **Create the canvas** with `create_canvas`. Organize with `add_group` / `add_layer` — e.g. `fx` over `char` over `shadow` over `bg`. Use `ensure_layers_present` to set up a standard structure in one call.
3. **Set the palette early.** Use `list_palette_presets` / `apply_palette_preset` or `set_palette`, and `generate_color_ramp` for shading ramps. Prefer hue-shifted ramps over pure value ramps.
4. **Draw** with `draw_pixels_at` / `draw_pixels` (coordinate lists), `draw_line_at`, `draw_rectangle_at`, `draw_circle_at`, `fill_area_at`, `draw_polygon`, `draw_path`. Draw in passes: silhouette/block-in → light/shadow → details → outline/cleanup. Use `get_pixels_rect` / `get_composite_rect` to inspect what is actually on the canvas.
5. **Animate** when needed: `add_frames`, `set_frame_duration` / `set_frame_duration_all`, `set_tag` for named clips (e.g. `idle`, `walk`, `attack`), `copy_frame`, `propagate_frame_to_range`, `create_cel`/`copy_cel`, and `tween_cel_positions*` / `tween_cel_opacity_eased` / `oscillate_cel_positions` for motion. Call `animation_workflow_guide` if unsure about the animation API.
6. **Verify** with `validate_scene`, `audit_animation`, `compare_frames`, `get_color_stats`, `render_onion_skin`. Fix stray pixels, banding artifacts, and broken frame transitions before exporting.
7. **Export** with `export_sprite` (PNG), `export_spritesheet` (sheet + JSON), `export_frame`, `export_layers`, `export_tag` (per animation clip). Deliver PNG/sheet outputs next to the `.aseprite` source.

## Craft rules

- Keep the palette small (≈4–16 colors per sprite). Quantize with `quantize_to_palette` when importing or cleaning up.
- Silhouette reads first: the shape must be recognizable at 100% zoom with no interior detail.
- Avoid single stray pixels and noisy clusters; every pixel should belong to a readable cluster or intentional texture.
- Prefer selective outlining over uniform black outlines; darker outline color than fill usually beats pure black.
- Dither sparingly (`apply_dither_gradient`, `apply_dither_pattern`) — large smooth ramp or ordered dither only where it serves the material.
- For animation: keep frame counts low (4–8 for loops), use anticipation → action → recovery timing, avoid sub-pixel jitter between frames, and tag clips so exports are addressable.
- Keep the `.aseprite` source file in the repo; exported PNGs/spritesheets go alongside it.

## After finishing

Report the file paths of the `.aseprite` source and exported assets, the palette used, and — for animations — the list of tags and frame counts.
