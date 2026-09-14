---
name: screenbrowser
description: Produce a narrated tutorial video of a web app with Screen Browser. Use when asked to "make a tutorial video", "record a walkthrough", "create a product demo" or "document this feature as a video" for the app whose source code you are working in. Writes a plain-language guide from the codebase, hands it to the Screen Browser MCP server (create project, upload guides, start the run) and brings back the video.
---

# Screen Browser: tutorial videos from your source code

Screen Browser turns a written guide into a narrated video: it signs in to the customer's deployed
app in a real browser, follows the guide, adds the narration and the on-screen effects, and returns
an MP4. **You write the guide; Screen Browser writes the script.** A guide is plain language — what
the viewer does and what they see — not selectors, not stage directions. The recorder resolves each
step against the live page and remembers what it learned for the next run.

## Setup (once per machine)

The MCP server is part of the Screen Browser app and signs the user in with OAuth: installing this
plugin, or `claude mcp add --transport http screenbrowser https://mcp.screenbrowser.com`, is enough —
the first tool call opens the browser for sign-in and consent. If the agent cannot open a browser,
an API key from **Settings → API keys** goes on the same command as
`--header "Authorization: Bearer sb_live_…"`.

If the `screenbrowser` MCP tools are not available in the session, stop and ask the user to install
the plugin or register the server; if they are present but every call is refused, ask them to run
`/mcp` and authenticate.

## Workflow

1. **Check credits.** `get_credits`. A one-minute video costs about 10 credits; a run charges nothing
   unless it completes.
2. **Find or create the project.** `list_projects`; if the app is not there, gather from the user:
   the deployed URL to record (staging or production), a **demo user** that signs in with email and
   password only (no OAuth, no 2FA, no captcha, no new-device email), and which attestation applies
   (they own the app, a client authorised them, or it is an internal tool). Then `create_project` with
   a login gate — `login_success_url_contains` when the post-login redirect in the code goes to a
   distinct path such as `/dashboard`, otherwise `login_success_text` (words only a signed-in user
   sees) — and `allow_hosts` (every host the frontend calls: API subdomain, auth provider, CDN).
   Credentials go in `variables`, never in guides. Narration defaults to English in the language's default
   voice; when the user wants another language or a particular voice, `list_voices` (optionally with a
   `language`) shows the ids, tiers and credits per minute, and `create_project` / `update_project` take
   `language` and `voice`. A single video can use a different voice via `start_run`'s `voice`.
   **Mobile web:** the same guide can be recorded as a phone or tablet. `create_project` /
   `update_project` take a default `device` (`desktop`, `iphone`, `iphone-max`, `android`,
   `android-large`, `ipad`, `ipad-pro`, `android-tablet`), a `frame` (`portrait` 9:16 in a phone
   frame, `landscape` widescreen 16:9 with the phone upright, or `none` for the bare screen) and a
   `backdrop` (`neutral` or `brand`). One run is one device: for a desktop and a phone version,
   start two runs of the same guide and pass `device` to the second. Same credits per minute.
3. **Write the auth guide** from the login form in the code: go to the login page, type
   `{USERNAME}` into the field the user sees as the email or username field, `{PASSWORD}` into the
   password field, press the sign-in button, then `Confirm` something only a signed-in user sees.
4. **Write the main guide** for the feature, in the words a colleague would use over the shoulder:
   10–25 steps, one action per line, each naming the thing as it appears on screen — the button's
   text, the field's label, the menu item's words. Add a short line in quotes every 1–3 steps saying
   what the viewer is achieving; Screen Browser turns those into the narration and the effects.
   Put a `Confirm …` line after every navigation or save so a broken flow stops early. For a
   phone or tablet run, never write hover steps (a touch screen has no pointer; the recorder
   skips them) and name mobile navigation as the user sees it ("the menu button"); keep zoom
   factors at 1.4 or below, the screen is small. If a step
   creates data, name it so reruns do not collide, or delete it at the end of the guide.
5. **Validate.** `validate_guide` (kind `auth`, then `main`, with `project_id`) and fix every problem
   until `passed` is true. Show both guides to the user before uploading.
