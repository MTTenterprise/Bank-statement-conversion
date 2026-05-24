---
name: bank-statement-conversion
description: "Use when integrating Bank Statement Conversion with AI agents, MCP clients, ChatGPT Actions, Codex, Cursor, Claude, REST API workflows, or automation that uploads financial documents and downloads converted statement outputs."
---

# Bank Statement Conversion

Use this skill when a user wants an AI agent or application to convert bank statements, invoices, receipts, or structured accounting files with Bank Statement Conversion.

## Choose the Integration Path

- Prefer remote MCP when the agent client supports MCP tools. Use `https://bank-statement-conversion.com/mcp/bank-statement-conversion`.
- Use OpenAPI when building ChatGPT Actions, custom GPTs, or schema-aware agent actions. Use `https://bank-statement-conversion.com/api/openapi.json`.
- Use REST API for backend services, custom agents, scheduled jobs, or workflow engines.
- Use the LLM context files when the agent needs canonical product and API context: `https://bank-statement-conversion.com/llms.txt` and `https://bank-statement-conversion.com/llms-full.txt`.

## Token Safety

- Use a dedicated dashboard-generated API token for every agent integration.
- Store the token as `BSC_API_TOKEN` or another environment variable. Do not paste tokens into prompts, source code, docs, screenshots, logs, or examples.
- Enable only the abilities the agent needs:
  - `account:status`
  - `conversion:upload`
  - `conversion:read`
  - `conversion:download`
- Ask for explicit user confirmation before uploading financial documents or preparing download links.
- Revoke the token from the dashboard when agent access should stop.

## Public Workflow

The public conversion workflow is:

1. Optionally call `GET /api/user-status` to check credits, page allowance, and subscription plan.
2. Upload files with `POST /api/upload`.
3. Poll `GET /api/conversion-status/{jobId}` until the completed response includes a `download_token`.
4. Download the converted file with `GET /api/download/{downloadToken}`.

Use `statement[]` for one or more files. Use `document_type` values such as `bank_statement`, `invoice`, or `receipt`. Use a supported `format` value for the selected document type.

## Supported Formats

Common input formats include PDF, JPG/JPEG, PNG, TIFF/TIF, CSV, XLS/XLSX, OFX, QBO, QFX, STA, MT940, MT940X, XML, and CAMT.053.

Common bank statement output values include `csv`, `excel`, `json`, `qb_online`, `qb_desktop`, `xero`, `ofx`, `ofx_legacy`, `qfx`, `mt940`, `camt053`, `bai2`, `sage`, `myob`, `datev`, and `csv_clean`.

## Implementation Rules

- Keep endpoint examples limited to the public workflow above.
- Do not construct download tokens. Use only the `download_token` returned by status responses.
- Poll status with a reasonable delay instead of hammering the API.
- Use multipart uploads for REST API file submission.
- Preserve binary responses when downloading converted files.
- Surface capacity, validation, and authentication errors clearly to the user.
- Do not make claims about local filesystem access for remote MCP clients.

For more detail, read `references/public-workflow.md` in this skill.
