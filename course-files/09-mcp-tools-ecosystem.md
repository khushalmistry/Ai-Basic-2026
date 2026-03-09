# Module 9: MCP Tools & the Model Context Protocol Ecosystem

## Overview

The Model Context Protocol (MCP) is the open standard that lets AI models connect to external tools, data sources, and services. Developed by Anthropic and now adopted industry-wide, MCP has become the USB-C of AI -- a universal connector that works across models and platforms. This module teaches you everything about MCP: how it works, how to use existing MCP servers, and how to build your own.

**By the end of this module, you will:**
- Understand the MCP architecture and why it matters
- Install and use 20+ popular MCP servers
- Build custom MCP servers in Python and TypeScript
- Connect MCP tools to Claude, Cursor, Windsurf, and other clients
- Deploy MCP servers for production use

---

## 1. What Is MCP?

### The Problem MCP Solves

Before MCP, every AI integration was custom-built:
- ChatGPT plugins used one format
- LangChain tools used another
- Each IDE had its own extension system
- No interoperability between platforms

MCP creates a universal standard: build a tool once, use it everywhere.

### Architecture

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  MCP Client  │────│  MCP Server  │────│  Data Source │
│  (Claude,    │    │  (Tool       │    │  (API, DB,   │
│   Cursor)    │    │   Provider)  │    │   Files)     │
└─────────────┘     └─────────────┘     └─────────────┘
```

**Key Components:**
- **MCP Client:** The AI application (Claude Desktop, Cursor, Windsurf, etc.)
- **MCP Server:** A lightweight program that exposes tools, resources, and prompts
- **Transport:** Communication layer (stdio for local, SSE/HTTP for remote)

### MCP Primitives

| Primitive | Description | Example |
|-----------|------------|----------|
| **Tools** | Functions the AI can call | `search_files`, `run_query` |
| **Resources** | Data the AI can read | Database schemas, file contents |
| **Prompts** | Reusable prompt templates | Code review template, analysis prompt |
| **Sampling** | Server requests AI completion | Server asks model to summarize data |

---

## 2. Setting Up MCP Clients

### Claude Desktop

The most popular MCP client. Configuration lives in:

**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/you/Documents"
      ]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_token_here"
      }
    }
  }
}
```

### Cursor IDE

Cursor supports MCP natively since v0.45:

1. Open Settings > MCP
2. Click "Add MCP Server"
3. Configure via JSON or UI

```json
// .cursor/mcp.json (project-level)
{
  "servers": {
    "database": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://localhost:5432/mydb"
      }
    }
  }
}
```

### Windsurf / VS Code

Windsurf added MCP support in 2026. VS Code supports it via the Copilot MCP extension:

```json
// .vscode/mcp.json
{
  "servers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

---

## 3. Essential MCP Servers

### Official Servers (by Anthropic)

| Server | Package | Purpose |
|--------|---------|----------|
| Filesystem | `@modelcontextprotocol/server-filesystem` | Read/write local files |
| GitHub | `@modelcontextprotocol/server-github` | Repos, issues, PRs |
| GitLab | `@modelcontextprotocol/server-gitlab` | GitLab operations |
| Google Drive | `@modelcontextprotocol/server-gdrive` | Drive file access |
| PostgreSQL | `@modelcontextprotocol/server-postgres` | Database queries |
| SQLite | `@modelcontextprotocol/server-sqlite` | Local database |
| Slack | `@modelcontextprotocol/server-slack` | Workspace access |
| Memory | `@modelcontextprotocol/server-memory` | Persistent memory via knowledge graph |
| Fetch | `@modelcontextprotocol/server-fetch` | Web page fetching |
| Puppeteer | `@modelcontextprotocol/server-puppeteer` | Browser automation |

### Community Favorites (2026)

| Server | Purpose | Install |
|--------|---------|----------|
| **Notion MCP** | Full Notion workspace access | `npx @sstan/notion-mcp` |
| **Linear MCP** | Issue tracking integration | `npx @linear/mcp-server` |
| **Brave Search** | Web search via Brave API | `npx @anthropic/mcp-brave-search` |
| **Docker MCP** | Container management | `npx @docker/mcp-server` |
| **Kubernetes MCP** | Cluster operations | `npx @k8s/mcp-server` |
| **AWS MCP** | AWS service management | `npx @aws/mcp-server` |
| **Stripe MCP** | Payment processing | `npx @stripe/mcp-server` |
| **Supabase MCP** | Database + auth + storage | `npx @supabase/mcp-server` |
| **Turbopuffer MCP** | Vector database operations | `npx @turbopuffer/mcp` |
| **Firecrawl MCP** | Web scraping at scale | `npx @firecrawl/mcp-server` |

### Installing Any MCP Server

```bash
# Most servers work with npx (no install needed)
npx -y @modelcontextprotocol/server-filesystem /path/to/dir