6. **Upload and run.** `put_guide` auth, `put_guide` main (give it a name), `start_run` (with
   `device`, `frame`, `backdrop` for a phone or tablet version), then `get_run`
   every 15–30 seconds until `is_terminal`. Runs take 2–6 minutes; do not start a second run of the
   same project while one is in progress. A failed run costs nothing and is the fastest way to learn
   what the page really calls things: `get_run` lists what was resolved and what was learned per step.
   If a run fails with "Login gate failed", the gate was wrong: fix it with `update_project` and start
   again.
7. **Deliver.** Give the user `video_url` (signed, valid 15 minutes; call `get_run` again for a fresh
   link) and the credits charged. `get_run`'s `artifacts` also carry the exports: `gif` (animated GIF of the
   video), `chapter_gifs` (zip, one per chapter), `subtitles_vtt` / `subtitles_srt`, and `chapters` (YouTube's
   chapter list) — hand over the ones the user asked for, or all of them for an upload elsewhere. On failure read `error.hints`, fix the guide, and retry once. Typical
   causes: the login gate never appeared, a host missing from `allow_hosts`, the demo user hit a
   captcha, a step that names something the page does not show.

## Writing guides that work on the first run

- **Name things the way a person sees them.** "Click Save", "Type the name into the Repository Name
  field", "Open Settings from the left menu". The recorder finds buttons by their text, fields by
  their label or placeholder, and learns the rest from the live page.
- **Never use class names a build tool generated** (`css-1x9f2k`, `sc-bdVaJa`, `_1a2b3c`): they change
  with every deploy and `validate_guide` refuses them. Ids and `name=` attributes from the templates
  are fine when a step needs precision; the syntax for that is in `references/guide-syntax.md`, but a
  good guide rarely needs it.
- **Use what you have.** With the source code, read the templates for the real labels and names.
  With a browser tool, open the dev server and look at the page. With neither, write plain prose and
  let the run learn.
- **Wait for the app, not the clock**: "Wait for the Saved message" beats "Wait 2 seconds".
- Anything hidden behind hover or below the fold needs a "Hover …" or "Scroll down" step first.
- **First-visit dialogs, cookie banners, "you are already signed in" interstitials:** write them as
  optional steps — `If visible, click "Remind Later"`, `If shown, click "Accept"`. The recorder
  skips an optional step whose element is not on the page instead of failing the run.
- Keep secrets out: never type a real password, token or card number literally.
- One feature per video. Split long flows into several guides on the same project.

## Effects (optional)

Screen Browser adds the narration, captions and highlights on its own from the quoted lines. When the
user asks for a specific effect, write it as a directive on the line before the step it decorates —
`[ZOOM "text=Save" 1.6]`, `[HIGHLIGHT "label=Email"]`, `[TOOLTIP "Pick a plan" on="text=Pricing"]`,
`[CHAPTER "Setting up billing"]`, `[CONFETTI 1500]`, `[BLUR "label=Card number"]`, `[PAUSE 0.8]`.
Point at elements the same human way (`text=`, `label=`, `role=button:Save`, `name=`), never at a
generated class name. The complete list is in `references/guide-syntax.md`. Effects depend on the plan:
every plan has captions, toasts, highlights, dim, annotations, chapters, step numbers, scroll, cursor and
blur; zoom, arrows, tooltips, headlines, fades, freeze, speed and confetti need Pro or above. An effect
outside the plan is skipped at recording time (the step still runs); `validate_guide` warns about it.

## Running out of credits

When `start_run` is refused with `insufficient_credits`, or `get_credits` shows the balance is short, do not
retry. Relay the two links from the response to the user verbatim: the one-click top-up link
(`top_up_url`, opens Stripe Checkout for the suggested pack, no login needed) and the billing page. `top_up`
(optional `pack`: small, medium, large) returns the same links on demand — a `/topup` request from the user
means call it and print the link. Once the user says they have paid, `get_credits` again and continue.

## When a run fails on our side

`get_run` marks such failures with `error.failure_report.available: true` (the page never loaded, the login
gate never appeared with correct credentials, a step timed out on an element that was there, a crash).
Then, and only then, **ask the user** whether to send Screen Browser the evidence — it shares their app's
screenshot, page structure and logs, never credentials — and call `report_run_failure` with the run id and
their note. Nothing is retried and the guide is not changed; Screen Browser replies on the run. If the
failure is in the guide (a button that does not exist, a wrong label), fix the guide instead.
