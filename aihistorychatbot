# AI World History Chatbot

A conversational AI assistant built in n8n that answers world history questions — built to demonstrate AI agent persona design, not just Q&A functionality.

## What it does
A chat-based assistant ("Daniel") that answers questions across world history — ancient civilizations, empires, wars, revolutions, historical figures, and more — with accurate, well-structured, and appropriately neutral responses.

Rather than just wiring a model to answer questions, this build focuses on **designing the assistant's behavior**: how it handles uncertainty, how it stays neutral on contested historical topics, how it structures simple vs. complex answers, and how it maintains a consistent, approachable personality throughout.

## Stack
- **n8n** — workflow orchestration (Chat Trigger + LangChain OpenAI node)
- **OpenAI (GPT-5-mini)** — underlying model
- Deployed as an embeddable public chat widget via n8n's native chat trigger

## Design notes
The system prompt was deliberately engineered, not just a one-line instruction. It defines:
- **Personality** — friendly, patient, neutral on controversial topics, avoids unexplained jargon
- **Scope** — a clearly defined set of historical subject areas the assistant is confident answering
- **Answering structure** — chronological framing, clear distinction between established fact vs. scholarly interpretation vs. disputed claims
- **Uncertainty handling** — explicit instructions to say "historians generally agree..." or "the evidence suggests..." rather than fabricating dates, sources, or events
- **Historical neutrality** — guidance on discussing sensitive topics (slavery, colonialism, war, religion) respectfully and without glorifying atrocities or imposing modern judgment without context
- **Conversation style** — concise answers for simple questions, structured answers (Overview / Background / Key Events / Causes / Consequences / Why It Matters) for complex ones

This is the same persona-design approach discussed in "How I design an AI assistant's personality to match a brand, not just answer questions" — applied here to an educational assistant rather than a business brand voice.

## Status
Fully functional and deployed as a public chat widget — tested live with multiple historical queries.
