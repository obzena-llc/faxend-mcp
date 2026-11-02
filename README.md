<p align="center"><img src="logo.png" width="96" alt="Faxend logo"></p>

# Faxend MCP server

Faxend sends real faxes from your phone, browser and now your AI assistant. This repository is the public documentation
for Faxend's hosted remote MCP server, which lets Claude, ChatGPT, Cursor and other MCP clients send a PDF to any fax
number, follow the delivery and check the pages left on your plan.

- **Server URL:** `https://faxend.com/mcp`
- **Transport:** Streamable HTTP (stateless, JSON responses)
- **Auth:** your Faxend API key (`Authorization: Bearer fxk_...`, or `?key=fxk_...` for connectors that only take a URL)
- **Full guide:** https://faxend.com/mcp/
- **MCP Registry name:** `com.faxend/faxend`

This repository holds documentation only. The server is hosted by Faxend; there is nothing to install or run locally.

## Tools

| Tool | What it does | Type |
| --- | --- | --- |
| `send_fax` | Sends a PDF (public https URL or base64) to one fax number, with an optional cover note page. Returns a `fax_id` | write |
| `get_fax_status` | Delivery status of one fax: sending, delivered or failed (with the reason and whether pages were refunded) | read-only |
| `list_recent_faxes` | Up to 20 recent faxes from your account (web, Chrome extension, AI assistants), recipients masked | read-only |
| `get_remaining_pages` | Pages left on your Faxend plans, per plan and in total | read-only |

Every tool has a `title` and `readOnlyHint` / `destructiveHint` annotations.

## Connect

1. Sign in at https://faxend.com and open **API keys** (https://faxend.com/api-keys/).
2. Create a key. It is shown once; copy it.
3. Add the server to your client.

Claude Code:

```bash
claude mcp add --transport http faxend https://faxend.com/mcp \
  --header "Authorization: Bearer YOUR_FAXEND_API_KEY"
```

Cursor and other clients:

```json
{
  "mcpServers": {
    "faxend": {
      "type": "http",
      "url": "https://faxend.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_FAXEND_API_KEY" }
    }
  }
}
```

claude.ai and ChatGPT custom connectors take only a URL: use `https://faxend.com/mcp?key=YOUR_FAXEND_API_KEY` and keep
that URL private. Step-by-step instructions for each client are on https://faxend.com/mcp/.

Keys can be revoked any time on the API keys page.

## Pricing

Each faxed page (and a cover page) uses one page from your Faxend plan. Failed faxes are refunded automatically.
Pages cannot be bought through the assistant; see https://faxend.com/pricing/.

## Limits

- PDF only, up to 20 MB and 100 pages per fax
- 120 requests per minute and 20 faxes per hour per key
- Delivery usually takes 1 to 5 minutes

## Example prompts

- "Fax this PDF to +1 212 555 0100: https://example.com/form.pdf"
- "Did my last fax go through?"
- "How many fax pages do I have left?"

## Privacy and terms

- Privacy policy: https://faxend.com/privacy-policy/
- Terms of use: https://faxend.com/terms-of-use/
- Support: https://faxend.com/support/

## License

Documentation in this repository is released under the MIT License. The Faxend service is proprietary.
