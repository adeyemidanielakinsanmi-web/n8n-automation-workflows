# Customer Support AI Agent — GLOW EMPIRE BOUTIQUE

A full AI Agent (not a single-turn chatbot) built in n8n for a fashion retail business — handles product discovery, order/pricing enquiries, lead capture, and end-to-end virtual meeting booking, with persistent memory across the conversation.

## What it does
A conversational AI Agent that acts as the front-line customer service and business-enquiry assistant for a boutique retail brand. It:
- Helps customers discover products (men's/women's wear, shoes, sneakers) by asking relevant questions — occasion, size, colour, budget, delivery location
- Handles pricing, availability, delivery, and return/exchange enquiries — without ever inventing information it doesn't have
- Captures leads (name, email, phone, intent) directly into a Google Sheet, updating the same record as the conversation progresses rather than creating duplicates
- Handles business enquiries (partnerships, wholesale, corporate supply) by booking real virtual meetings — checking calendar availability, offering only real open slots, and creating the Google Calendar event with the customer as an attendee, live in the chat session
- Enforces safety rules — never asks for card PINs, OTPs, BVNs, or other sensitive payment credentials

## Stack
- **n8n** — AI Agent orchestration (LangChain Agent node, not a single LLM call)
- **OpenAI (GPT-5-mini)** — underlying model
- **Simple Memory** — conversational context retained across the session
- **Google Sheets** — lead capture and CRM logging
- **Google Calendar** — real-time availability checking and meeting booking
- **Code Tool** — custom logic exposed to the agent as a callable tool

## Design notes
This build goes further than a standard Q&A chatbot — it's a genuine **AI Agent with tools**, meaning the model decides when to call the Google Sheets, Google Calendar, or Code Tool functions based on the conversation, rather than following a fixed linear flow.

Key behaviors deliberately engineered into the system prompt:
- **Never guesses** — explicitly instructed not to invent prices, stock availability, delivery timelines, or store details it hasn't been given
- **Full-loop meeting booking** — the agent is instructed to complete the entire booking (check date → check availability → offer real slots → create the event → confirm) within the same chat session, not just collect details and stop
- **Duplicate-safe lead capture** — updates existing lead records instead of creating new ones for returning customers
- **Safety-first** — hard rules against ever requesting sensitive payment information

## Status
Fully functional and tested — verified end-to-end including live lead capture into Google Sheets and live calendar event creation with confirmed booking details returned to the user.

*Google Sheet and calendar screenshots use test/demo data.*
