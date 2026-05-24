# Bank Statement Conversion Agent Skill

Public agent integration assets for [Bank Statement Conversion](https://bank-statement-conversion.com).

This repository publishes the `bank-statement-conversion` Agent Skill so AI coding agents and agent clients can safely integrate Bank Statement Conversion through MCP, OpenAPI, and the REST API.

The main Bank Statement Conversion application source remains in Azure DevOps. This GitHub repository is only for public agent-facing artifacts.

## Install

```bash
npx skills add MTTenterprise/Bank-statement-conversion --skill bank-statement-conversion
```

After installing, restart your agent client if it requires a restart to load new skills.

## What This Skill Does

The skill teaches agents how to:

- Choose between remote MCP, OpenAPI, and REST API integration paths.
- Use the public Bank Statement Conversion workflow.
- Configure `BSC_API_TOKEN` safely.
- Request only scoped token abilities.
- Confirm before uploading financial documents or preparing downloads.
- Avoid private or local-only endpoint assumptions.

## Repository Layout

```text
agent-skills/
  bank-statement-conversion/
    SKILL.md
    agents/
      openai.yaml
    references/
      public-workflow.md
```

## Public Endpoints

Use the public product and API URLs:

- Agent landing page: `https://bank-statement-conversion.com/agents`
- API documentation: `https://bank-statement-conversion.com/api`
- OpenAPI schema: `https://bank-statement-conversion.com/api/openapi.json`
- LLM index: `https://bank-statement-conversion.com/llms.txt`
- Expanded LLM context: `https://bank-statement-conversion.com/llms-full.txt`
- Remote MCP endpoint: `https://bank-statement-conversion.com/mcp/bank-statement-conversion`

## Agent Workflow

The public conversion workflow is:

1. Optionally call `GET /api/user-status` to check credits, page allowance, and subscription plan.
2. Upload files with `POST /api/upload`.
3. Poll `GET /api/conversion-status/{jobId}` until the completed response includes a `download_token`.
4. Download the converted file with `GET /api/download/{downloadToken}`.

Use `statement[]` for file uploads, include `document_type`, and choose a supported output `format`.

## Token Safety

For every agent integration:

- Create a dedicated dashboard API token.
- Store it as `BSC_API_TOKEN` or another environment variable.
- Do not paste tokens into prompts, source code, docs, screenshots, logs, or examples.
- Enable only the abilities the agent needs:
  - `account:status`
  - `conversion:upload`
  - `conversion:read`
  - `conversion:download`
- Confirm with the user before uploading bank statements, invoices, receipts, or other financial documents.
- Confirm before preparing download links.
- Revoke the token when agent access should stop.

## MCP

Use the remote MCP endpoint when your client supports remote MCP tools:

```text
https://bank-statement-conversion.com/mcp/bank-statement-conversion
```

Configure the dashboard token in the client environment as `BSC_API_TOKEN`.

Remote MCP clients should send file bytes through the client-supported upload payload and ask the user for confirmation before sending financial documents.

## OpenAPI And REST

Use OpenAPI for ChatGPT Actions, custom GPTs, and schema-aware agents:

```text
https://bank-statement-conversion.com/api/openapi.json
```

Use REST for custom backends, scheduled jobs, and workflow engines:

```text
https://bank-statement-conversion.com/api
```

## Supported Formats

Common input formats include PDF, JPG/JPEG, PNG, TIFF/TIF, CSV, XLS/XLSX, OFX, QBO, QFX, STA, MT940, MT940X, XML, and CAMT.053.

Common bank statement output values include `csv`, `excel`, `json`, `qb_online`, `qb_desktop`, `xero`, `ofx`, `ofx_legacy`, `qfx`, `mt940`, `camt053`, `bai2`, `sage`, `myob`, `datev`, and `csv_clean`.

## License

Copyright MTTenterprise Inc.
