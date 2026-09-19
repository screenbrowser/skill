# Screen Browser guide syntax

A project has two guides. Both are plain text, one step per line, written the way a colleague would
explain the flow over your shoulder. Screen Browser turns each line into a browser action, writes the
narration and the effects from the quoted sentences, and resolves each step against the live page — the
precise forms below are there when a step needs them, not the way guides are normally written.

- **Auth guide** — the login flow only. Runs first, is never recorded, and may use `{USERNAME}` /
  `{PASSWORD}` placeholders (values come from the project's encrypted variables and are substituted
  only inside the browser, never sent to a model).
- **Main guide** — the walkthrough that becomes the video. Runs after login, recorded, narrated.

Recording starts only after the **login gate** passes. The gate is one or more of: the address bar
contains `login_success_url_contains` (e.g. `/dashboard`), a CSS `login_success_selector` is visible
(e.g. `nav a[href="/campaigns"]`), or `login_success_text` is on the page (e.g. `Sign out`). Read the
post-login redirect in the code: if the app lands on `/` after signing in, a URL fragment cannot work,
so use a selector or text that exists only for signed-in users.

## Step lines (actions)

| Write | Does |
|---|---|
| `Go to https://app.example.com/login` | open a URL (the first step of every guide) |
| `Click selector button[type="submit"]` | click by CSS selector (most reliable) |
| `Click "New campaign"` | click the element with exactly this visible text |
| `Type {USERNAME} into selector input[name="email"]` | type a variable (auth guide) |
| `Type "Spring promo" into selector #name` | type a literal |
| `Press Enter` | press a key (Enter, Tab, Escape, …) |
| `Select "Monthly" in selector select#interval` | choose an option in a `<select>` |
| `Hover selector .menu-trigger` | reveal hover menus / tooltips |
| `Scroll down 400 pixels` | scroll the page |
| `Wait for selector .toast-success` | wait until an element is visible (preferred over fixed waits) |
| `Wait 1 second` | fixed wait (use sparingly) |
| `Confirm text "Campaign created" is visible` | assert visible text; fails the run early if missing |
| `Confirm the URL contains "/campaigns"` | assert the address bar |

Naming the element the way a person sees it (preferred — these survive rebuilds and work on apps
you cannot inspect):

| Write | Resolves by |
|---|---|
| `Click "Save"` | exact visible text |
| `Click the button named "Save"` | accessible role + name |
| `Type {USERNAME} into the field labelled "Email address"` | the input's label / aria-label |
| `Type "Acme" into the field with placeholder "Company"` | placeholder |
| `Type {PASSWORD} into the input named "password"` | the `name` attribute |
| `Click the element with test id "save-button"` | `data-testid` |
| `Click selector #wp-submit` | CSS, only for ids, `name=` and word-like classes |

Rules:
- One action per line. Prefer the human forms above; use `selector` for ids, `name=` attributes and
  `data-testid`. **Never** use class names a build tool generated (`.css-1x9f2k`, `.sc-bdVaJa`,
  `_1a2b3c`): they differ between the dev server and production and change on every deploy — they
  are rejected.
- After every navigation or form submit, add a `Confirm …` line so a broken flow stops early.
- Never type a real password or token literally; always `{VARIABLE}`.
- Start every guide with `Go to https://…`.

## Directive lines (effects and narration)

Directives are `[NAME …]` tokens. Put them on their own line before the step they decorate, or
several on one line. `[CAPTION "…"]` text is also what the narrator says for that moment; keep it to
one short sentence. A `[PAUSE n]` after a caption gives the viewer time to read.

```
[CAPTION "text"]                      on-screen caption + narration
[TOAST "text"]                        small notification bubble
[HIGHLIGHT "css=#save-btn"]           ring around an element (also "text=Save", "label=Email", "role=button:Save", "name=email", "placeholder=…", "testid=…", "id=…")
[HIGHLIGHT "text=Billing"]
[DIM 0.35]                            darken everything except highlights (0–1)
[ZOOM "css=#panel" 1.6]               zoom towards an element (factor)
[ZOOM_OUT]
[TOOLTIP "text" on="css=#el" placement=bottom]   placement: top|bottom|left|right
[ANNOTATION "css=#el|Label text"]     label pinned to an element
[ARROW from="css=#a" to="css=#b"]
[BLUR "css=#secret-field"]            blur sensitive content
[CURSOR "css=#el"]                    move the cursor to an element
[SCROLL_INTO_VIEW "css=#el"]
[STEP_NUMBER 2 of 6]                  step badge
[CHAPTER "Title" duration_ms=1800]    chapter card
[HEADLINE "Title" duration_ms=1800 size=lg]
[THEME info]                          overlay colour theme; [THEME] resets
[CONFETTI 1500]
[SPEED 2.0]                           playback speed until [SPEED 1.0]
[FREEZE 1.0]                          hold the frame (seconds)
[FADE_OUT 600] / [FADE_IN 600]        milliseconds
[PAUSE 0.8]                           pause narration/pacing (seconds)
[WAIT 2]                              wait (seconds) — prefer "Wait for selector"
[CLEAR_OVERLAYS]                      remove every overlay
```

## Length and pacing

Videos are capped at 5 minutes in beta. A good tutorial is 10–25 action steps with a caption every
1–3 steps: roughly 60–120 seconds. Open with a `[CAPTION]` that says what the viewer will achieve,
close with a `[TOAST]` or `[CAPTION]` that confirms the result, then `[CLEAR_OVERLAYS]`.

## Prerequisites on the customer's side

- A **demo user** that signs in with email + password: no OAuth, no 2FA, no captcha, no "new device"
  emails. Seed it with realistic data so the walkthrough has something to show.
- Every host the app talks to (API subdomain, CDN, auth provider) listed in the project's
  `allow_hosts`; the recording browser can reach only those.
- The flow must be repeatable: if a step creates something (a campaign named "Spring promo"), either
  delete it afterwards in the guide or make the name unique per run.


## Optional steps

A line that starts with `If visible,` / `If shown,` / `If it appears,` / `If there is` / `If present` is
optional: `If visible, click "Remind Later"`. When the element is not on the page within two seconds the
step is skipped and the run continues; the run activity says so. Use it for first-visit dialogs, cookie
banners and interstitials that may or may not appear.
