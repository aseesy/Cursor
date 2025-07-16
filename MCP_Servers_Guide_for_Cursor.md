# Complete Guide: Adding MCP Servers to Cursor Projects

## What is MCP (Model Context Protocol)?

**Model Context Protocol (MCP)** is an open standard developed by Anthropic that allows applications to share context and tools with large language models (LLMs) through a unified interface. In Cursor IDE, MCP acts as a plugin framework that enables you to extend the built-in AI assistant with custom tools and integrations.

### Why Use MCP in Cursor?

- **Extend Functionality**: Connect Cursor to external tools and data sources
- **Customize AI**: Build custom tools that work exactly how you need them
- **Flexibility**: Write servers in any language (Python, JavaScript, Go, etc.)
- **Context Management**: Give AI models access to relevant external context
- **Plug-and-Play**: Easy integration with minimal configuration

## How MCP Works

MCP servers expose capabilities through the protocol, connecting Cursor to external tools or data sources. Cursor supports three transport methods:

| Transport | Environment | Deployment | Users | Input | Auth |
|-----------|-------------|------------|--------|-------|------|
| **stdio** | Local | Cursor manages | Single user | Shell command | Manual |
| **SSE** | Local/Remote | Deploy as server | Multiple users | URL to SSE endpoint | OAuth |
| **HTTP** | Local/Remote | Deploy as server | Multiple users | URL to HTTP endpoint | OAuth |

## Installation Methods

### Method 1: One-Click Installation (Recommended)

Cursor provides a directory of pre-built MCP servers that you can install with one click:

1. **Browse Available Servers**: Visit [cursor.directory/mcp](https://cursor.directory/mcp)
2. **Install via Settings**: 
   - Open Cursor Settings (Ctrl+Shift+J or Cmd+Shift+J)
   - Navigate to **Features → Model Context Protocol**
   - Click "Browse MCP Tools"
   - Find your desired server and click "Add to Cursor"

### Method 2: Manual Configuration with mcp.json

For custom servers or advanced configurations, you'll need to create an `mcp.json` file.

#### Configuration File Locations

- **Project-specific**: `.cursor/mcp.json` (in your project root)
- **Global**: `~/.cursor/mcp.json` (in your home directory)
  - **Windows**: `%USERPROFILE%\.cursor\mcp.json`
  - **macOS/Linux**: `~/.cursor/mcp.json`

#### Basic Configuration Examples

**Node.js MCP Server (stdio transport):**
```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@your/mcp-server"],
      "env": {
        "API_KEY": "your_api_key_here"
      }
    }
  }
}
```

**Python MCP Server:**
```json
{
  "mcpServers": {
    "python-server": {
      "command": "python",
      "args": ["/path/to/your/server.py"],
      "env": {
        "PYTHONPATH": "/path/to/your/project"
      }
    }
  }
}
```

**Remote SSE Server:**
```json
{
  "mcpServers": {
    "remote-server": {
      "url": "http://localhost:7070/sse",
      "transport": "sse"
    }
  }
}
```

### Method 3: Using MCP Installer

The MCP Installer is a tool that streamlines the installation process:

1. **Install globally**: `npm install -g cursor-mcp-installer-free`
2. **Add to mcp.json**:
```json
{
  "mcpServers": {
    "mcp-installer": {
      "command": "npx",
      "args": ["cursor-mcp-installer-free"]
    }
  }
}
```
3. **Restart Cursor**
4. **Use Claude to install servers**: Ask Claude to install any MCP server for you

## Popular MCP Servers

### Development Tools

**GitHub MCP Server**
- Manage repository issues and pull requests
- Search repositories and manage code
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your_token"
      }
    }
  }
}
```

**Browser MCP Server**
- Web automation and scraping
- Browser interaction and testing
```json
{
  "mcpServers": {
    "browser": {
      "command": "npx",
      "args": ["@browsermcp/mcp@latest"]
    }
  }
}
```

**Playwright MCP Server**
- End-to-end testing automation
- DOM context for better test writing
```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["playwright-mcp"]
    }
  }
}
```

### Database & Storage

**Supabase MCP Server**
- Database operations and queries
- Real-time data access
```json
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": ["-y", "@supabase/mcp-server"],
      "env": {
        "SUPABASE_URL": "your_supabase_url",
        "SUPABASE_ANON_KEY": "your_anon_key"
      }
    }
  }
}
```

**Google Drive MCP Server**
- File management and storage
- Document access and search
```json
{
  "mcpServers": {
    "google-drive": {
      "command": "npx",
      "args": ["-y", "@google/drive-mcp-server"],
      "env": {
        "GOOGLE_CLIENT_ID": "your_client_id",
        "GOOGLE_CLIENT_SECRET": "your_client_secret"
      }
    }
  }
}
```

### Productivity & Notes

**Obsidian MCP Server**
- Read and search Markdown notes
- Link notes to coding process
```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["-y", "@obsidian/mcp-server"],
      "env": {
        "OBSIDIAN_VAULT_PATH": "/path/to/your/vault"
      }
    }
  }
}
```

**Notion MCP Server**
- Access Notion databases and pages
- Integration with project management
```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@notion/mcp-server"],
      "env": {
        "NOTION_API_KEY": "your_notion_api_key"
      }
    }
  }
}
```

## Using MCP Tools in Cursor

### Enabling Tools

1. **Open Cursor Composer** (Ctrl+Shift+I or Cmd+Shift+I)
2. **Switch to Agent Mode**
3. **Check Available Tools**: Look for "Available Tools" section in the sidebar
4. **Toggle Tools**: Click on tool names to enable/disable them

### Invoking Tools

You can use MCP tools in several ways:

**Direct Tool Request:**
```
"Use the GitHub tool to create a new issue titled 'Bug fix needed'"
```

**Contextual Usage:**
```
"Help me write a Playwright test for the login form"
```

**Tool-Specific Commands:**
```
"Search my Obsidian notes for information about authentication"
```

### Tool Approval & Auto-run

- **Manual Approval**: By default, Cursor asks for approval before using MCP tools
- **Auto-run Mode**: Enable in settings for automatic tool execution
- **Tool Arguments**: Click the arrow next to tool names to see arguments

## Creating Custom MCP Servers

### Quick Start Example

Here's a simple Node.js MCP server:

```javascript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

