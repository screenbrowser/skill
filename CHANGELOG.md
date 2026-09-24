# Changelog

## 1.3.0 — 2026-09-24

- Openings and endings: the skill gives each main guide a `video_title` for the branded opening,
  skips `[HEADLINE]` when the project already opens with the title, and knows the new
  `update_project` fields for a project's own opening and ending (style, 3–10 s length, call to
  action, fade, narrated title). They are free and uploads stay in the app.

## 1.2.0 — 2026-09-20

- The skill now says which effect fits which moment (a ring before a click, a zoom aimed at the
  card rather than its heading, a tooltip on the side that covers nothing, an annotation for what
  the video cannot show, one chapter card per section), asks for a caption before every action the
  viewer should hear about, and has the agent review the video at the moment of every effect
  before handing it over. The free check runs after every edit. Written from one real Claude Code
  session and its twelve takes of the same guide.

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
