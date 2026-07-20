# RAG Chatbot API

A context-only Retrieval-Augmented Generation (RAG) chatbot built with FastAPI. It allows users to upload documents, which are ingested into a hybrid retrieval pipeline (BM25 + dense + cross-encoder reranking + HyDE). When a user asks a question, the system retrieves relevant chunks and uses a local LLM to generate grounded responses based solely on those chunks.

---

## Features

- **Multi-format ingestion**: Supports `.pdf`, `.docx`, `.txt`, `.md`
- **Hybrid retrieval**: Combines BM25, dense embeddings, HyDE generation, and RRF fusion
- **Context-only LLM**: Answers strictly from provided documents (no hallucination)
- **Cross-encoder reranking**: Improves retrieval quality
- **Auto reindexing** on document upload/delete
- **Frontend support**: Serves static HTML/JS/CSS

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/rag-chatbot-api.git
cd rag-chatbot-api

```
### 2. Create a virtual environment 
```bash
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
```
if prefer using Conda

```bash
conda create --name <env_name> python=<version>
conda activate <env_name>
```
### 3. Installing dependencies
```bash
pip install -r requirements.txt
```


## Project Structure

.  
├── app.py                       # FastAPI app entrypoint  
├── bulk_ingest.py              # Ingests all files from ./data  
├── data/                       # Uploaded documents  
├── static/                     # Frontend HTML/CSS/JS  
├── models/  
│   └── llama_wrapper.py        # Local LLM (Mistral/OpenHermes wrapper)  
├── memory/  
│   ├── embedder.py             # Embedding logic (MiniLM or similar)  
│   ├── memory_store.py         # Vector store (e.g., ChromaDB)  
│   └── logger.py               # JSON-based chat logger  
├── utils/  
│   ├── pipeline_runner.py      # HyDE and standard pipeline runners  
│   └── prompt_builder.py       # Builds system+context prompts  
├── hybrid_retrieval_pipeline.py# Hybrid RAG retrieval logic  
├── document_ingestor.py        # Ingests individual documents  
├── requirements.txt  
└── README.md  

## Running the App
```bash
uvicorn app:app --reload
```

### API endpoints
| Method | Endpoint          | Description                      |
| ------ | ----------------- | -------------------------------- |
| GET    | `/`               | Serves the frontend UI           |
| POST   | `/upload`         | Uploads and ingests a document   |
| POST   | `/chat`           | Chat with the LLM using RAG      |
| GET    | `/documents`      | Lists all uploaded documents     |
| DELETE | `/documents/{fn}` | Deletes a document and reindexes |
| POST   | `/reindex`        | Manually reindex all files       |
| GET    | `/health`         | Health check for readiness       |


### LLM + Retrieval

    LLM: openhermes-2.5-mistral-7b.Q4_K_M.gguf (run locally via llama-cpp-python)

    Embedding: all-MiniLM-L6-v2 via SentenceTransformers

    Cross-Encoder: ms-marco-MiniLM-L-6-v2

    Vector store: ChromaDB (can be swapped)


### Sample screenshots
![Screenshot 2025-06-06 140004](https://github.com/user-attachments/assets/9f0decb4-c6f9-4f65-9b21-a52ca16f3eed)

