# DropFlow AI — Voice-Powered Dropshipping Assistant & CRM

A voice AI agent (ElevenLabs) backed by an n8n automation system that answers dropshipping questions, captures and manages leads in real time, and sends personalized follow-up emails — all from a live spoken conversation.

## What it does
1. A user has a live voice conversation with the ElevenLabs conversational AI agent ("danrep")
2. ElevenLabs forwards the conversation context to an n8n webhook, where a backend AI Agent takes over the "thinking" — deciding intent, managing lead data, and executing actions
3. The agent answers general dropshipping questions directly (niches, supplier sourcing, starting budgets) using a Pinecone-backed knowledge base built from ingested documents
4. It naturally extracts lead information from the conversation as it's shared — name, email, niche, budget, business goal — without demanding a fixed form
5. It checks whether the person already exists as a lead (matched by email, then phone, then name) and either creates a new record or updates the existing one in a Google Sheets CRM, without ever overwriting good data with blanks
6. On request, it sends a personalized follow-up email tailored to that specific lead's niche, budget, and goals — and only marks the email as sent once the email tool confirms success

## Stack
- **ElevenLabs** — real-time conversational voice interface
- **n8n** — backend AI Agent orchestration, triggered via webhook
- **Google Drive + Pinecone** — document ingestion pipeline feeding a RAG knowledge base for dropshipping Q&A
- **Google Sheets** — lead/CRM database
- **OpenAI (GPT-5-mini)** — backend reasoning and email content generation
- **Gmail** — personalized outbound email delivery

## Design notes
The most deliberate part of this build is the backend agent's system prompt, which draws a hard line between two responsibilities: **ElevenLabs owns the conversation, the n8n agent owns the actions.** This keeps the voice experience natural while the backend focuses purely on correct, non-hallucinated data handling.

Specific behaviors engineered into the prompt:
- **Never invents information** — if a field wasn't stated by the user, it stays empty rather than being guessed
- **Duplicate-safe lead identification** — checks email, then phone, then name, in priority order, before deciding whether to create or update a record
- **Non-destructive updates** — new information is merged into an existing lead rather than overwriting or erasing prior data
- **Honesty about tool results** — explicitly forbidden from claiming an email was sent unless the email tool actually confirms it, and required to report partial failures accurately (e.g. "lead updated, but the email could not be sent")
- **Lead status logic** — a defined progression (New → Engaged → Qualified → Customer → Follow-Up) based on how much genuine buying intent the conversation reveals, not just contact

## Status
Fully functional and tested — verified through live voice conversations resulting in correct lead creation/updates and successful personalized email delivery.
