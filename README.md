# RAG Builder — PDF Analysis with LangChain

> **Learning project — intentionally kept simple.**
> This project is an example built for a Software Solution Architecture learning programme.
> The goal is to demonstrate and understand core RAG concepts (retrieval, chunking, embeddings, reranking, evaluation) in a clear, readable notebook.
> It is **not** production-ready code: there are no unit tests, no containerisation, no CI/CD, and no horizontal scaling.
> Complexity is deliberately minimised so the RAG patterns remain the centre of attention.

---

## What this project does

Analyses a collection of PDF learning materials using a full RAG pipeline:

```
PDF Files → Chunking → ChromaDB (Vector Store)
                               ↓
               RAG Chain (LLM + MMR Retriever + Cohere Reranker)
                               ↓
        Structured Analysis per document (11 queries each)
                               ↓
     RAG Evaluation (context recall + answer faithfulness)
                               ↓
   CSV Export + Plotly Charts + Sankey Diagram → HTML Report
```

**Output artifacts** (written to `output/`):
| File | Contents |
|---|---|
| `analysis_results.csv` | One row per PDF: summary, topics, patterns, technologies, difficulty, source chunks |
| `report.html` | Self-contained browser report: document cards, 6 charts, Sankey relationship diagram |

---

## Prerequisites

- Python 3.10+
- An OpenAI API key **or** a Cohere API key (or both)
- Jupyter Lab or VS Code with the Jupyter extension

---

## Setup

```bash
# 1. Clone the repository
git clone [<repo-url>](https://github.com/dimalu85/dl-ai-sas-rag)
cd RAG

# 2. Create and activate a virtual environment (recommended)
python3 -m venv .venv
source .venv/bin/activate       # macOS / Linux
# .venv\Scripts\activate        # Windows

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure API keys
cp .env.example .env
# Open .env and fill in your OPENAI_API_KEY and/or COHERE_API_KEY

# 5. Add your PDF files
# Copy the PDFs you want to analyse into the pdfs/ folder.
# Any PDF collection works — the queries are tuned for Software Architecture
# material but will run against any technical document set.

# 6. Open and run the notebook
jupyter lab rag-builder-sas.ipynb
# Run cells top-to-bottom (Kernel → Restart & Run All)
```

---

## Dataset note

The original version of this project used EPAM internal Software Solution Architecture learning materials (20 PDFs) which are not publicly available.

**To use your own PDFs:** drop any `.pdf` files into the `pdfs/` folder and run the notebook. No other changes are needed — filenames are picked up automatically.

If you want to replicate results on a public dataset, any collection of software architecture or cloud-computing papers/guides works well (e.g. AWS whitepapers, CNCF guides, Martin Fowler articles exported to PDF).

---

## Configuration

All tuneable settings live in the Step 2 cell of the notebook:

| Variable | Default | Description |
|---|---|---|
| `LLM_PROVIDER` | `"openai"` | `"openai"` or `"cohere"` |
| `OPENAI_MODEL` | `"gpt-4o-mini"` | Any OpenAI chat model |
| `COHERE_MODEL` | `"command-r"` | Any Cohere chat model |
| `EMBEDDING_MODEL` | `"all-mpnet-base-v2"` | HuggingFace sentence-transformer |
| `CHUNK_SIZE` | `1024` | Characters per chunk |
| `CHUNK_OVERLAP` | `128` | Overlap between adjacent chunks |
| `TOP_K_RETRIEVAL` | `4` | Chunks retrieved per query |

The **Cohere reranker** activates automatically when `COHERE_API_KEY` is set — it works with either LLM provider.

---

## Project structure

```
RAG/
├── rag-builder-sas.ipynb   # Main notebook (all logic)
├── requirements.txt        # Pinned dependencies
├── .env.example            # API key template
├── pdfs/                   # Drop your PDFs here
├── vectorstore/            # ChromaDB persistence (auto-created)
└── output/
    ├── analysis_results.csv
    └── report.html
```
