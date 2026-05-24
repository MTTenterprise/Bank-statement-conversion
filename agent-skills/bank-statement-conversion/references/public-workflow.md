# Public Workflow Reference

## Discovery URLs

- Agent landing page: `https://bank-statement-conversion.com/agents`
- API documentation: `https://bank-statement-conversion.com/api`
- OpenAPI schema: `https://bank-statement-conversion.com/api/openapi.json`
- LLM index: `https://bank-statement-conversion.com/llms.txt`
- Expanded LLM context: `https://bank-statement-conversion.com/llms-full.txt`
- Remote MCP endpoint: `https://bank-statement-conversion.com/mcp/bank-statement-conversion`

## REST Flow

1. `GET /api/user-status`
   - Optional preflight for credits, page allowance, and subscription plan.
2. `POST /api/upload`
   - Required multipart upload endpoint.
   - Send files as `statement[]`.
   - Include `document_type` and `format`.
3. `GET /api/conversion-status/{jobId}`
   - Poll until conversion is complete.
   - Read `download_token` from completed previews.
4. `GET /api/download/{downloadToken}`
   - Download the converted binary file.

## MCP Flow

Use the remote MCP endpoint with a dashboard token configured in the client environment as `BSC_API_TOKEN`.

Required token abilities map to live tools:

- `account:status`
- `conversion:upload`
- `conversion:read`
- `conversion:download`

Remote MCP uploads should pass file bytes through the client-supported upload payload. Confirm with the user before sending financial documents.

## Safety Checklist

- Use a dedicated token for the agent.
- Keep the token out of prompts and committed files.
- Confirm uploads and downloads.
- Use only documented public workflow endpoints.
- Revoke access when the integration is no longer needed.
