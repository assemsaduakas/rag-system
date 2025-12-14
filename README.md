
# Local City Guide & Event Assistant (RAG System)

## Main Idea
This project implements a Retrieval-Augmented Generation (RAG) system that acts as a local city guide for Amsterdam.
Users can ask natural-language questions about attractions, neighborhoods, and events, and the system retrieves
relevant documents from a vector database and uses an LLM to generate grounded answers.

## Concepts Used
- Retrieval-Augmented Generation (RAG)
- Vector embeddings
- Similarity search (FAISS)
- Prompt grounding with retrieved context
- Lightweight UI

## Dataset Concept
The dataset consists of short, original, non-copyrighted textual descriptions of:
- Tourist attractions
- Neighborhoods
- Parks and public spaces
- Recurring cultural events

Each entry contains:
- title
- category
- tags
- description text

The dataset is intentionally small (~20 items) but representative.

## System Design
1. Dataset preparation (Amsterdam city data)
2. Text chunking (per document)
3. Embedding generation using SentenceTransformers
4. Vector storage and similarity search with FAISS
5. User query embedding
6. Top-k document retrieval
7. LLM response generation using retrieved context

## Technical Stack
- Python
- Google Colab
- sentence-transformers
- FAISS
- OpenAI-compatible LLM client
- Jupyter Notebook UI

## Requirements
- Internet access (for LLM API)
- Python 3.9+
- Google Colab or local Python environment

## Limitations
- Small dataset (demo purposes)
- No real-time event updates
- LLM responses depend on retrieved context quality

