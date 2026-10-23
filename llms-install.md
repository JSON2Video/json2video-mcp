# Installing the JSON2Video MCP server

Instructions for AI agents (Cline and similar) installing this server for a user.

1. Check that Node.js 20 or later is available: `node --version`.
2. Ask the user for their JSON2Video API key. If they do not have one, they can sign up for free at https://json2video.com/get-api-key/?utm_source=llms-install&utm_medium=mcp-directory&utm_campaign=mcp-listing and copy the key from https://json2video.com/dashboard.?utm_source=llms-install&utm_medium=mcp-directory&utm_campaign=mcp-listing Never invent a key.
3. Add this entry to the MCP settings file, replacing `YOUR_API_KEY` with the user's key:

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

4. No build step and no repository clone are needed: `npx` downloads the published `@json2video/cli` package from npm.
5. To verify the installation, call the `get_account` tool. It returns the account details and remaining credits. If it fails with an authentication error, the API key is wrong.
6. Before writing a movie JSON, call `get_help` to learn the syntax, or `get_example` to start from a working example. Use `validate_movie` before `render_video`.
