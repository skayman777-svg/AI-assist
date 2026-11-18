# n8n AI Support Assistant

An n8n workflow that answers customer questions: it receives a message via Webhook, searches a knowledge base,
asks an LLM (any OpenAI-compatible API) to write the answer, checks confidence, and either replies on its own or
hands the ticket to a manager in Telegram. Every request is logged to Google Sheets.

**Live demo:** https://skayman777-svg.github.io/AI-assist/ (interactive diagram + chat sandbox that uses the same
logic as the workflow Code nodes)

**Workflow file:** `n8n-support-assistant.json` — import it in n8n via Workflows → Import from File.

## Nodes

Webhook → Settings & request → Knowledge base search → LLM → Confidence check → IF (confidence ≥ threshold)
→ Reply to customer / Notify manager (Telegram) + Reply: handed to manager → Log row → Request log (Google Sheets).
12 standard nodes, no community nodes.

## Setup

1. Edit `CONFIG` in the “Settings & request” node: API URL, model, confidence threshold, manager chat.
2. Create credentials: Header Auth for the LLM (`Authorization: Bearer KEY`), a Telegram bot, Google Sheets.
3. Create a “Log” sheet with the columns from the “Log row” node.
4. Replace the sample articles in “Knowledge base search” with your own, or with a query to Notion / a vector DB.

Endpoint: `POST /webhook/support-assistant` with `{"message": "...", "session": "...", "channel": "site"}`.

## Notes

- The keys in the JSON are `REPLACE` placeholders. The workflow is a template: Code node logic is tested on sample
  questions, but it has not been run with real keys.
- Knowledge base search is simplified to keyword matching; a large knowledge base needs vector search.
- The knowledge base belongs to a fictional cookware store.
