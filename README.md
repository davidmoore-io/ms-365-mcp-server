# ms-365-mcp-server

[![npm version](https://img.shields.io/npm/v/@davidmoore-io/ms-365-mcp-server.svg)](https://www.npmjs.com/package/@davidmoore-io/ms-365-mcp-server) [![build status](https://github.com/davidmoore-io/ms-365-mcp-server/actions/workflows/build.yml/badge.svg)](https://github.com/davidmoore-io/ms-365-mcp-server/actions/workflows/build.yml) [![license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/davidmoore-io/ms-365-mcp-server/blob/main/LICENSE)

Microsoft 365 MCP Server - Fork with Pre-authenticated Token Support

> **Fork Notice**: This is a fork of the original [@softeria/ms-365-mcp-server](https://www.npmjs.com/package/@softeria/ms-365-mcp-server) with added support for pre-authenticated tokens via environment variables. This allows for better integration with applications that already handle Microsoft authentication.

A Model Context Protocol (MCP) server for interacting with Microsoft 365 services through the Graph API.

## Prerequisites

- Node.js >= 14

## Features

- Authentication via Microsoft Authentication Library (MSAL)
- Excel file operations
- Calendar event management
- Mail operations
- OneDrive file management
- OneNote notebooks and pages
- To Do tasks and task lists
- Planner plans and tasks
- Outlook contacts

## Quick Start Example

Test login in Claude Desktop:

![Login example](https://github.com/user-attachments/assets/e457884f-c98a-4186-9e6f-eb323ec24e0a)

## Examples

![Image](https://github.com/user-attachments/assets/1a296afb-48ed-42b0-9e7c-e685d5d1784c)

## Integration

### Claude Desktop

To add this MCP server to Claude Desktop:

Edit the config file under Settings > Developer:

```json
{
  "mcpServers": {
    "ms365": {
      "command": "npx",
      "args": [
        "-y",
        "@davidmoore-io/ms-365-mcp-server"
      ]
    }
  }
}
```

### Claude Code CLI

```bash
claude mcp add ms365 -- npx -y @davidmoore-io/ms-365-mcp-server
```

For other interfaces that support MCPs, please refer to their respective documentation for the correct
integration method.

### Authentication

> ⚠️ You must authenticate before using tools.

#### Option 1: Pre-authenticated Token (New!)

If you already have a Microsoft 365 access token (e.g., from your application), you can pass it via environment variable:

```bash
MS365_ACCESS_TOKEN="your-access-token" npx @softeria/ms-365-mcp-server
```

Or in your MCP configuration:

```json
{
  "mcpServers": {
    "ms365": {
      "command": "npx",
      "args": ["-y", "@softeria/ms-365-mcp-server"],
      "env": {
        "MS365_ACCESS_TOKEN": "your-access-token"
      }
    }
  }
}
```

The server will validate the token on startup and use it for all API calls. If the token is invalid or expires, it will fall back to the standard authentication flow.

#### Option 2: Interactive Device Code Flow (Original)

1. **MCP client login**:
    - Call the `login` tool (auto-checks existing token)
    - If needed, get URL+code, visit in browser
    - Use `verify-login` tool to confirm
2. **Optional CLI login**:
   ```bash
   npx @softeria/ms-365-mcp-server --login
   ```
   Follow the URL and code prompt in the terminal.

Tokens are cached securely in your OS credential store (fallback to file).

## CLI Options

The following options can be used when running ms-365-mcp-server directly from the command line:

```
--login           Login using device code flow
--logout          Log out and clear saved credentials
--verify-login    Verify login without starting the server
```

### Server Options

When running as an MCP server, the following options can be used:

```
-v                Enable verbose logging
--read-only       Start server in read-only mode, disabling write operations
```

Environment variables:

- `READ_ONLY=true|1`: Alternative to --read-only flag
- `LOG_LEVEL`: Set logging level (default: 'info')
- `SILENT=true`: Disable console output

## License

MIT © 2025 Softeria
