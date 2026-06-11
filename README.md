# 🤖 Clinical LLM RAG Pipeline

> **RAG-powered clinical decision support system** — ingests PubMed medical literature, structures semantic embeddings in ChromaDB, and serves LLM-based natural language Q&A via FastAPI + Streamlit.

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)
![ChromaDB](https://img.shields.io/badge/ChromaDB-FF6B35?style=for-the-badge&logo=database&logoColor=white)
![HuggingFace](https://img.shields.io/badge/Hugging%20Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)
![Status](https://img.shields.io/badge/Status-In%20Progress-orange?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

---

## 📋 Overview

This project builds a production-ready **Retrieval-Augmented Generation (RAG)** pipeline specifically designed for clinical decision support. It directly supports doctoral research into AI-driven predictive decision systems for life sciences and healthcare.

The system enables clinicians and researchers to ask natural language questions over a curated corpus of biomedical literature and receive grounded, citation-backed answers — reducing hallucination through retrieval-first architecture.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Data Ingestion Layer                  │
│  PubMed API  ──►  PDF Parser  ──►  Text Chunker          │
└──────────────────────────┬──────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────┐
│                   Embedding & Storage Layer              │
│  HuggingFace Embeddings  ──►  ChromaDB Vector Store      │
└──────────────────────────┬──────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────┐
│                    RAG Query Pipeline                    │
│  Query  ──►  Retriever  ──►  LLM (LangChain)  ──►  Answer│
└──────────────────────────┬──────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────┐
│                    Serving Layer                         │
│  FastAPI REST Endpoints  +  Streamlit Chat Interface     │
└─────────────────────────────────────────────────────────┘
```

---

## 🚀 Features

- **PubMed Ingestion:** Automated literature fetch via NCBI E-utilities API with configurable query terms and date ranges
- **Intelligent Chunking:** Sentence-aware text splitting preserving clinical context windows
- **Semantic Embeddings:** HuggingFace `sentence-transformers` for domain-tuned biomedical embeddings
- **Vector Store:** ChromaDB for persistent, efficient similarity search over thousands of documents
- **LangChain RAG Chain:** Context-injected prompt construction with source citation in every response
- **FastAPI Backend:** RESTful endpoints for query submission, document upload, and health checks
- **Streamlit UI:** Conversational chat interface with source document previews and relevance scores
- **MLflow Tracking:** Query latency, retrieval precision, and answer quality metrics logging

---

## 📁 Project Structure

```
clinical-llm-rag-pipeline/
├── data/
│   ├── raw/                    # Downloaded PubMed PDFs & abstracts
│   └── processed/              # Cleaned, chunked text documents
├── src/
│   ├── ingestion/
│   │   ├── pubmed_fetcher.py   # PubMed API client
│   │   └── pdf_parser.py       # PDF text extraction
│   ├── embeddings/
│   │   ├── chunker.py          # Text chunking strategies
│   │   └── embedder.py         # HuggingFace embedding wrapper
│   ├── vectorstore/
│   │   └── chroma_store.py     # ChromaDB CRUD operations
│   ├── rag/
│   │   ├── retriever.py        # Similarity search & reranking
│   │   └── chain.py            # LangChain RAG chain setup
│   └── serving/
│       ├── api.py              # FastAPI application
│       └── app.py              # Streamlit chat interface
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_embedding_evaluation.ipynb
│   └── 03_rag_benchmarking.ipynb
├── tests/
│   ├── test_ingestion.py
│   ├── test_retriever.py
│   └── test_api.py
├── requirements.txt
├── .env.example
└── README.md
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Python 3.11+ |
| RAG Framework | LangChain |
| LLM | HuggingFace `BioMedLM` / OpenAI GPT-4 (configurable) |
| Embeddings | `sentence-transformers/all-MiniLM-L6-v2` |
| Vector Store | ChromaDB |
| API | FastAPI + Uvicorn |
| Frontend | Streamlit |
| Data Source | PubMed / NCBI E-utilities |
| Experiment Tracking | MLflow |
| Testing | Pytest |

---

## ⚙️ Setup & Installation

```bash
# Clone the repository
git clone https://github.com/BrianKeith2027/clinical-llm-rag-pipeline.git
cd clinical-llm-rag-pipeline

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure environment variables
cp .env.example .env
# Edit .env with your API keys (HuggingFace, OpenAI optional)
```

---

## 🔬 Usage

**1. Ingest PubMed Literature**
```bash
python src/ingestion/pubmed_fetcher.py --query "clinical decision support AI" --max-results 500
```

**2. Build Vector Store**
```bash
python src/vectorstore/chroma_store.py --build
```

**3. Launch FastAPI Backend**
```bash
uvicorn src.serving.api:app --reload --port 8000
```

**4. Launch Streamlit Chat UI**
```bash
streamlit run src/serving/app.py
```

---

## 📊 Evaluation Metrics

| Metric | Target |
|--------|--------|
| Retrieval Precision@5 | > 0.85 |
| Answer Faithfulness | > 0.90 |
| Avg. Query Latency | < 3s |
| Context Relevance | > 0.80 |

---

## 🔗 Research Context

This project is part of doctoral research at the **University of Maryland** investigating AI-driven predictive decision systems for life sciences. It extends the work from [healthcare-knowledge-graph-ml](https://github.com/BrianKeith2027/healthcare-knowledge-graph-ml) by adding a RAG layer on top of the knowledge graph for free-form natural language querying.

---

## 📜 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 👤 Author

**Brian Stratton** — Senior Data Engineer | AI/ML Engineer | DBA Candidate @ UMD

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/briankstratton/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/BrianKeith2027)
