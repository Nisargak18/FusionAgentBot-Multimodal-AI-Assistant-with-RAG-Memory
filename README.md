## 🤖 FusionAgentBot — Multimodal AI Assistant with RAG Memory

FusionAgentBot is a high-performance Telegram AI agent built to solve the "context window" problem. Unlike standard chatbots that lose context after a session, FusionAgentBot utilizes Retrieval-Augmented Generation (RAG) to maintain a persistent, long-term memory of every user interaction.

This project demonstrates the orchestration of multimodal data (Voice/Text/Images) and autonomous decision-making using an Agentic framework.

## 🛠️ The Technical "Why"🧠 
 
# Persistent Context via RAG
Standard LLMs are limited by their context window. FusionAgentBot solves this by:

1.Embedding: Every conversation is passed through an embedding model.
2.Vector Storage: These numerical representations are stored in Supabase (pgvector).
3.Semantic Search: When a user asks a question, the system performs a cosine similarity search to inject "relevant memories" into the prompt before the LLM generates a response.

# 🎙️ Multimodal Processing
The bot bridges the gap between different data formats:

1.Speech-to-Text: Handles .oga voice files from Telegram and transcribes them using Gemini STT model.
2.Dynamic Response: The agent can choose to output text or trigger a separate workflow to generate and send images via Nano banana 2.5flash model.

# 🔁 System Architecture & Logic
The intelligence of the bot is distributed across a specialized n8n workflow:

1. The Input Layer (Ingestion)
The system uses a Telegram Webhook to listen for updates. A conditional logic branch identifies the message type:

Text: Passed directly to the memory retrieval phase.
Voice: The binary file is downloaded, buffered, and sent to an STT (Speech-to-Text) engine.

2. The Retrieval Layer (Memory)Before the AI speaks, it "thinks" about the past. It queries the Supabase Vector Store.

Example: If you told the bot your name 3 weeks ago, the semantic search identifies that high-relevance data point and provides it to the LLM as "Context."

3. The Agentic Layer (Reasoning)This is the "Brain" of the operation. I used an AI Agent node that has access to defined 

Tools:Memory Tool: For looking up past facts.
Image Tool: For generating visuals.
Calculation Tool: For complex logic.The agent evaluates the prompt and determines if it needs to call a tool or reply immediately.

4. The Output & Storage Layer (Closing the Loop)
After the response is sent to Telegram, the bot performs a final background task:

It takes the User Input + Bot Response and creates a new vector record.This ensures the "Memory" is always up to date.

# Key Learning MilestonesAgent Orchestration: 
Learning how to prevent "Agent Loops" and ensuring the AI chooses the correct tool for the task.

1. Data Latency: Optimizing the STT-to-LLM pipeline to ensure responses feel "instant" to the user.
2. Vector Management: Implementing effective metadata filtering within Supabase to ensure retrieved memories are actually relevant.

# 🔧 Installation & SetupClone the Repo:

Bashgit clone https://github.com/Nisargak18/FusionAgentBot.git

n8n Configuration:Import FusionAgent_Workflow.json.Connect your Groq, Gemini and Telegram credentials.

Database Setup:Run the provided SQL script in your Supabase SQL Editor to initialize the documents table with vector support.Environment 

Variables:GROQ_API_KEY,SUPABASE_URL,TELEGRAM_BOT_TOKEN,GEMINI_API_KEY


## Outputs

1. Demo video
https://github.com/user-attachments/assets/822981fd-9be6-4b17-a533-cf23870efbf0

2. Main Workflow
<img width="1624" height="599" alt="Mainworkflow" src="https://github.com/user-attachments/assets/fb3f4a2c-2bae-46f3-9108-e47e3ccff185" />

3. Sub Workflow (generate image)
<img width="1292" height="471" alt="SubWorkflow(generate image)" src="https://github.com/user-attachments/assets/fb72852e-1747-4564-84f5-114ebc88467e" />





