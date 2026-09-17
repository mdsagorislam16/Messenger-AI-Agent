# 🤖 Messenger AI Agent — n8n Automation Workflow

An end-to-end **Facebook Messenger AI Agent** built entirely with [n8n](https://n8n.io/), combining **Retrieval-Augmented Generation (RAG)**, **image understanding**, and **multi-agent orchestration** to power a smart, context-aware chatbot — no traditional backend code required.

> Built as a no-code/low-code automation to demonstrate how far you can push n8n for production-style conversational AI.

---

## 🧠 What It Does

This workflow listens to Facebook Messenger events via webhook and intelligently routes each incoming message through the right AI pipeline:

- **Text queries** → answered using a **RAG Agent** grounded in a custom knowledge base (Pinecone vector store)
- **Images** → analyzed, summarized, and interpreted using a chain of AI agents
- **Replies** → sent back to the user on Messenger automatically via the Graph API

The result is a single Messenger bot that can answer knowledge-based questions *and* understand images sent by users — all orchestrated visually in n8n.

---

## 🗺️ Workflow Architecture

```
Webhook (Messenger entry point)
   ├── IF (verifies webhook / event type) → Respond to Webhook
   └── Switch (routes by message type)
        ├── Text → RAG Agent
        │            ├── OpenAI Chat Model (reasoning)
        │            ├── Pinecone Vector Store (knowledge retrieval)
        │            │        └── Embeddings OpenAI (vectorization)
        │            └── HTTP Request2 → send reply to Messenger
        │
        └── Image/Media → HTTP Request (fetch attachment)
                     → Analyze an Image
                     → Summarizer Agent (Google Gemini)
                     → Question Agent (Google Gemini)
                     → HTTP Request1 → send reply to Messenger
```

### Core Nodes

| Node | Purpose |
|---|---|
| **Webhook** | Entry point — receives Facebook Messenger events |
| **IF** | Handles Messenger's webhook verification / event filtering |
| **Respond to Webhook** | Responds to Meta's verification handshake |
| **Switch** | Routes the incoming payload based on message type (text vs. image) |
| **RAG Agent** | AI agent that answers text queries using retrieved context |
| **OpenAI Chat Model** | LLM used as the reasoning model for the RAG Agent |
| **Pinecone Vector Store** | Stores & retrieves embedded knowledge-base chunks |
| **Embeddings OpenAI** | Converts text into vector embeddings for Pinecone |
| **HTTP Request** | Fetches image/attachment data from Messenger |
| **Analyze an Image** | Extracts context/description from the image |
| **Summarizer Agent** | Summarizes image context using **Google Gemini** |
| **Question Agent** | Generates a relevant, conversational answer using **Google Gemini** |
| **HTTP Request1 / HTTP Request2** | Sends the final AI-generated reply back to the user via the Messenger Graph API |

---

## ⚙️ Tech Stack

- **[n8n](https://n8n.io/)** — workflow automation & orchestration engine
- **Facebook Messenger Platform (Graph API + Webhooks)** — messaging channel
- **OpenAI** — chat completion + embeddings (RAG pipeline)
- **Google Gemini** — image summarization & question-answering agents
- **Pinecone** — vector database for semantic search / RAG

---

## 🚀 Setup

1. **Clone this repo** and import `workflow.json` into your n8n instance (`Workflows → Import from File`).
2. **Create credentials** in n8n for:
   - Facebook Graph API (Page Access Token)
   - OpenAI API Key
   - Google Gemini (PaLM/Generative Language) API Key
   - Pinecone API Key + Environment
3. **Set up the Messenger webhook** in the [Meta Developer Console](https://developers.facebook.com/), pointing it to your n8n Webhook node's production URL, and use the same **Verify Token** configured in the `IF` node.
4. **Upload your knowledge base** into Pinecone (via the Embeddings OpenAI node or a separate ingestion workflow) so the RAG Agent has context to retrieve from.
5. **Activate the workflow** and start messaging your Facebook Page — the bot will respond automatically.

---

## 📌 Notes

- This project was originally built and tested during n8n's trial period; the workflow JSON is preserved here for reference and reuse.
- Model providers are swappable — the flow currently mixes **OpenAI** (for RAG/embeddings) and **Google Gemini** (for image summarization/Q&A), but either branch can be pointed to a single provider if preferred.
- Ideal as a starting template for building **Messenger-based support bots, FAQ assistants, or document-grounded chat agents** without writing a traditional backend.

---

## 📄 License

MIT — feel free to fork, modify, and build on top of this workflow.

---

## 🙋‍♂️ Author

Built with ❤️ using n8n. If you found this useful, a ⭐ on the repo is appreciated!
