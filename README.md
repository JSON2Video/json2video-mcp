# JSON2Video MCP Server

<img src="assets/logo.svg" alt="JSON2Video" width="72" align="right">

Official [Model Context Protocol](https://modelcontextprotocol.io/) server for **[JSON2Video](https://json2video.com)**, the video API for automated video production at scale.

Connect your AI agent (Claude, Cursor, VS Code, Windsurf, Cline…) to JSON2Video and let it produce finished MP4 videos: build a video from reusable templates, add AI voiceovers, subtitles, AI-generated images and video, render it in the cloud and get back a URL you can publish.

**Built and maintained by the JSON2Video team.** The server ships inside the [`@json2video/cli`](https://www.npmjs.com/package/@json2video/cli) npm package and runs locally over stdio.

Listed in the official MCP Registry as `com.json2video/mcp`.

## What you can do

- "Make a 30-second vertical video about our new product with a voiceover and subtitles."
- "Render my *Real estate listing* template for each of these 10 properties."
- "Create a template for daily news shorts, then render today's three stories."
- "Upload this logo and add it as a watermark to the video."
- "How many credits do I have left?"

## Requirements

- Node.js 20 or later
- A JSON2Video API key. [Sign up for free](https://json2video.com/get-api-key/) and copy the key from the [dashboard](https://json2video.com/dashboard).

## Installation

### Claude Code (plugin)

Install the plugin from this repository. Claude Code asks for your API key and stores it securely:

```
/plugin install json2video --marketplace JSON2Video/json2video-mcp
```

On older Claude Code versions, run `/plugin marketplace add JSON2Video/json2video-mcp` first, then `/plugin install json2video@json2video`.

### Claude Code (manual)

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

### Claude Desktop, Cursor, Windsurf and other clients

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

### VS Code

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

Restart your client after saving. Full instructions for every editor: [MCP server documentation](https://json2video.com/docs/v2/help-for-ai-agents/mcp-server).

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
| `get_account` | Get account information and remaining credits. |
| `media_list` | List files in your media storage. |
| `media_get` | Get a file's details and download URL. |
| `media_upload` | Upload a file from a local path or a public URL. |
| `media_delete` | Delete a file from your media storage. |

## What runs on your machine

The plugin and the configs above run `npx -y @json2video/cli mcp run`. `npx` downloads the published [`@json2video/cli`](https://www.npmjs.com/package/@json2video/cli) package from npm and starts the MCP server locally over stdio. The server sends your requests (movie JSON, template variables, media files you choose to upload) to the JSON2Video API at `api.json2video.com`, authenticated with your API key. To answer help and example requests it also downloads public reference files (documentation, examples, voice and music lists) from `cdn.json2video.com` and JSON2Video's public S3 buckets. It sends no data to any other service.

## Configuration

| Environment variable | Required | Description |
|---|---|---|
| `JSON2VIDEO_API_KEY` | Yes | Your JSON2Video API key. |
| `JSON2VIDEO_TIMEOUT` | No | Maximum time to wait for a render, in seconds (default 240). |

Renders use the credits of your JSON2Video account. Free accounts include credits to try it out; see [pricing](https://json2video.com/pricing/).

## Resources

- [JSON2Video](https://json2video.com)
- [Documentation](https://json2video.com/docs/v2/)
- [MCP server guide](https://json2video.com/docs/v2/help-for-ai-agents/mcp-server)
- [Video editing with Claude Code](https://json2video.com/claude-video/claude-video-editing/)
- [`@json2video/cli` on npm](https://www.npmjs.com/package/@json2video/cli)
- Support: the chat on [json2video.com](https://json2video.com)

## License

[MIT](LICENSE)
