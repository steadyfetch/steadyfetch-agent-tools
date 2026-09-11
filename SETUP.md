# Setup

One step: an Apify API token.

## 1. Get a token

Sign in at [apify.com](https://apify.com), then open **Settings -> API & Integrations** in the Apify
Console and copy a Personal API token. The account's free tier is enough to try every actor here.

Put it in your environment as `APIFY_TOKEN`:

```bash
export APIFY_TOKEN="apify_api_..."
```

The manifests in this repo reference `${APIFY_TOKEN}` and never contain a token value.

## 2. Install for your client

### Claude Code

Install this repository as a plugin. It brings the MCP server and both skills:

```bash
claude plugin marketplace add steadyfetch/steadyfetch-agent-tools
claude plugin install steadyfetch-agent-tools@steadyfetch
```

The MCP server is defined in `.mcp.json` and reads `APIFY_TOKEN` from your environment. The two
skills in `skills/` come with it.

### Gemini CLI

```bash
gemini extensions install https://github.com/steadyfetch/steadyfetch-agent-tools
```

Gemini CLI prompts for the Apify API token on install and stores it for you; it is declared in
`gemini-extension.json` under `settings`.

### Skills only (any skills-aware client)

```bash
npx skills add steadyfetch/steadyfetch-agent-tools
```

### Cursor

Install from the Cursor marketplace, or drop this repository in `~/.cursor/plugins/local/` and
reload the window. Set `APIFY_TOKEN` under **Plugins -> Configure**.

### Any other MCP client

Add a streamable-HTTP server with the pinned URL from the [README](README.md#the-pinned-url) and the
header:

```
Authorization: Bearer <your Apify API token>
```

## 3. Check it works

Ask the agent to call `fetch-actor-details` for `steadyfetch/media-transcriber`. It returns the
actor's input schema and its charged events. Nothing is charged for reading an actor's details.