# Or install globally
npm install -g @modelcontextprotocol/server-github

# Python servers use uvx or pip
uvx mcp-server-sqlite --db-path ./mydata.db
pip install mcp-server-postgres
```

---

## 4. Building MCP Servers in Python

### Setup

```bash
pip install mcp
# or
uv add mcp
```

### Basic Server Structure

```python
# server.py
from mcp.server import Server
from mcp.server.stdio import stdio_server
from mcp.types import Tool, TextContent
import json

app = Server("my-tool-server")


@app.list_tools()
async def list_tools() -> list[Tool]:
    return [
        Tool(
            name="get_weather",
            description="Get current weather for a city",
            inputSchema={
                "type": "object",
                "properties": {
                    "city": {
                        "type": "string",
                        "description": "City name"
                    }
                },
                "required": ["city"]
            }
        )
    ]


@app.call_tool()
async def call_tool(name: str, arguments: dict) -> list[TextContent]:
    if name == "get_weather":
        city = arguments["city"]
        # In production, call a real weather API
        weather = {
            "city": city,
            "temp": "72°F",
            "condition": "Sunny",
            "humidity": "45%"
        }
        return [TextContent(
            type="text",
            text=json.dumps(weather, indent=2)
        )]
    raise ValueError(f"Unknown tool: {name}")


async def main():
    async with stdio_server() as (read_stream, write_stream):
        await app.run(
            read_stream,
            write_stream,
            app.create_initialization_options()
        )


if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
```

### Adding Resources

```python
from mcp.types import Resource


@app.list_resources()
async def list_resources() -> list[Resource]:
    return [
        Resource(
            uri="config://app-settings",
            name="Application Settings",
            description="Current application configuration",
            mimeType="application/json"
        )
    ]


@app.read_resource()
async def read_resource(uri: str) -> str:
    if uri == "config://app-settings":
        return json.dumps({
            "debug": True,
            "version": "2.1.0",
            "features": ["auth", "logging", "cache"]
        })
    raise ValueError(f"Unknown resource: {uri}")
```

### Adding Prompts

```python
from mcp.types import Prompt, PromptArgument, PromptMessage, TextContent


@app.list_prompts()
async def list_prompts() -> list[Prompt]:
    return [
        Prompt(
            name="code_review",
            description="Review code for bugs and improvements",
            arguments=[
                PromptArgument(
                    name="code",
                    description="The code to review",
                    required=True
                ),
                PromptArgument(
                    name="language",
                    description="Programming language",
                    required=False
                )
            ]
        )
    ]


@app.get_prompt()
async def get_prompt(name: str, arguments: dict) -> list[PromptMessage]:
    if name == "code_review":
        code = arguments["code"]
        lang = arguments.get("language", "unknown")
        return [
            PromptMessage(
                role="user",
                content=TextContent(
                    type="text",
                    text=f"""Review this {lang} code for:
1. Bugs and potential errors
2. Performance issues
3. Security vulnerabilities
4. Code style improvements

Code:
```{lang}
{code}
```"""
                )
            )
        ]
```

### Complete Real-World Example: Database Query Server

```python
# db_mcp_server.py
from mcp.server import Server
from mcp.server.stdio import stdio_server
from mcp.types import Tool, TextContent, Resource
import asyncpg
import json
import os

app = Server("database-query-server")
db_pool = None


async def get_pool():
    global db_pool
    if db_pool is None:
        db_pool = await asyncpg.create_pool(
            os.environ["DATABASE_URL"],
            min_size=2,
            max_size=10
        )
    return db_pool


