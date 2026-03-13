# RAG Pipeline — PDF Analysis with LangChain
## End-to-end workflow: ingest PDFs → build vector store → run analysis queries → export CSV + HTML report with charts

### Architecture Overview
```
PDF Files  →  Text Extraction  →  Chunking  →  ChromaDB (Vector Store)
                                                        ↓
                                          RAG Chain (LLM + Retriever)
                                                        ↓
                               Structured Analysis (topics, entities, sentiment)
                                                        ↓
                                  CSV Export  +  Plotly Charts  →  HTML Report
```

### Steps
1. Environment Setup & Package Installation
2. Configuration (paths, API keys, model settings)
3. PDF Ingestion & Text Extraction
4. Text Chunking & Preprocessing
5. Embeddings & ChromaDB Vector Store
6. RAG Chain Construction
7. Batch Document Analysis
8. Results Export to CSV
9. Visualizations with Plotly
10. HTML Report Generation
