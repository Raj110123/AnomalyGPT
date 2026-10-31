# AnomalyGPT

### A tool-using AI assistant for conversation, research, and your documents.

AnomalyGPT is a full-stack, browser-based AI chat application built around Google Gemini. It can answer questions, search the web for current information, find answers in your uploaded files, perform calculations, and save or recall conversation-specific memories—all from one chat interface.

<p align="center">
  <strong>Chat · Search · Read · Remember · Calculate</strong>
</p>

---

## What it can do

- **Stream conversations:** responses arrive progressively in the chat interface.
- **Choose a Gemini model:** select a supported model in the UI, including Gemini 2.5 Flash, 2.5 Pro, 2.5 Flash Lite, and Gemini 1.5 models.
- **Search the web:** Tavily Search helps answer questions about current events and other time-sensitive topics.
- **Ask questions about files:** upload PDF, DOCX, TXT, Markdown, Python, or CSV files and retrieve relevant passages with retrieval-augmented generation (RAG).
- **Use an AI toolset:** calculate expressions, search uploaded documents, and save or recall memories through agent tools.
- **Keep chat history:** revisit conversations and their messages in the sidebar.
- **Dictate messages:** use browser speech recognition where supported.
- **Persist agent state:** conversation data, long-term memory, vector indexes, and LangGraph checkpoints are stored locally.

## Tech stack

| Layer | Technologies |
|---|---|
| **Web app and API** | Python, FastAPI, Uvicorn |
| **Chat interface** | HTML, CSS, JavaScript, Jinja2 templates, Server-Sent Events (SSE) |
| **Language models** | Google Gemini through LangChain's Google GenAI integration |
| **Agent orchestration** | LangChain, LangGraph, tool calling, SQLite checkpointing |
| **Web search** | Tavily Search |
| **Document RAG** | ChromaDB, Google Generative AI embeddings, LangChain text splitters |
| **Document parsing** | pypdf, docx2txt, Python text/CSV/Markdown readers |
| **Relational persistence** | SQLite, SQLAlchemy |
| **Configuration and certificates** | python-dotenv, certifi |

## How it works

1. The browser sends a message and selected model to the FastAPI application.
2. The LangGraph agent sends the conversation to Gemini and routes tool calls as needed.
3. Tools can search Tavily, retrieve relevant uploaded-file chunks from Chroma, calculate expressions, or access saved memories.
4. The response streams back to the browser. Conversations and agent checkpoints are persisted in SQLite.

Uploaded documents are split into overlapping text chunks, embedded, and stored in Chroma. Retrieval is scoped to the current conversation thread so document searches return that thread's uploaded content.

## Get started

### Requirements

- Python 3.10 or newer
- A Google AI API key
- A Tavily API key for web search

### 1. Clone and enter the project

```bash
git clone https://github.com/Raj110123/AnomalyGPT.git
cd AnomalyGPT
```

### 2. Create and activate a virtual environment

**Windows PowerShell**

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install -r requirements.txt
```

### 4. Configure API keys

Create a `.env` file in the project root:

```dotenv
GOOGLE_API_KEY=your_google_ai_api_key
TAVILY_API_KEY=your_tavily_api_key

# Optional: default model used when no valid model is selected
GEMINI_MODEL=gemini-2.5-flash
```

Keep `.env` private; do not commit API keys.

### 5. Start AnomalyGPT

```bash
uvicorn app:app --reload --host 127.0.0.1 --port 8080
```

Open [http://127.0.0.1:8080](http://127.0.0.1:8080) in your browser.

## Supported uploads

| File type | Extension |
|---|---|
| PDF | `.pdf` |
| Word document | `.docx` |
| Plain text | `.txt` |
| Markdown | `.md` |
| Python source | `.py` |
| Comma-separated values | `.csv` |

## Project layout

```text
AnomalyGPT/
├── app.py              # FastAPI routes, uploads, chat streaming
├── agent.py            # Gemini agent and LangGraph workflow
├── tools.py            # Web search, calculator, RAG, and memory tools
├── rag.py              # File parsing, chunking, embeddings, retrieval
├── database.py         # SQLite models and persistence helpers
├── templates/
│   └── index.html      # Chat UI and browser-side behavior
├── requirements.txt    # Python dependencies
├── uploads/            # Uploaded source files (created at runtime)
├── chroma_db/          # Persistent document vector store (created at runtime)
└── data/               # SQLite databases and checkpoints (created at runtime)
```

## Configuration notes

- The application reads settings and API keys from environment variables, including `.env`.
- `GEMINI_MODEL` optionally sets the default model. The chat interface also lets you choose a model.
- Runtime files are stored in `uploads/`, `chroma_db/`, and `data/`; back these up if you need to preserve local conversations, memories, or indexes.
- Speech input depends on browser support for the Web Speech API.

## License

See [LICENSE](./LICENSE).
