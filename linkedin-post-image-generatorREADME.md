# LinkedIn Post & Image Generator Agent (AI Industry)

A two-agent AI pipeline in n8n that turns a topic + target audience into a fully researched LinkedIn post *and* a matching AI-generated marketing graphic, delivered straight to inbox.

## What it does
1. User submits a topic, target audience, and email via a form
2. **Content Generator Agent** searches the web in real time (Tavily) for a current, relevant article on the topic, then writes an original, audience-tailored LinkedIn post — hook, insight, light source attribution, hashtags, and a call to action
3. **Image Generator Agent** reads that finished post, extracts its core idea, and writes a detailed visual/design brief translating the message into a marketing-style graphic concept (not a literal scene — clean, modern, brand-appropriate design language)
4. That image prompt is sent to an AI image generation endpoint to produce the actual graphic
5. The finished LinkedIn post and its matching image are emailed back to the user as a ready-to-post package

## Stack
- **n8n** — multi-agent orchestration (two chained LangChain Agent nodes)
- **Tavily API** — real-time web research, exposed to the agent as a callable tool
- **OpenAI (GPT-5-mini)** — both the writing and the image-prompt agent
- **Pollinations AI** — text-to-image generation from the derived prompt
- **Gmail** — final delivery with the image as an attachment

## Design notes
This is a **two-agent chain**, not one model doing double duty — each agent has a distinct, narrowly scoped job:
- The **Content Agent** is instructed to research before writing, stay audience-specific, and never reveal that web search or automation was involved in producing the post
- The **Image Agent** is deliberately restricted to *design thinking*, not storytelling — it's instructed to avoid literal or photorealistic scenes and instead produce abstract, brand-style marketing visual briefs (icons, gradients, data visuals, typography elements), which is what actually reads as "professional LinkedIn content" rather than generic AI art

Chaining agents this way — one output becoming the next agent's entire input — is a different pattern from a single-agent build, and it's what makes the final output (text + matching visual) coherent rather than two disconnected pieces.

## Status
Fully functional end-to-end — tested live from form submission through to receiving the finished post and generated image via email.
