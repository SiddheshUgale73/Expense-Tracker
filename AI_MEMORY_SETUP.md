# 🧠 AI Assistant Memory Architecture (LangChain + Vector DB)

This guide covers enhancing your assistant with long-term memory using LangChain and a Vector Database (Chroma/FAISS) within n8n.

## 1. Memory Architecture Overview
The assistant uses a **Retrieval-Augmented Generation (RAG)** approach for memory:
1.  **Storage (Upsert)**: Every message from the user is embedded (using OpenAI Embeddings) and stored in the Vector Database with a timestamp and session ID.
2.  **Retrieval**: When a new message arrives, the system searches the Vector DB for the most relevant past messages.
3.  **Augmentation**: The retrieved context is injected into the OpenAI prompt, allowing the AI to "remember" past interactions.
4.  **Generation**: OpenAI generates a response based on the current message + retrieved context.

## 2. Component Stack
- **AI Agent Node**: The core n8n node that orchestrates LangChain components.
- **Vector Store (Chroma/FAISS)**: Stores the high-dimensional embeddings of your conversations.
- **OpenAI Embeddings**: Converts text into numerical vectors for similarity search.
- **Memory Node**: n8n's "Window Buffer Memory" or "Motorhead" for short-term context.

## 3. n8n Node Configuration

### Vector Store (Chroma)
- **Operation**: `Retrieve` / `Upsert`
- **Chroma URL**: `http://chroma:8000` (if running in Docker)
- **Collection Name**: `assistant_memory`

### AI Agent (LangChain)
- **Agent Type**: `Conversational Retrieval Agent`
- **Memory**: `Window Buffer Memory` (for immediate context)
- **Tools**: The existing Gmail, Sheets, and Calendar tools.

## 4. Example Memory Queries

### Scenario 1: Recalling Past Preferences
**User**: "Remember I like my coffee black."
**Assistant**: "Got it! I've noted that you prefer your coffee black."
*(Later)*
**User**: "I'm at a cafe, what should I order?"
**Assistant**: "Based on our past conversation, you mentioned you like your coffee black. Should I find a black coffee option for you?"

### Scenario 2: Project Context
**User**: "The project we discussed yesterday is called 'Project Titan'."
*(Next Day)*
**User**: "Add a task for Titan."
**Assistant**: "I remember Project Titan from yesterday. I'll add that task to your list now."

## 5. Setup Instructions
1.  **Add Chroma to Docker**: Add a Chroma service to your `docker-compose.yml`.
2.  **Configure Embeddings**: Use the `OpenAI Embeddings` node in n8n.
3.  **Link to AI Agent**: Connect the Vector Store and Memory nodes to the inputs of the AI Agent node.