const server = new McpServer({
  name: "Custom Calculator",
  version: "1.0.0"
});

server.tool("add",
  { a: z.number(), b: z.number() },
  async ({ a, b }) => ({
    content: [{ type: "text", text: String(a + b) }]
  })
);

const transport = new StdioServerTransport();
await server.connect(transport);
```

### Python Example

```python
import asyncio
from mcp.server import Server
from mcp.types import Tool, TextContent

server = Server("my-python-server")

@server.tool("greet")
async def greet(name: str) -> list[TextContent]:
    return [TextContent(type="text", text=f"Hello, {name}!")]

async def main():
    await server.run()

if __name__ == "__main__":
    asyncio.run(main())
```

## Troubleshooting

### Common Issues

**1. Connection Issues**
- **Symptom**: MCP server won't connect
- **Solutions**:
  - Check server endpoint URL
  - Verify JSON configuration syntax
  - Ensure required dependencies are installed
  - Check firewall settings for remote servers

**2. Tool Not Appearing**
- **Symptom**: MCP tools not visible in Cursor
- **Solutions**:
  - Restart Cursor after configuration changes
  - Check MCP logs in Output panel (Ctrl+Shift+U → "MCP Logs")
  - Verify server is running correctly
  - Check tool limits in Cursor settings

**3. Authentication Failures**
- **Symptom**: Server fails to authenticate
- **Solutions**:
  - Verify API keys and tokens
  - Check environment variable names
  - Ensure OAuth setup is complete
  - Review server documentation for auth requirements

### Debugging Steps

1. **Check MCP Logs**:
   - Open Output panel (Ctrl+Shift+U)
   - Select "MCP Logs" from dropdown
   - Look for errors, warnings, or connection issues

2. **Test Server Manually**:
   - Run server command in terminal
   - Use MCP inspector: `npx @modelcontextprotocol/inspector@latest`
   - Verify server responds to requests

3. **Validate Configuration**:
   - Check JSON syntax in mcp.json
   - Verify file paths are correct
   - Ensure environment variables are set

### Working with npx-based Servers

If you have issues with npx-based servers, use `mcp-proxy`:

1. **Install mcp-proxy**: `uv tool install mcp-proxy`
2. **Run proxy**: `mcp-proxy --sse-port 7070 --pass-environment npx -- your-server-name`
3. **Configure Cursor**: Add SSE server with URL `http://localhost:7070/sse`

## Security Considerations

- **Verify Sources**: Only install MCP servers from trusted developers
- **Review Permissions**: Check what data and APIs servers will access
- **Limit API Keys**: Use restricted API keys with minimal permissions
- **Audit Code**: Review server source code for critical integrations
- **Environment Variables**: Use environment variables for sensitive data

## Best Practices

1. **Start Small**: Begin with simple, well-documented servers
2. **Test Locally**: Always test servers locally before deployment
3. **Monitor Usage**: Keep track of API usage and costs
4. **Keep Updated**: Regularly update MCP servers to latest versions
5. **Documentation**: Document your MCP server configurations
6. **Backup Configs**: Keep backups of your mcp.json configurations

## Advanced Configuration

### Multiple Environments

```json
{
  "mcpServers": {
    "dev-database": {
      "command": "npx",
      "args": ["@supabase/mcp-server"],
      "env": {
        "SUPABASE_URL": "https://dev-project.supabase.co",
        "SUPABASE_ANON_KEY": "dev_key"
      }
    },
    "prod-database": {
      "command": "npx",
      "args": ["@supabase/mcp-server"],
      "env": {
        "SUPABASE_URL": "https://prod-project.supabase.co",
        "SUPABASE_ANON_KEY": "prod_key"
      }
    }
  }
}
```

### Conditional Server Loading

You can create different mcp.json files for different projects and environments, allowing you to load only relevant servers for each context.

## Useful Resources

- **Official Documentation**: [docs.cursor.com/context/model-context-protocol](https://docs.cursor.com/context/model-context-protocol)
- **MCP Server Directory**: [cursor.directory/mcp](https://cursor.directory/mcp)
- **MCP SDK Documentation**: [modelcontextprotocol.io](https://modelcontextprotocol.io)
- **Community Forum**: [forum.cursor.com](https://forum.cursor.com)
- **GitHub Examples**: Search for "mcp-server" repositories

## Next Steps

1. **Choose Your First Server**: Start with a simple server like GitHub or file operations
2. **Set Up Configuration**: Create your mcp.json file
3. **Test Integration**: Verify the server works in Cursor
4. **Explore Advanced Features**: Try multiple servers and custom configurations
5. **Build Custom Tools**: Create your own MCP server for specific needs

Remember: MCP servers significantly enhance Cursor's capabilities by providing context-aware assistance and expanding the AI's access to external tools and data sources. Start with pre-built servers and gradually move to custom solutions as your needs grow.