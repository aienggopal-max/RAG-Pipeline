# RAG Pipeline

A Python retrieval-augmented generation pipeline that combines dense Qdrant search
with BM25, fuses rankings with reciprocal rank fusion, and reranks results with a
cross-encoder. FastAPI serves answers with source citations checked against the
retrieved text. The project also includes a LangGraph corrective retrieval flow
and an evaluation harness for retrieval, citation attribution, and answer quality.

## Quick start

With Python 3.11 or later, run these commands in Bash (Git Bash on Windows):

```bash
git clone https://github.com/aienggopal-max/RAG-Pipeline.git
cd RAG-Pipeline
python -m venv .venv
source .venv/bin/activate  # Windows Git Bash: source .venv/Scripts/activate
python -m pip install -e ".[dev]"
python -m pytest
python -m uvicorn rag.api.app:app --port 8000
```

From another terminal, run `curl -fsS http://127.0.0.1:8000/health` to check that
the API is running. Open [the API docs](http://127.0.0.1:8000/docs) to explore
the endpoints.

To query the included sample corpus, stop the server, copy `.env.example` to
`.env`, set `ANTHROPIC_API_KEY` in `.env`, and run:

```bash
export QDRANT_PATH=./storage/qdrant
export CORPUS_DIR=data/sample
python -m rag.indexing.build
python -m uvicorn rag.api.app:app --port 8000
```

The first run downloads the local retrieval models. From another terminal, send a
sample question:

```bash
curl -fsS http://127.0.0.1:8000/query \
  -H 'Content-Type: application/json' \
  -d '{"query":"What is the default speed limit on a dry motorway?"}'
```

## Documentation

- [Architecture and design decisions](docs/architecture.md)
- [Sample corpus and evaluation data](data/README.md)
- [Published evaluation reports](docs/reports/)
- [Make targets for development and evaluation](Makefile)

## License and provenance

This repository is owned by [aienggopal-max](https://github.com/aienggopal-max).
The original project is [Hybrid RAG Pipeline by Samir Anbri](https://github.com/AnbriSamir/hybrid-rag-pipeline).
Imported Git commit metadata was rewritten for this copy. The project is
distributed under the [MIT License](LICENSE), with Samir Anbri's copyright notice.
