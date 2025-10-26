# FINBOT — Financial News & Document Research Chatbot (LangChain + OpenAI + FAISS + Streamlit)

**FINBOT** is a production-ready **RAG (Retrieval-Augmented Generation)** chatbot for **financial analysts** who need **near-real-time research** across **news articles, company prospectuses, earnings coverage, and market commentary**. Paste a list of **news URLs** (e.g., Moneycontrol, exchange notices, business news sites). FINBOT will **scrape → chunk → embed → index (FAISS)** and then answer your **natural-language questions** with **grounded citations/sources** via **LangChain** and **OpenAI**.

---

## ✨ Features

- **Multi-URL ingest** from the sidebar; works with most mainstream financial news/article pages.  
- **Automated web scraping** using robust URL loaders to extract article content.  
- **Smart text chunking** (`RecursiveCharacterTextSplitter`) with tuned separators and `chunk_size=1000` for high-quality retrieval.  
- **Embeddings + Vector Store** via `OpenAIEmbeddings` + **FAISS** for fast, local semantic search.  
- **Retrieval QA with sources** using `RetrievalQAWithSourcesChain` (answers include supporting links).  
- **Streamlit UI**: clean, fast, and deployable (`streamlit run`).  
- **Persisted index** (e.g., `faiss_store_openai.pkl`) for repeat queries without re-processing.

---

## 🧱 Tech Stack

- **Framework**: LangChain (chains, loaders, splitting, retriever)  
- **LLM**: OpenAI (temperature-controlled, `max_tokens` configurable)  
- **Embeddings**: OpenAI Embeddings (semantic retrieval)  
- **Vector DB**: FAISS (in-memory + pickled persistence)  
- **UI**: Streamlit (sidebar URL inputs, query box, sourced answer view)  
- **Parsing**: `unstructured` (or equivalent) for URL content extraction  
- **Env**: `python-dotenv` for local `.env` management

---

## 📂 Project Structure

```
.
├── main.py                   # Streamlit app (ingest, index, RAG chat)            <- run this
├── requirements.txt          # Pinned dependencies
├── env.txt                   # Example env file (OPENAI_API_KEY placeholder)
└── DBS.pkl    # (generated) persisted FAISS index after processing
```

> The Streamlit page title and UI text should reference **FINBOT** (e.g., “FINBOT: News Research Tool 📈”). If your current `main.py` still shows an older name, replace it with **FINBOT**.

---

## 🚀 Quickstart

### 1) Clone & Environment

```bash
git clone <your-repo-url>
cd <your-repo-folder>
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate
```

### 2) Install Dependencies

```bash
pip install -r requirements.txt
```

> Typical pins (example):  
> `langchain`, `streamlit`, `unstructured`, `faiss-cpu`, `python-dotenv`, `openai`, `tiktoken`, `python-magic` / `python-magic-bin`.

### 3) Configure API Key

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=sk-***********************
```

You can use `env.txt` as a reference for the variable name.

### 4) Run FINBOT

```bash
streamlit run main.py
```

Open the local URL shown in your terminal.

---

## 🧠 How FINBOT Works (RAG Pipeline)

1. **Input URLs**: Add up to N article links in the Streamlit **sidebar** and click **“Process URLs.”**  
2. **Web Scrape**: A URL loader (e.g., `UnstructuredURLLoader`) fetches HTML and extracts raw text.  
3. **Chunking**: `RecursiveCharacterTextSplitter` splits the corpus (`chunk_size ~ 1000`) for optimal retrieval granularity.  
4. **Embeddings + Index**: `OpenAIEmbeddings` encodes chunks → **FAISS** builds a vector index and persists it (e.g., `faiss_store_openai.pkl`).  
5. **Ask a Question**: Type your research question in **“Question:”** — FINBOT loads the FAISS index and initializes a **`RetrievalQAWithSourcesChain`**.  
6. **Answer + Sources**: FINBOT returns a concise, cited answer and lists **source URLs** used to ground the response.

---

## 🖥️ Usage Examples (Financial Research)

- “Summarize today’s key updates on *[Company]* from the provided links. Include risk factors and management commentary.”  
- “What does the latest prospectus say about debt, cash flows, and capex guidance?”  
- “Compare earnings highlights from these two news posts and list revenue/EBITDA deltas.”  
- “List regulatory approvals or compliance issues mentioned across the pages.”  

> Answers cite the exact pages ingested during this session, keeping research **auditable** and **compliance-friendly**.

---

## 🔐 Security & Compliance Notes

- **Local indexing**: FAISS runs locally; only embeddings/LLM calls go to OpenAI.  
- **Private research**: Use self-hosted deployment and restrict access for internal teams.  
- **Attribution**: The UI displays **sources** for every answer to support due diligence.

---

## ⚙️ Configuration & Tuning

- **LLM controls**: e.g., `temperature=0.0–0.9`, `max_tokens` as needed.  
- **Chunking**: tune `chunk_size` and separators to match article styles (shorter chunks → better recall; longer chunks → more context).  
- **Persistence**: change the FAISS index path/name if you prefer a different location (default often `faiss_store_openai.pkl`).  
- **Batching**: for large link lists, consider batching ingestion to manage rate limits/cost.

---

## 🧩 Requirements

Install all key libraries via:

```bash
pip install -r requirements.txt
```

- LangChain, Streamlit, Unstructured, FAISS, OpenAI SDK, `tiktoken`, `python-dotenv`, and `python-magic` (`python-magic-bin` on Windows) are commonly included.

---

## 🛠️ Troubleshooting

- **No answer / empty sources**: Ensure you **clicked “Process URLs”** after adding links; verify the content isn’t paywalled or JS-heavy (server-side rendered pages work best).  
- **Index not found**: Build it once by processing URLs; a `.pkl` FAISS store should appear.  
- **OpenAI auth error**: Confirm `.env` contains a valid `OPENAI_API_KEY` and that your key has credits.  
- **Loader errors**: Some sites block bots; try alternative sources or pre-cleaned copies.

---

## 🗺️ Roadmap

- 🔄 **Automated refresh** of tracked sources (scheduled re-ingest)  
- 🧾 **PDF/Prospectus ingestion** (e.g., filings, exchange disclosures) with PDF loaders  
- 🧠 **Re-rankers** or hybrid search for higher-precision retrieval  
- 🧮 **Metadata filters** (ticker, source, date) + **multi-index** switching  
- 🏷️ **Entity extraction** (tickers, executives, segments) for analyst notes  
- 🧰 **Dockerfile + CI** for reproducible deployments

---


## 🙏 Acknowledgments

Built with **LangChain**, **FAISS**, **OpenAI**, **Streamlit**, **Unstructured**, and **python-dotenv**.

---

## 🧪 Local Dev Commands

```bash
# create venv
python -m venv .venv
source .venv/bin/activate   # or .venv\Scripts\activate on Windows

# install deps
pip install -r requirements.txt

# run FINBOT
streamlit run main.py
```

---

## 🔑 Environment

```env
# .env
OPENAI_API_KEY=sk-********************************
```

