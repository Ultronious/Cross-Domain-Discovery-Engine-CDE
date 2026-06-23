# RouteLLM

A multi-specialist RAG system that routes questions to domain-specific 
knowledge bases using semantic search and local LLMs.

> Built as a stepping stone toward a cross-domain scientific discovery engine 
> that finds knowledge humans can't see — by detecting gaps between domains 
> using vector interpolation.

## What it does

Instead of one large model handling everything, RouteLLM uses small 
specialist knowledge bases — each covering a specific domain — with a 
router that directs each question to the right specialist.

## Architecture

Question → Router → Domain KB (FAISS) → Context → LLM → Answer

- **Router** — keyword-based scoring (Stage 0), semantic classifier (Stage 1 planned)
- **Knowledge Bases** — Wikipedia articles chunked and embedded using MiniLM (384D vectors)
- **Vector Search** — FAISS IndexFlatL2 for semantic retrieval
- **LLM** — qwen3:8b running locally via Ollama

## Stack

- Python, FAISS, sentence-transformers, Ollama, Jupyter

## How to run

```bash
# Install dependencies
pip install faiss-cpu sentence-transformers requests wikipedia-api jupyter

# Pull a model
ollama pull qwen3:8b

# Open the notebook
jupyter notebook routellm_stage0.ipynb
```

Run cells 1-10 in order.

## Roadmap

- **Stage 0** ✅ Keyword router + FAISS KB + Ollama RAG pipeline
- **Stage 0.5**📅 arXiv integration (replace Wikipedia with real research papers via arXiv API, no key needed, search by category: physics.plasm-ph, math.DG, cs.LG), query rewriting before search
- **Stage 1** 📅 Semantic router using embedding-based classification
- **Stage 2** 📅 RL-trained router with GRPO reward signal
- **Stage 2.5** 📅 Cross-domain retrieval — embed query against ALL domain KBs simultaneously, interpolate between top-2 domain vectors, compute midpoint M, KNN search from M, flag empty regions as discovery candidates, generative model proposes hypothesis for gap
- **Stage 3** 📅 Cross-domain gap detection via vector interpolation