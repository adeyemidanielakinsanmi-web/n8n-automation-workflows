# E-commerce Support Agent: RAG + n8n + Telegram

An AI customer support agent for an online store, built in n8n. Customers chat with it on Telegram to get answers from the store's own documents, place orders, check order status, book showroom appointments, and escalate to a human when the bot shouldn't guess.

> Built as a practice project for a fictional client, **Verdant Homegoods** (a sustainable home and kitchen store in Lagos, Nigeria). All business data, names and contact details in this repo are made up.

![Workflow canvas](docs/workflow.png)

## What it does

- **Answers from the knowledge base (RAG):** shipping rates, returns policy, FAQs and product details are retrieved from a Pinecone vector index instead of being guessed by the model.
- **Creates orders:** collects all required details in conversation, confirms the summary with the customer, then writes a new row to a Google Sheet with an Order ID in the format `VHG-####`.
- **Looks up orders:** fetches an order by Order ID.
- **Books showroom appointments:** logs requests to a Google Sheet.
- **Escalates safely:** for wholesale, custom orders, discounts, complaints and out-of-policy refunds, it logs the case to a sheet and sends an email to the business owner.
- **Self-updating knowledge:** adding or changing a file in a Google Drive folder re-indexes it automatically, so the owner never needs a developer to update policies.

![Escalation email](docs/escalation-email.png)

## Architecture

**Flow 1: Knowledge ingestion**

Google Drive Trigger (file created in folder) -> Download file -> Pinecone Vector Store (insert) with OpenAI Embeddings and a Default Data Loader for chunking.

**Flow 2: Customer chat**

Telegram Trigger -> Edit Fields -> AI Agent -> Send a text message (Telegram)

The AI Agent is connected to:

| Component | Purpose |
|---|---|
| OpenAI Chat Model | Language model for the agent |
| Simple Memory | Keeps conversation context within a chat |
| `knowledge_base_lookup1` | Pinecone retrieval tool (OpenAI Embeddings) |
| `Create_Order` | Google Sheets: append a row to the order sheet |
| `Get_Order` | Google Sheets: read an order by Order ID |
| `Appointments_sheets` | Google Sheets: append a showroom appointment |
| `Escalation_log` | Google Sheets: log escalation cases |
| `escalation_tool` | Gmail: email the business owner |

## Tech stack

n8n, OpenAI (chat model and embeddings), Pinecone, Telegram Bot API (BotFather), Google Drive, Google Sheets, Gmail.

## Google Sheets structure

**Orders:** Order ID, Date/Time, Customer Name, Telegram Username, Phone Number, Delivery Address, Products Ordered (SKU + qty), Order Total, Payment Method, Payment Status, Shipping Zone, Gift Wrap

**Appointments:** Requested Date/Time, Customer Name, Telegram Username, Phone Number, Status, Notes

**Escalations:** Date/Time, Telegram Username, Customer Question, Reason, Status

## Setup

1. Import `workflow.json` into n8n.
2. Create credentials for OpenAI, Pinecone, Telegram (token from BotFather), Google Drive, Google Sheets and Gmail, and attach them to the nodes.
3. Create a Pinecone index and select it in both the ingestion and `knowledge_base_lookup1` nodes. Both must use the same embedding model.
4. Create the Google Sheets above and point each sheet node at the right tab.
5. Create a Google Drive folder, set it as the Drive Trigger target, and drop in the knowledge base documents (see `knowledge-base/`).
6. Paste the agent system message into the AI Agent node (role, rules, tool usage, boundaries, knowledge summary).
7. Activate the workflow and message your bot on Telegram.

## Testing

Tested with a scripted set of conversations covering:

1. Knowledge retrieval (delivery times, COD rules, dishwasher-safe checks)
2. Hallucination traps (items not in the catalog, unsupported countries, off-topic questions)
3. Full order creation with total and shipping calculation
4. Order lookup, including an Order ID that doesn't exist
5. Appointment booking with business-day rules
6. Escalation (wholesale requests, discount requests)

## Known limitations and next steps

- Order IDs should be generated and de-duplicated in a Code node rather than by the LLM.
- Timestamps should be injected from n8n (`$now`) rather than produced by the model.
- Payment status should be confirmed by a payment webhook (e.g. Paystack), not typed in chat.
- A Supabase (pgvector) version of the retrieval layer is planned for comparison.

## Author

Adeyemi Daniel, Automation Engineer. (https://www.linkedin.com/in/adeyemi-daniel-akinsanmi-a69a873a5/)
