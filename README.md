# JSON2Video MCP Server

<img src="assets/logo.svg" alt="JSON2Video" width="72" align="right">

Official [Model Context Protocol](https://modelcontextprotocol.io/) server for **[JSON2Video](https://json2video.com/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mcp-listing)**, the video API for automated video production at scale.

Connect your AI agent (Claude, Cursor, VS Code, Windsurf, Cline…) to JSON2Video and let it produce finished MP4 videos: build a video from reusable templates, add AI voiceovers, subtitles, AI-generated images and video, render it in the cloud and get back a URL you can publish.

**Built and maintained by the JSON2Video team.** Two ways to use it: connect to the hosted **remote server** by URL (no install, sign in with your JSON2Video account), or run the **local server** from the [`@json2video/cli`](https://www.npmjs.com/package/@json2video/cli) npm package over stdio.

Listed in the official MCP Registry as `com.json2video/mcp`.

## What you can do

- "Make a 30-second vertical video about our new product with a voiceover and subtitles."
- "Render my *Real estate listing* template for each of these 10 properties."
- "Create a template for daily news shorts, then render today's three stories."
- "Upload this logo and add it as a watermark to the video."
- "How many credits do I have left?"

## Connect by URL (remote server)

The easiest way. Add this URL to your AI app as a custom MCP server:

```text
https://mcp.json2video.com/mcp
```

The first time, the app sends you to JSON2Video to sign in and approve the connection ("<App> wants to connect to your JSON2Video account", **Allow** / **Cancel**). No API key to copy, nothing to install. Renders use the credits of the account you sign in with.

- **Claude (claude.ai and Claude Desktop):** **Settings → Connectors → Add custom connector**, paste the URL, then **Connect**. On Team and Enterprise plans an organization owner adds it for the organization first.
- **ChatGPT:** **Settings → Connectors → Add → Add custom MCP server**, paste the URL and choose **OAuth**. Availability depends on your ChatGPT plan and workspace.
- **Claude Code:**

  ```bash
  claude mcp add --transport http json2video https://mcp.json2video.com/mcp
  ```

  then run `/mcp` and authenticate.
- **Cursor** (`~/.cursor/mcp.json`):

  ```json
  {
    "mcpServers": {
      "json2video": { "url": "https://mcp.json2video.com/mcp" }
    }
  }
  ```
- **VS Code** (`.vscode/mcp.json`):

  ```json
  {
    "servers": {
      "json2video": { "type": "http", "url": "https://mcp.json2video.com/mcp" }
    }
  }
  ```

**Connected apps.** Each app you allow gets its own API key, named `<App> (connected app)`, listed in Dashboard → API Keys. To disconnect an app, open Dashboard → Account → **Connected apps** and click **Revoke**: its access and its key are removed. An account can have up to 20 API keys.

**No browser sign-in?** For servers, CI and scripts, send an API key instead, as `Authorization: Bearer YOUR_API_KEY` or `x-api-key: YOUR_API_KEY`:

```bash
claude mcp add --transport http json2video https://mcp.json2video.com/mcp \
  --header "Authorization: Bearer YOUR_API_KEY"
```

**Limits.** A render returns a project ID quickly and the assistant checks its progress (`check_status`, `list_recent_renders`), so long videos keep rendering in the background. `media_upload` on the remote server accepts public URLs only (up to 50 MB); to use files from your computer, upload them in the dashboard or use the local server.

Step-by-step guides and troubleshooting: [MCP server documentation](https://json2video.com/docs/v2/help-for-ai-agents/mcp-server?utm_source=github&utm_medium=mcp-directory&utm_campaign=mcp-listing).

## Run it locally

Use the local server for offline work, CI, editors that only support local (stdio) servers, or to upload files from your disk.

### Requirements

- Node.js 20 or later
- A JSON2Video API key. [Sign up for free](https://json2video.com/get-api-key/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mcp-listing) and copy the key from the [dashboard](https://json2video.com/dashboard/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mcp-listing).

### Installation

#### Claude Code (plugin)

Install the plugin from this repository. Claude Code asks for your API key and stores it securely:

```
/plugin install json2video --marketplace JSON2Video/json2video-mcp
```

On older Claude Code versions, run `/plugin marketplace add JSON2Video/json2video-mcp` first, then `/plugin install json2video@json2video`.

#### Claude Code (manual)

The quickest way, from your project folder:

```bash
npm install -g @json2video/cli
json2video auth --api-key YOUR_API_KEY
json2video mcp install claude
```

This adds the server to the project's `.mcp.json` and installs the `create-video` skill. Use `cursor` or `vscode` instead of `claude` for those editors. Or add it by hand:

```bash
claude mcp add json2video --env JSON2VIDEO_API_KEY=YOUR_API_KEY -- npx -y @json2video/cli mcp run
```

#### Claude Desktop, Cursor, Windsurf and other clients

Add this to your client's MCP configuration file (`claude_desktop_config.json`, `~/.cursor/mcp.json`, `~/.codeium/windsurf/mcp_config.json`…):

```json
{
  "mcpServers": {
    "json2video": {
      "command": "npx",
      "args": ["-y", "@json2video/cli", "mcp", "run"],
      "env": {
        "JSON2VIDEO_API_KEY": "YOUR_API_KEY"
      }
    }
  }
}
```

#### VS Code

Add this to `.vscode/mcp.json`:

```json
{
  "servers": {
    "json2video": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@json2video/cli", "mcp", "run"],
      "env": {
        "JSON2VIDEO_API_KEY": "YOUR_API_KEY"
      }
    }
  }
}
```

Restart your client after saving. Full instructions for every editor: [MCP server documentation](https://json2video.com/docs/v2/help-for-ai-agents/mcp-server?utm_source=github&utm_medium=mcp-directory&utm_campaign=mcp-listing).

## Tools

| Tool | What it does |
|---|---|
| `render_video` | Render a video from a movie JSON or from a template with variables. Returns the video URL. |
| `check_status` | Check the rendering status of a video project. |
| `validate_movie` | Validate a movie JSON without rendering it. |
| `list_templates` | List the templates in your account. |
| `list_featured_templates` | List the templates in the public template library. |
| `get_template` | Get a template's variables or its full source. |
| `save_template` | Save a movie JSON as a reusable template. |
| `get_example` | Get an example movie JSON. |
| `get_help` | Get help on the movie JSON syntax, elements and transitions. |
| `list_recent_renders` | List your latest renders so a video can be found again. |
| `get_account` | Get account information and remaining credits. |
| `media_list` | List files in your media storage. |
| `media_get` | Get a file's details and download URL. |
| `media_upload` | Upload a file from a local path or a public URL (the remote server accepts public URLs only). |
| `media_delete` | Delete a file from your media storage. |

## What runs on your machine (local server)

The plugin and the local configs above run `npx -y @json2video/cli mcp run`. `npx` downloads the published [`@json2video/cli`](https://www.npmjs.com/package/@json2video/cli) package from npm and starts the MCP server locally over stdio. The server sends your requests (movie JSON, template variables, media files you choose to upload) to the JSON2Video API at `api.json2video.com`, authenticated with your API key. To answer help and example requests it also downloads public reference files (documentation, examples, voice and music lists) from `cdn.json2video.com` and JSON2Video's public S3 buckets. It sends no data to any other service.

With the remote server, your AI app sends the same requests to `mcp.json2video.com`, which calls the JSON2Video API on your behalf with the key created for that connection. Nothing is sent to any other service.

## Configuration (local server)

| Environment variable | Required | Description |
|---|---|---|
| `JSON2VIDEO_API_KEY` | Yes | Your JSON2Video API key. |
| `JSON2VIDEO_TIMEOUT` | No | Maximum time to wait for a render, in seconds (default 240). |

Renders use the credits of your JSON2Video account. Free accounts include credits to try it out; see [pricing](https://json2video.com/pricing/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mcp-listing).

## Resources

- [JSON2Video](https://json2video.com/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mcp-listing)
- [Documentation](https://json2video.com/docs/v2/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mcp-listing)
- Remote MCP server URL: `https://mcp.json2video.com/mcp`
- [MCP server guide](https://json2video.com/docs/v2/help-for-ai-agents/mcp-server?utm_source=github&utm_medium=mcp-directory&utm_campaign=mcp-listing)
- [Video editing with Claude Code](https://json2video.com/claude-video/claude-video-editing/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mcp-listing)
- [`@json2video/cli` on npm](https://www.npmjs.com/package/@json2video/cli)
- Support: the chat on [json2video.com](https://json2video.com/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mcp-listing)

## License

[MIT](LICENSE)
