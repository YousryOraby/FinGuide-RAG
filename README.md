# FinGuide RAG

A retrieval-augmented assistant for exploring financial controlling and regulatory standards. Ask a question in plain English and the app searches its reference documents, ranks relevant passages, and drafts an answer with source attribution.

**Live demo:** https://share.streamlit.io/user/yousryoraby

## What it does

- Searches a small curated collection of financial and regulatory reference documents.
- Combines semantic vector search with BM25 keyword search using Reciprocal Rank Fusion.
- Retrieves precise child passages, then reranks them with a cross-encoder.
- Restores the broader parent passage before answer generation to retain context.
- Uses Groq to generate a grounded response and displays the source, query expansion, recognized terms, retrieved passages, scores, and context used.
- Includes a Streamlit interface with sample questions and a control for the number of reranked results.

## How a question moves through the system

```text
Question
   -> domain-aware query expansion and entity recognition
   -> dense search (ChromaDB) + sparse search (BM25)
   -> Reciprocal Rank Fusion
   -> cross-encoder reranking
   -> parent-context reconstruction
   -> grounded answer with source attribution
```

## Technology

Python · Streamlit · ChromaDB · Sentence Transformers · spaCy · BM25 · LangChain · Groq

The embedding model is `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`; the reranker is `BAAI/bge-reranker-base`.

## Run locally

1. Create and activate a Python environment, then install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

2. Add a Groq API key to `.streamlit/secrets.toml`:

   ```toml
   GROQ_API_KEY = "your-groq-api-key"
   ```

3. Start the app from the repository root:

   ```bash
   streamlit run ui/streamlit_app.py
   ```

4. In the app, choose **Run Document Ingestion** and wait for indexing to complete before asking questions.

For Streamlit Community Cloud, set `GROQ_API_KEY` in the app's **Settings → Secrets** instead of committing a secrets file.

## Repository layout

```text
data/raw/          Curated JSON reference corpus for the hosted demo
src/ingestion/     Parsing, hierarchical chunking, and indexing
src/retrieval/     Query expansion, hybrid retrieval, and reranking
src/generation/    Grounded answer prompt and RAG pipeline
ui/                Streamlit interface
```

The hosted demo uses a deliberately small JSON corpus. The larger 75k-row dataset and generated indexes are excluded from this repository. Indexes are built when ingestion runs and may need to be rebuilt if the hosting environment restarts.

## Scope

This is an educational project and document-search aid. Answers depend on the reference corpus and can be incomplete or outdated; verify regulatory requirements against the official source before relying on them.
