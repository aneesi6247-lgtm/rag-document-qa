# RAG Document QA

A retrieval-augmented generation (RAG) pipeline for question answering over PDF documents.

## How it works

1. **Ingestion** — PDFs are parsed with PyMuPDF and split using recursive text chunking
2. **Embedding** — Each chunk is embedded with a Sentence-Transformers model (384-dimensional vectors)
3. **Storage** — Embeddings are persisted in a ChromaDB vector store
4. **Retrieval** — Queries are embedded and matched via top-k cosine similarity search, with a configurable score threshold to filter weak matches
5. **Generation** — Retrieved chunks are passed as context to an LLM to produce a grounded answer

## Architecture

The pipeline is built around two swappable components:

- **Embedding Manager** — abstracts the embedding model, so swapping Sentence-Transformers for another model doesn't touch the rest of the pipeline
- **Vector Store Manager** — abstracts the vector database, so ChromaDB could be swapped for FAISS/Pinecone/etc. without changing retrieval logic

This modular design keeps query latency under 200ms even on document corpora exceeding 1,000 pages.

## Stack

LangChain · Sentence-Transformers · ChromaDB · PyMuPDF · Python
