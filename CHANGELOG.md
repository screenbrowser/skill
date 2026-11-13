# Changelog

## 1.3.18 — 2026-11-13

- Flutter web apps: how to name checkboxes, switches and icon buttons, how to set a slider, and what the sign-in needs.

## 1.3.17 — 2026-11-10

- Fixed the skill's description so every tool can read it.

## 1.3.16 — 2026-11-10

- Works from the app's address alone: no source code needed. Inside a codebase, the skill still reads the code for the real labels.
- Tutorial or promo: a promo is a short, upbeat video of the best parts.
- Wider triggers: demo, walkthrough, promo, onboarding, launch and pitch-deck videos.
- New plugin description and keywords.

## 1.3.15 — 2026-11-08

- Apps that only run on your computer: the skill opens an ngrok or Cloudflare Tunnel address and records that. ngrok's free-plan warning page is skipped, with no extra guide step.

## 1.3.14 — 2026-11-04

- Free videos carry a small Screen Browser logo; the skill says so when it points to the free sign-up.

## 1.3.13 — 2026-11-03

- Shorter listing descriptions: your coding agent records demo and tutorial videos of your web app.

## 1.3.12 — 2026-11-03

- New accounts: the first 100 get 10 free minutes of video with no card; the skill points users without an account to the free sign-up, and to the $3.99 trial once it is full.

## 1.3.11 — 2026-10-31

- Most runs take 2–6 minutes and a long guide up to 15, so keep polling until `is_terminal`.
- `get_run` warns when a page or video inside the app could not load during the recording.

## 1.3.10 — 2026-10-30

- The MCP server has a `get_guide_instructions` tool with the same writing rules as this skill, so an agent without the skill writes guides the same way.
- `put_guide` with a new `name` starts another video on the project instead of replacing the newest guide; `guide_id` saves a new version of a specific guide. New `get_guide` tool reads a saved guide.
- `get_run` warns when a guide line named something that was not on the page and the recorder used a close match.
- `validate_guide` gives a corrected example for each problem.
- A second example guide, in plain language without selectors; the first is now a guide from a real first-run test.
- `create_project` and `list_projects` return `project_url`; `start_run` refuses a run whose guides use a variable with no value, instead of failing at sign-in.
- `get_credits` also gives the rate of the default Studio voice.
- `get_run` lists the video and its exports only.

## 1.3.9 — 2026-10-23

- `validate_guide` is described as what it is: an instant check of the guide's text, without opening the app.
- A failed sign-in is named by its real error code, `login_failed` ("Sign-in didn't go through").

## 1.3.8 — 2026-10-06

- Setup says what connecting needs (a plan and a confirmed email address) and points new users to the 7-day trial.

## 1.3.7 — 2026-10-01

- Credential wording: values are stored encrypted and never returned by any tool; values given to the agent pass through its conversation, so set them in the dashboard to keep them out of chat.

## 1.3.6 — 2026-09-27

- Zoom is included on every plan; the skill's plan note lists it with the everyday effects.

## 1.3.5 — 2026-09-27

- Manifest names the terms of service, the support page and the documentation page for directory listings.

## 1.3.4 — 2026-09-27

- README: the API-key example shows a placeholder instead of an environment variable.

## 1.3.3 — 2026-09-27

- Plugin manifest carries an icon, the privacy policy and the docs page; the README says exactly what the
  plugin sends to Screen Browser and what never leaves your machine.

## 1.3.2 — 2026-09-26

- The repository is an [Agent Plugins](https://agent-plugins.org/) package as well: `plugin.json` and
  `mcp.json` at the root, so Cursor, VS Code, GitHub Copilot and other clients of the standard install
  it from the repository URL. Nothing changes for Claude Code.
- The MCP server's tools now carry titles, annotations, a description on every parameter and an
  output schema; results also arrive as structured content.

## 1.3.1 — 2026-09-24

- `update_project` takes `title_line`, the words the narrator says over the opening (`{title}`
  fills in the video title). The narrated opening is off by default.

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