@app.list_tools()
async def list_tools() -> list[Tool]:
    return [
        Tool(
            name="query",
            description="Execute a read-only SQL query",
            inputSchema={
                "type": "object",
                "properties": {
                    "sql": {
                        "type": "string",
                        "description": "SQL SELECT query to execute"
                    },
                    "limit": {
                        "type": "integer",
                        "description": "Max rows to return (default 100)",
                        "default": 100
                    }
                },
                "required": ["sql"]
            }
        ),
        Tool(
            name="describe_table",
            description="Get schema information for a table",
            inputSchema={
                "type": "object",
                "properties": {
                    "table_name": {
                        "type": "string",
                        "description": "Name of the table"
                    }
                },
                "required": ["table_name"]
            }
        )
    ]


@app.call_tool()
async def call_tool(name: str, arguments: dict) -> list[TextContent]:
    pool = await get_pool()

    if name == "query":
        sql = arguments["sql"]
        limit = arguments.get("limit", 100)

        # Safety: only allow SELECT
        if not sql.strip().upper().startswith("SELECT"):
            return [TextContent(
                type="text",
                text="Error: Only SELECT queries are allowed"
            )]

        async with pool.acquire() as conn:
            rows = await conn.fetch(f"{sql} LIMIT {limit}")
            results = [dict(row) for row in rows]
            return [TextContent(
                type="text",
                text=json.dumps(results, indent=2, default=str)
            )]

    elif name == "describe_table":
        table = arguments["table_name"]
        async with pool.acquire() as conn:
            columns = await conn.fetch("""
                SELECT column_name, data_type, is_nullable, column_default
                FROM information_schema.columns
                WHERE table_name = $1
                ORDER BY ordinal_position
            """, table)
            schema = [dict(col) for col in columns]
            return [TextContent(
                type="text",
                text=json.dumps(schema, indent=2)
            )]


@app.list_resources()
async def list_resources() -> list[Resource]:
    pool = await get_pool()
    async with pool.acquire() as conn:
        tables = await conn.fetch("""
            SELECT table_name FROM information_schema.tables
            WHERE table_schema = 'public'
        """)
        return [
            Resource(
                uri=f"db://table/{t['table_name']}",
                name=t["table_name"],
                description=f"Database table: {t['table_name']}"
            )
            for t in tables
        ]


async def main():
    async with stdio_server() as (read, write):
        await app.run(read, write, app.create_initialization_options())

if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
```

---

## 5. Building MCP Servers in TypeScript

### Setup

```bash
npm init -y
npm install @modelcontextprotocol/sdk
npm install -D typescript @types/node
npx tsc --init
```

### Basic TypeScript Server

```typescript
// src/index.ts
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import {
  CallToolRequestSchema,
  ListToolsRequestSchema,
} from "@modelcontextprotocol/sdk/types.js";

const server = new Server(
  { name: "my-ts-server", version: "1.0.0" },
  { capabilities: { tools: {} } }
);

// List available tools
server.setRequestHandler(ListToolsRequestSchema, async () => ({
  tools: [
    {
      name: "calculate",
      description: "Evaluate a math expression",
      inputSchema: {
        type: "object" as const,
        properties: {
          expression: {
            type: "string",
            description: "Math expression to evaluate",
          },
        },
        required: ["expression"],
      },
    },
    {
      name: "convert_units",
      description: "Convert between units",
      inputSchema: {
        type: "object" as const,
        properties: {
          value: { type: "number", description: "Value to convert" },
          from: { type: "string", description: "Source unit" },
          to: { type: "string", description: "Target unit" },
        },
        required: ["value", "from", "to"],
      },
    },
  ],
}));

// Handle tool calls
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  const { name, arguments: args } = request.params;

  if (name === "calculate") {
    try {
      // Safe evaluation (use a proper math parser in production)
      const result = Function(`"use strict"; return (${args?.expression})`)();
      return {
        content: [
          {
            type: "text" as const,
            text: `${args?.expression} = ${result}`,
          },
        ],
      };
    } catch (e: any) {
      return {
        content: [{ type: "text" as const, text: `Error: ${e.message}` }],
        isError: true,
      };
    }
  }

  if (name === "convert_units") {
    const conversions: Record<string, Record<string, number>> = {
      km: { miles: 0.621371, meters: 1000, feet: 3280.84 },
      miles: { km: 1.60934, meters: 1609.34, feet: 5280 },
      kg: { lbs: 2.20462, grams: 1000, oz: 35.274 },
      lbs: { kg: 0.453592, grams: 453.592, oz: 16 },
      celsius: { fahrenheit: -1 }, // Special handling
    };

    const from = args?.from?.toLowerCase();
    const to = args?.to?.toLowerCase();
    const value = args?.value as number;

    let result: number;
    if (from === "celsius" && to === "fahrenheit") {
      result = (value * 9) / 5 + 32;
    } else if (from === "fahrenheit" && to === "celsius") {
      result = ((value - 32) * 5) / 9;
    } else if (conversions[from]?.[to]) {
      result = value * conversions[from][to];
    } else {
      return {
        content: [
          {
            type: "text" as const,
            text: `Cannot convert from ${from} to ${to}`,
          },
        ],
        isError: true,
      };
    }

    return {
      content: [
        {
          type: "text" as const,
          text: `${value} ${from} = ${result.toFixed(4)} ${to}`,
        },
      ],
    };
  }

  throw new Error(`Unknown tool: ${name}`);
});

