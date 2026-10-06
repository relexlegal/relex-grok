# Relex × Grok (xAI)

**Legal Workspace — One source of truth for any Agent — confidential by design**

Keep legal knowledge, matter context and saved progress in Relex, independently
of the assistant you use. Authorize another compatible agent to continue from
the same stored context, without rebuilding the background in another chat.

## Portable context, with your permission

1. Create or open the matter in Relex and add information through its protected
   intake and document workflows.
2. Connect a supported client to `https://relex.legal/api/mcp` and authorize
   your own Relex account. Installing a package does not authorize private data.
3. Ask the agent to read the permitted matter context before working and save
   its conclusions when finished. A second authorized client can then use that
   continuing record.

Portability covers information saved in Relex, not automatic import of private
chat histories or a model's internal memory. Client-side identity encryption,
de-identification and MCP access controls protect the supported workflows;
de-identified legal facts may still be sensitive. Review what you authorize.

## Workspace, SDK and Marketplace

Use **Legal Workspace** for persistent legal context; the **Legal SDK** for
building your firm's or legal department's own platform; and the
**Legal Marketplace** to discover published professional profiles or make an
AI-first law firm discoverable across specialties.

An agent may help find a professional and prepare a reference-only request.
The user must review and approve sharing in Relex. Discovery is not engagement,
a completed conflict check, payment or a guarantee of professional availability.

Client support depends on the host product, plan and administrator settings.
Gemini CLI support does not imply support in every Gemini web experience.
Harvey BYOMCP is a customer-admin connection path, not a claim of Harvey
Connector Library listing or approval. Check the current
[connector guides](https://relex.legal/docs/connectors) and
[portable-context guide](https://relex.legal/guides/portable-legal-context).
## Official xAI / Grok names

| Surface | Product calls it |
|---------|------------------|
| **grok.com** | **Connector** → New Connector → **Custom** |
| **Grok Build** | **MCP server** and/or **plugin** |
| **xAI API** | **Remote MCP tool** (`type: "mcp"`, `server_url`) |

Always label **Relex**, URL `https://relex.legal/api/mcp`.

## How it works

Grok reaches the same MCP endpoint via Streamable HTTP. Two tools — `search` and
`execute` — with a fixed ~1k-token footprint. Auth: **OAuth** (browser) on
grok.com / Grok Build, or `authorization` / bearer **Relex API key** for xAI API
/ headless.

Party data is sealed client-side; documents are redacted client-side by
default; `execute` refuses plaintext PII and returns deep links.

OAuth detail for any host: https://relex.legal/docs/connectors/mcp


## MCP tools (remote server)

The hosted connector at `https://relex.legal/api/mcp` exposes **eleven** tools:
`list_matters`, `read_matter_context`, `diagnose_matter_sources`,
`save_matter_work_product`, `correct_matter_ontology`, `conclude_matter_session`,
`find_legal_professionals`, `read_legal_professional`, `prepare_professional_request`,
`search`, and `execute`. Auth is OAuth 2.1 + PKCE; connector scopes are
`relex.cases.read relex.cases.write relex.draft`.

Grok connectors use `https://grok.com/connectors-oauth-exchange-code/` (also `console.x.ai`). Grok Build CLI: `grok mcp add --transport http` or `.mcp.json`.

## Quick start

### grok.com Connectors

1. [grok.com/connectors](https://grok.com/connectors) → **New Connector** → **Custom**
2. URL `https://relex.legal/api/mcp`, name **Relex** → complete OAuth

### xAI API (Responses / native SDK)

```python
from xai_sdk import Client
from xai_sdk.tools import mcp
import os

client = Client(api_key=os.getenv("XAI_API_KEY"))
chat = client.chat.create(
    model="grok-4.5",
    tools=[
        mcp(
            server_url="https://relex.legal/api/mcp",
            server_label="relex",
            server_description="Relex legal case management — PII-safe MCP",
            # For headless: pass a Relex API key
            # authorization=os.getenv("RELEX_API_KEY"),
        ),
    ],
)
```

OpenAI-compatible Responses shape:

```json
{
  "type": "mcp",
  "server_url": "https://relex.legal/api/mcp",
  "server_label": "relex",
  "server_description": "Relex legal case management — PII-safe MCP"
}
```

With Relex API key (Settings → API Keys):

```json
{
  "type": "mcp",
  "server_url": "https://relex.legal/api/mcp",
  "server_label": "relex",
  "authorization": "rlx_..."
}
```

### Grok Build (recommended)

```bash
grok plugin install relexlegal/relex-grok#plugin --trust
grok plugin enable relex-legal
# Authenticate MCP (browser OAuth) via /mcps → relex → i, or:
grok mcp add --transport http relex https://relex.legal/api/mcp
```

Or MCP-only:

```toml
# ~/.grok/config.toml
[mcp_servers.relex]
url = "https://relex.legal/api/mcp"
enabled = true
```

Full guide: [`docs/connect-grok-build.md`](docs/connect-grok-build.md).

## Personal vs Team

| Context | Who configures MCP | Who authenticates to Relex |
|---------|--------------------|----------------------------|
| **Individual** xAI API key / personal Grok | You add the remote MCP tool in your app or Grok Build config | You — OAuth or your Relex API key |
| **Team / org** shared Grok or xAI workspace | Admin publishes the Relex MCP config / secrets policy | Each user should use **their own** Relex OAuth or key; do not share PII passwords |

If your product surfaces **Connectors** with admin install (similar to Claude /
ChatGPT Team): admin installs once; members only click **Connect**.

Full guide: [`docs/install.md`](docs/install.md).

## Layout

```
relex-grok/
├── plugin/
│   ├── .mcp.json
│   ├── plugin.json
│   ├── skills/
│   ├── agents/
│   ├── commands/
│   └── references/
├── docs/
│   ├── install.md
│   ├── connect-xai-api.md
│   ├── connect-grok-build.md
│   └── positioning.md
└── SECURITY.md
```

## Docs on relex.legal

- [Grok connector](https://relex.legal/docs/connectors/grok)
- [MCP Server](https://relex.legal/docs/mcp)
- [For AI Agents](https://relex.legal/for-agents)

## License

AGPL-3.0-or-later — see [LICENSE](LICENSE).
