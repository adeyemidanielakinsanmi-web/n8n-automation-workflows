# RAG Pipeline with Pinecone — Global Dropshipping FAQ Knowledge Base

A retrieval-augmented generation (RAG) system built in n8n that turns a static FAQ knowledge base into a conversational AI agent — answering dropshipping questions grounded in real source documents, with memory across the conversation.

## What it does
This build is actually two connected workflows:

**1. Knowledge base ingestion pipeline**
- Watches a Google Drive folder for new/updated FAQ documents
- Automatically downloads each file, chunks it, generates embeddings (OpenAI), and stores it in a Pinecone vector index — keeping the knowledge base current without manual re-uploading

**2. The chat agent**
- A conversational AI Agent that receives a user's question, retrieves the most relevant chunks from the Pinecone vector store, and answers using that retrieved context rather than the model's general knowledge alone
- Maintains conversation memory, so follow-up questions stay contextual instead of resetting each turn
- Deployed as a live, embeddable chat widget

## Stack
- **n8n** — pipeline orchestration (LangChain nodes)
- **Google Drive** — source document storage, watched via trigger for auto-ingestion
- **OpenAI Embeddings** — vectorizing both source documents and incoming queries
- **Pinecone** — vector database for semantic search/retrieval
- **OpenAI (GPT-5-mini)** — response generation grounded in retrieved context
- **Simple Memory** — session-level conversational context

## Design notes
The key architectural decision here is separating **ingestion** from **retrieval** into two distinct flows sharing the same Pinecone index. This means the knowledge base can be updated (new FAQ documents dropped into Drive) without touching or redeploying the chat agent itself — the agent always queries the current state of the index.

This is a genuine RAG pattern, not a static prompt stuffed with reference text: the agent only pulls in the specific chunks relevant to each individual question, which keeps answers accurate and scoped to the actual source material rather than the model guessing from general training knowledge.

## Status
Fully functional and deployed — knowledge base auto-updates from Google Drive, and the chat agent retrieves and answers from it live, tested with real dropshipping FAQ queries.