// Start server
const transport = new StdioServerTransport();
await server.connect(transport);
console.error("Server running on stdio");
```

### Publishing Your Server

```json
// package.json
{
  "name": "@yourname/mcp-server-example",
  "version": "1.0.0",
  "type": "module",
  "bin": {
    "mcp-server-example": "./dist/index.js"
  },
  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js"
  }
}
```

```bash
npm run build
npm publish --access public

# Users can then run:
# npx @yourname/mcp-server-example
```

---

## 6. Remote MCP Servers (HTTP/SSE Transport)

Local stdio servers are great for desktop apps, but production deployments need remote servers.

### SSE Transport Server (Python)

```python
# remote_server.py
from mcp.server import Server
from mcp.server.sse import SseServerTransport
from starlette.applications import Starlette
from starlette.routing import Route, Mount
import uvicorn

app = Server("remote-server")
sse = SseServerTransport("/messages/")

# ... define tools, resources, prompts as before ...

async def handle_sse(request):
    async with sse.connect_sse(
        request.scope, request.receive, request._send
    ) as streams:
        await app.run(
            streams[0], streams[1],
            app.create_initialization_options()
        )

starlette_app = Starlette(
    routes=[
        Route("/sse", endpoint=handle_sse),
        Mount("/messages/", app=sse.handle_post_message),
    ]
)

if __name__ == "__main__":
    uvicorn.run(starlette_app, host="0.0.0.0", port=8080)
```

### Streamable HTTP Transport (2026 Standard)

```python
# The newer Streamable HTTP transport is now preferred over SSE
from mcp.server.streamable_http import StreamableHTTPServer

http_server = StreamableHTTPServer(
    app,
    host="0.0.0.0",
    port=8080,
    path="/mcp"
)

# Single endpoint handles all MCP communication
# POST /mcp for requests, GET /mcp for streaming
await http_server.run()
```

### Connecting Clients to Remote Servers

```json
// Claude Desktop config for remote server
{
  "mcpServers": {
    "remote-tools": {
      "url": "https://your-server.com/sse",
      "transport": "sse"
    }
  }
}
```

---

## 7. MCP Server Discovery & Registries

### Finding MCP Servers

| Registry | URL | Description |
|----------|-----|------------|
| **Smithery** | smithery.ai | Largest MCP server registry |
| **MCP Hub** | mcphub.io | Curated collection with reviews |
| **Awesome MCP** | github.com/punkpeye/awesome-mcp-servers | GitHub community list |
| **Glama** | glama.ai/mcp/servers | Searchable directory |
| **npm** | npmjs.com (search "mcp-server") | Package registry |
| **PyPI** | pypi.org (search "mcp-server") | Python packages |

### MCP Inspector (Testing Tool)

```bash
# Test any MCP server interactively
npx @modelcontextprotocol/inspector

# Opens a web UI where you can:
# - Connect to any MCP server
# - List and test tools
# - View resources and prompts
# - Debug communication
```

---

## 8. Advanced Patterns

### Multi-Server Composition

Connect multiple MCP servers to create powerful workflows:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "ghp_xxx" }
    },
    "database": {
      "command": "python",
      "args": ["db_server.py"],
      "env": { "DATABASE_URL": "postgres://..." }
    },
    "slack": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-slack"],
      "env": { "SLACK_TOKEN": "xoxb-xxx" }
    }
  }
}
```

Now Claude can: "Check the latest GitHub PRs, query the database for related customer tickets, and post a summary to Slack" -- all in one conversation.

