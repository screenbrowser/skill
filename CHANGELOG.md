# Changelog

## 1.1.1 — 2026-09-15

- Listing metadata for the plugin marketplace: `displayName` ("Screen Browser"), a `mcpServers`
  declaration in `plugin.json`, contact email on author/owner, and discovery keywords/tags led by
  video, audio and screen recording. Sharper descriptions and a tagline — "Screen recordings with
  AI voiceover, from your code." No behaviour change.

## 1.1.0 — 2026-09-09

- Mobile web: `create_project`, `update_project` and `start_run` take `device` (a current iPhone,
  Pixel, iPad or Galaxy Tab preset), `frame` (portrait, widescreen landscape, or none) and
  `backdrop` (neutral or brand). `get_run` reports the run's `output`. The skill says how to write
  guides for touch devices.

## 1.0.0 — 2026-09-05

- First release: the `screenbrowser` skill (write auth + main guides from source code, validate, run,
  fetch the video) and the MCP server configuration for Claude Code.
- Guide syntax reference and a complete example (Chatamatic, "Create your first keyword campaign").
