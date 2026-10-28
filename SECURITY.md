# Security

Relex lets you use Grok on legal matters **without exposing client PII**.

## Authentication

- MCP: `https://relex.legal/api/mcp`
- OAuth 2.1 + PKCE when the host supports browser auth
- Relex API key as `authorization` / bearer for xAI remote MCP and CI
- xAI API keys are separate from Relex credentials

## Client PII never reaches the model

- Parties sealed client-side under PII password
- Documents read and redacted by the Relex Private Model on Relex servers by default (browser mode keeps reading on the device)
- MCP `execute` blocks plaintext party/document endpoints

## Reporting

**security@relex.legal**