### Authentication for Remote Servers

```python
from starlette.middleware import Middleware
from starlette.middleware.authentication import AuthenticationMiddleware

# Add API key authentication
async def verify_api_key(request):
    api_key = request.headers.get("Authorization", "").replace("Bearer ", "")
    if api_key != os.environ["MCP_API_KEY"]:
        return JSONResponse({"error": "Unauthorized"}, status_code=401)

starlette_app = Starlette(
    routes=[...],
    middleware=[Middleware(AuthenticationMiddleware, backend=verify_api_key)]
)
```

### Error Handling Best Practices

```python
@app.call_tool()
async def call_tool(name: str, arguments: dict) -> list[TextContent]:
    try:
        result = await execute_tool(name, arguments)
        return [TextContent(type="text", text=json.dumps(result))]
    except ValidationError as e:
        return [TextContent(
            type="text",
            text=f"Invalid input: {e.message}"
        )]
    except ExternalAPIError as e:
        return [TextContent(
            type="text",
            text=f"External service error: {e.status_code} - {e.message}"
        )]
    except Exception as e:
        # Log internally, return safe message
        logger.error(f"Tool {name} failed: {e}")
        return [TextContent(
            type="text",
            text=f"Tool execution failed. Please try again."
        )]
```

---

## 9. Deploying MCP Servers

### Docker Deployment

```dockerfile
# Dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 8080
CMD ["python", "remote_server.py"]
```

### Cloud Deployment Options

| Platform | Best For | Setup |
|----------|----------|-------|
| **Railway** | Quick deploys | `railway up` |
| **Fly.io** | Edge deployment | `fly deploy` |
| **AWS Lambda** | Serverless | Via SAM/CDK |
| **Cloudflare Workers** | Edge compute | `wrangler deploy` |
| **Google Cloud Run** | Auto-scaling | `gcloud run deploy` |

### Production Checklist

- [ ] Authentication (API keys or OAuth)
- [ ] Rate limiting per client
- [ ] Input validation on all tool arguments
- [ ] Logging and monitoring
- [ ] Health check endpoint
- [ ] Graceful shutdown handling
- [ ] CORS configuration for web clients
- [ ] TLS/HTTPS for remote servers

---

## 10. Hands-On Projects

### Project 1: Personal Knowledge Base MCP Server

Build a server that lets Claude search and manage your notes:

```python
# Features:
# - Tool: search_notes(query) - semantic search across notes
# - Tool: create_note(title, content, tags) - save new notes
# - Tool: list_tags() - show all tags
# - Resource: notes://recent - last 10 notes
# - Prompt: summarize_topic(topic) - summarize all notes on a topic
```

### Project 2: API Gateway MCP Server

Wrap any REST API as MCP tools:

```python
# Load OpenAPI spec, auto-generate MCP tools
import yaml

def openapi_to_mcp_tools(spec_path: str) -> list[Tool]:
    with open(spec_path) as f:
        spec = yaml.safe_load(f)
    
    tools = []
    for path, methods in spec["paths"].items():
        for method, details in methods.items():
            tools.append(Tool(
                name=details.get("operationId", f"{method}_{path}"),
                description=details.get("summary", ""),
                inputSchema=build_schema(details.get("parameters", []))
            ))
    return tools
```

### Project 3: Team Dashboard MCP Server

Aggregate data from multiple services:

```python
# Tools:
# - get_sprint_status() - Linear/Jira sprint data
# - get_deploy_status() - Latest deployments
# - get_incidents() - PagerDuty/Opsgenie alerts
# - get_metrics() - Key business metrics
# All in one server, one conversation
```

---

## Summary

| Topic | Key Takeaway |
|-------|-------------|
| MCP Architecture | Client-server model with tools, resources, prompts |
| Clients | Claude Desktop, Cursor, Windsurf, VS Code all support MCP |
| Python SDK | `mcp` package with decorator-based API |
| TypeScript SDK | `@modelcontextprotocol/sdk` with handler pattern |
| Transport | stdio for local, SSE/HTTP for remote |
| Discovery | Smithery, MCP Hub, npm, PyPI |
| Production | Docker + cloud deploy with auth and monitoring |

**Next Module:** [Module 10: No-Code AI Automation](./10-no-code-ai-automation.md) -- Build powerful AI workflows without writing code.