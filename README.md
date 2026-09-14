# Screen Browser skill

Turn a feature of your web app into a narrated tutorial video, from inside your coding agent.

[Screen Browser](https://screenbrowser.com) records a real browser following a written guide against
your deployed app, adds a narrator and on-screen effects, and returns an MP4. This repository gives
your AI coding agent two things:

- **The `screenbrowser` skill** — how to read your codebase and write the two guides Screen Browser
  needs (the login flow and the walkthrough), how to validate them, and how to run the recording.
- **The MCP server configuration** — the tools the agent calls: create the project, upload guides,
  start the run, poll it, fetch the video. The server itself is part of your Screen Browser
  account at `https://mcp.screenbrowser.com`.

## Install (Claude Code)

Add this repository as a plugin marketplace and install the plugin:

```bash
claude plugin marketplace add screenbrowser/skill
claude plugin install screenbrowser@screenbrowser
```

The first time the agent reaches for a Screen Browser tool, Claude Code opens your browser: sign in
to Screen Browser, approve the connection, and you are done. Nothing to copy. (If it does not
prompt, run `/mcp` and pick **screenbrowser → Authenticate**.)

Without the plugin system, register the server yourself and copy the skill:

```bash
claude mcp add --transport http screenbrowser https://mcp.screenbrowser.com
cp -r skills/screenbrowser ~/.claude/skills/
```

### With an API key instead

If your agent cannot open a browser to sign in, create a key under **Settings → API keys** in
Screen Browser and pass it as a bearer token:

```bash
claude mcp add --transport http screenbrowser https://mcp.screenbrowser.com \
  --header "Authorization: Bearer $SCREENBROWSER_API_KEY"
```

Any MCP client works the same way: Streamable HTTP at `https://mcp.screenbrowser.com`, OAuth 2.1 with
dynamic client registration for interactive sign-in, or `Authorization: Bearer <key>`.

### Other agents

The skill is a plain `SKILL.md` in the open Agent Skills format, and the server is standard MCP over
HTTP, so the same two pieces work outside Claude Code.

**Cursor** — add the server to `.cursor/mcp.json` (or the global one in Cursor settings), click
**Sign in** next to it when Cursor offers, and copy the skill into `.cursor/skills/`:

```json
{
  "mcpServers": {
    "screenbrowser": { "url": "https://mcp.screenbrowser.com" }
  }
}
```

```bash
mkdir -p .cursor/skills && cp -r skills/screenbrowser .cursor/skills/
```

**OpenAI Codex CLI** — add the server to `~/.codex/config.toml`, sign in once, and copy the skill
into Codex's skills folder:

```toml
[mcp_servers.screenbrowser]
url = "https://mcp.screenbrowser.com"
```

```bash
codex mcp login screenbrowser
mkdir -p ~/.codex/skills && cp -r skills/screenbrowser ~/.codex/skills/
```

**Anything else (Gemini CLI, Windsurf, a custom agent)** — register an HTTP MCP server with the URL
the way that tool documents it; a client that supports OAuth will sign you in, one that does not
takes the API key as an `Authorization: Bearer` header. Give the agent `skills/screenbrowser/SKILL.md`
as instructions (paste it, or point the tool's rules file at it). The MCP server itself also carries
the guide syntax as a resource and a `write-tutorial-guide` prompt, so a client with MCP prompts can
work without the skill file at all.

## Use

Videos can be recorded on the desktop or as a phone or tablet (`device: iphone`, `android`, `ipad`,
…), in a phone frame on a vertical 9:16 or widescreen 16:9 canvas or as the bare screen; one run is
one device.

Open your app's repository in Claude Code and ask:

> Make a tutorial video of creating a campaign.

The agent will check credits, create the project (it will ask you for the deployed URL, a demo
user, and the legal attestation), write the guides from your routes and templates, validate them,
show them to you, start the run, and give you the video link a few minutes later.

The MCP server also ships a `write-tutorial-guide` prompt that walks any client through the same
flow, and two resources: `screenbrowser://guide-syntax` and `screenbrowser://example-guides`.

## What the agent needs from you

- A **demo user** that signs in with email and password: no OAuth, no 2FA, no captcha. Seed it with
  realistic data.
- The deployed URL to record (staging is fine) and every host the app talks to (API subdomain,
  auth provider, CDN).
- Confirmation that you are allowed to record the app (yours, a client's with permission, or an
  internal tool).

## Layout

```
.claude-plugin/plugin.json         plugin manifest
.claude-plugin/marketplace.json    lets this repo act as its own marketplace
.mcp.json                          MCP server config (the URL; sign-in is OAuth)
skills/screenbrowser/SKILL.md      the skill
skills/screenbrowser/references/   guide syntax reference, worked example
examples/chatamatic/               a real auth guide + main guide pair
```

## What a guide looks like

Plain language, one step per line, naming things as they appear on screen. Screen Browser turns it
into the script — the narration, the highlights, the pacing — and resolves each step against the
live page:

```
Go to https://app.acme.test/campaigns
"Let's create a spring promotion."
Click "New campaign"
Type "Spring promo" into the Name field
Click "Save"
Confirm text "Campaign created" is visible
```

Ids and `name=` attributes are allowed when a step needs precision; class names generated by a build
tool are refused. The full reference, including the optional effect directives, is in
[`skills/screenbrowser/references/guide-syntax.md`](skills/screenbrowser/references/guide-syntax.md).

## Support

Issues and ideas about the skill are welcome in this repository. Anything about your account or a
particular recording: hello@screenbrowser.com.

## License

MIT. Screen Browser itself is a hosted service; this repository contains only the agent-facing
skill, configuration and examples.
