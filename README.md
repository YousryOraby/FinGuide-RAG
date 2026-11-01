# FinCon RAG — Streamlit Cloud

This deployment copy uses the Streamlit interface in `ui/streamlit_app.py` and the small JSON sample corpus in `data/raw/`. It deliberately excludes local credentials, generated indexes, the large BAAI CSV, Docker files, and unused evaluation dependencies.

## Deploy

1. This repository and the Streamlit app are public. Do not add private documents, personal datasets, credentials, or local generated indexes.
2. In Streamlit Community Cloud, create an app from that repository and select `ui/streamlit_app.py` as the app file.
3. In Advanced settings, select Python 3.12 and add this secret:

```toml
GROQ_API_KEY = "your-new-Groq-key"
```

4. Deploy the app. On its first visit, click **Run Document Ingestion** and wait for indexing to finish before submitting a question.

The first start downloads the embedding and reranker models. The included corpus is intentionally small for a hosted demo; the 75k-row corpus and its generated indexes are kept out of this repository. Streamlit Cloud storage can be recreated after a restart, so ingestion may need to be run again if the app loses its local index.

Never commit `.env`, `.streamlit/secrets.toml`, ChromaDB data, or generated BM25/parent-store files.
