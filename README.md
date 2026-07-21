## Overview
This n8n workflow implements a production-ready, streaming-capable AI chat agent powered by Alibaba Cloud's Qwen large language model. It features built-in conversation memory, a clean web-based chat interface, and a friendly, context-aware system prompt — all without writing code.

The workflow is designed for easy deployment: once imported and configured, it exposes a public webhook URL that anyone can use as a lightweight AI assistant interface.

## How It Works
1. A user sends a message via the embedded web chat UI (triggered by the Chat Trigger node).
2. The message flows into the AI Agent node, which orchestrates the LLM interaction.
3. The AI Agent retrieves the latest 10 turns of conversation history from the Conversation Memory node (Buffer Window memory).
4. The AI Agent passes both the user input and contextual history to the Qwen Cloud Chat Model node (Alibaba Cloud’s Qwen API).
5. The model generates a response, streamed back in real time to the chat UI.
6. The new exchange is automatically appended to memory for future context.

## Nodes & Tools Used
- `Chat Trigger` (@n8n/n8n-nodes-langchain): Provides the web-based chat interface with streaming support.
- `AI Agent` (@n8n/n8n-nodes-langchain): Orchestrates tool use, memory integration, and LLM routing.
- `Conversation Memory` (@n8n/n8n-nodes-langchain.memoryBufferWindow): Maintains a rolling 10-turn buffer of chat history for context continuity.
- `Qwen Cloud Chat Model` (@n8n/n8n-nodes-langchain.lmChatAlibabaCloud): Connects to Alibaba Cloud’s Qwen API for inference.
- `Sticky Note` (n8n-nodes-base.stickyNote): Embedded documentation for quick setup reference.

## Prerequisites
- An active n8n instance (v1.50.0 or newer recommended for LangChain node compatibility).
- An Alibaba Cloud account with access to the DashScope (Qwen) API.
- A valid Alibaba Cloud API key (to be added as an `alibabaCloudApi` credential in n8n).
- Basic familiarity with n8n’s credential management and workflow import process.

## Setup & Usage
1. Import this workflow JSON into your n8n instance (via Workflow → Import → From Clipboard or File).
2. Navigate to Credentials → Create New Credential → `Alibaba Cloud API`.
3. Paste your DashScope API key and save; then assign it to the `Qwen Cloud Chat Model` node.
4. Ensure the `Chat Trigger` node’s `public` parameter is set to `true` (it is by default).
5. Activate the workflow. Copy the public webhook URL shown in the Chat Trigger node’s settings.
6. Share that URL — users can begin chatting immediately in their browser with zero setup.
7. Optional: Customize the system message in the `AI Agent` node or adjust `contextWindowLength` in `Conversation Memory` to tune behavior.

## Use Cases
- Internal teams needing a secure, on-prem or self-hosted AI assistant for FAQs, onboarding, or documentation lookup.
- Customer support teams embedding a lightweight, branded chatbot on internal dashboards or help portals.
- Developers prototyping LLM-powered interfaces without frontend engineering overhead.
- Educators or researchers deploying domain-specific assistants (e.g., coding tutor, policy explainer) using Qwen’s multilingual strengths.