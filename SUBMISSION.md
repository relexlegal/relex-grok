# Submission — xAI (Grok Bot / Grok Build marketplace)

> Prepared 2026-10-28 on branch `directory-review-2026-10`. Nothing has been
> submitted, pushed or deployed. Before any submission: deploy the backend branch
> `directory-review-2026-10` (neutral server instructions, data-only responses),
> then merge and push this repo.

Hosted server: `https://relex.legal/api/mcp` (streamable HTTP, OAuth 2.1 with
PKCE and dynamic client registration). Twelve tools: `list_matters`,
`read_matter_context`, `diagnose_matter_sources`, `read_matter_source`,
`save_matter_work_product`, `correct_matter_ontology`, `conclude_matter_session`,
`find_legal_professionals`, `read_legal_professional`,
`prepare_professional_request`, `search`, `execute`.

## Done in this branch
- [x] Plugin 0.1.8 (`plugin/plugin.json`, `plugin/.grok-plugin/plugin.json`, `.grok-plugin/marketplace.json`), description says "legal practice management".
- [x] README: twelve tools; reading default corrected. Skills audited (0 unknown endpoints).

## User must do — form (https://x.ai/bot/marketplace/submit-a-plugin)
- [ ] Plugin name: Relex Legal · work email · organisation: Relex · handle (lowercase): relex [CONFIRM WITH GEORGE]
- [ ] Website: https://relex.legal · Logo URL (1:1 PNG with background plate): https://raw.githubusercontent.com/relexlegal/relex-grok/main/plugin/assets/logo.png (check it has a solid background)
- [ ] Listing targets: Grok Bot, Grok (coming soon), Cursor — pick what applies
- [ ] MCP URL: https://relex.legal/api/mcp · public repo: https://github.com/relexlegal/relex-grok
- [ ] Description: "Relex legal practice management for Grok: work on your Relex matters over a PII-safe MCP connector."
- [ ] Testing instructions: sign in with the reviewer account, open the QA matter (synthetic data), run "List my Relex matters", "Summarise the documents in my demo matter", "Find a verified employment lawyer admitted in Germany".
- [ ] Tick "tested locally and in-app" only after actually testing; accept Publisher Terms.

## User must do — Grok Build marketplace PR (github.com/xai-org/plugin-marketplace)
Add one entry to `.grok-plugin/marketplace.json`, then run
`python3 scripts/generate-plugin-index.py` and `python3 scripts/validate-catalog.py --check`:
```json
{
  "name": "relex-legal",
  "description": "Relex legal practice management. Work on your Relex matters over a PII-safe MCP connector: redacted sources, drafts, matter ontology and verified legal professionals.",
  "category": "productivity",
  "source": {"source": "url", "url": "https://github.com/relexlegal/relex-grok.git", "path": "plugin", "sha": "<40-char commit sha on main after merge>"},
  "homepage": "https://relex.legal/docs/mcp",
  "keywords": ["relex", "relex legal", "legal practice management"],
  "domains": ["relex.legal"]
}
```

## Install (verify after push)
```
grok plugin install relexlegal/relex-grok#plugin --trust
grok plugin enable relex-legal
```
