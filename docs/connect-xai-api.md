# Connect Relex via xAI Remote MCP Tools

xAI **Remote MCP Tools** attach external MCP servers to Grok. Streaming HTTP
and SSE are supported. Relex serves Streamable HTTP at:

```
https://relex.legal/api/mcp
```

## Native xAI SDK

```python
import os
from xai_sdk import Client
from xai_sdk.chat import user
from xai_sdk.tools import mcp

client = Client(api_key=os.getenv("XAI_API_KEY"))

chat = client.chat.create(
    model="grok-4.5",
    tools=[
        mcp(
            server_url="https://relex.legal/api/mcp",
            server_label="relex",
            server_description="Relex legal workspace — typed matter workflows with redacted, paged source reading",
            # Optional headless auth:
            # authorization=os.getenv("RELEX_API_KEY"),
        ),
    ],
)

chat.append(user("Set up my practice workflow with Relex."))
for response, chunk in chat.stream():
    if chunk.content:
        print(chunk.content, end="")
```

## OpenAI-compatible Responses API

```python
from openai import OpenAI
import os

client = OpenAI(api_key=os.getenv("XAI_API_KEY"), base_url="https://api.x.ai/v1")

response = client.responses.create(
    model="grok-4.5",
    input="List my Relex onboarding status.",
    tools=[
        {
            "type": "mcp",
            "server_url": "https://relex.legal/api/mcp",
            "server_label": "relex",
            "server_description": "Relex legal case management",
            # "authorization": os.getenv("RELEX_API_KEY"),
        }
    ],
)
print(response)
```

## Allowed tools (optional)

Relex exposes typed workflows for matter reading, source diagnosis, paged source
reading, and grounded work products, plus `search` and `execute` for operations
without a typed workflow. Do not pin only `search` and `execute`, or the agent
will lose the safer matter workflow tools. Omit `allowed_tool_names` to expose
the full server tool list:

```python
mcp(
    server_url="https://relex.legal/api/mcp",
    server_label="relex",
)
```

## Notes

- `require_approval` / `connector_id` from OpenAI Responses are not required by xAI.
- Use HTTPS only.
- Prefer per-user Relex credentials; do not share firm PII passwords via env.
