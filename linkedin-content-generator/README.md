# LinkedIn & Twitter Automated Content Generator Agent

An AI Agent built in n8n that researches trending news on a given topic and writes original, voice-consistent LinkedIn and Twitter(X) posts from it — part of the content pipeline behind my own build-in-public posting.

## What it does
1. Pulls the next queued topic from a Google Sheet (status: "To Do")
2. Searches the web for current, trending articles on that topic via the Tavily Search API
3. Feeds three real source excerpts into an AI Agent that synthesizes them into one original LinkedIn post and one original Twitter(X) post — grounded in the sources, not copied from them
4. Writes the generated content back into the same Google Sheet row and updates its status to "Created"
5. Sends an email notification confirming the content is ready

## Stack
- **n8n** — AI Agent orchestration
- **Google Sheets** — content queue and output storage
- **Tavily API** — real-time web search for trending source material
- **OpenAI (GPT-5-mini)** — synthesis and writing
- **Gmail** — completion notification

## Design notes
The core of this build isn't the plumbing — it's the system prompt. Rather than a generic "write a LinkedIn post about X" instruction, the agent runs on a detailed writing brief that enforces:
- **Grounding** — only uses facts present in the three retrieved source excerpts; explicitly forbidden from inventing statistics, quotes, or timelines
- **Synthesis over summary** — required to combine ideas from all three sources into one original argument, not stitch together three summaries
- **A defined voice** — British English, a specific literary tone reference, no American spelling, a banned-word list, no em dashes
- **Platform-specific output** — a longer, more textured LinkedIn version (160–260 words) and a tighter Twitter(X) version (45–55 words), deliberately non-identical
- **Structural requirements** — a hook in the first two sentences, one derived insight not lifted from any single source, and a specific, non-generic call to action

This mirrors the same personality/brand-voice design principle from the AI World History Chatbot project — applied here to a content-writing persona instead of a support persona.

## Status
Fully functional and in active use — running as part of my own daily content pipeline, generating both LinkedIn and Twitter(X) drafts per topic automatically.
