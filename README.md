# Stateful Tools FastAPI Server

This project is an LLM tool server built with FastAPI, demonstrating stateful tools and session-based state management for LLM tool calling.

See also: [OpenAPI Tool Servers](https://github.com/open-webui/openapi-servers).

## Concept

The server demonstrates two key concepts for LLM tool calling:

### Session-Based State Management
The server provides tools that maintain state across multiple interactions by using session IDs:

1. **Session Creation**: Call `POST /session` to get a session ID (the session ID should not be revealed to the user, but must be passed to every tool call).
2. **Stateful Tools**: All other tools (like the calculator) require the session ID to maintain state, such as calculation history.

This approach allows LLMs to work with tools that have persistent state across multiple calls, enabling more complex workflows.

### Documentation as System Prompts
The server uses OpenAPI documentation and endpoint descriptions as additional system prompts to guide LLM behavior. Instructions for the LLM agent (like speaking like a pirate) are embedded directly in the API documentation, creating a self-documenting system where the API spec itself contains behavioral instructions.

## Getting Started

1. Install dependencies: `uv sync`
2. Run the server: `uv run uvicorn main:app --reload`
3. Access the API at `http://127.0.0.1:8000`

## Endpoints

- `POST /session` - Create a new session and get a session ID
- `POST /tools/calculator` - Stateful calculator tool that maintains history

# MCP Proxy

To access this tool server as an MCP server, use a wrapping proxy:

```bash
OPENAPI_SPEC_URL=http://localhost:8000/openapi.json OPENAPI_SIMPLE_MODE=true uvx mcp-openapi-proxy
```