# Clinical RAG Demo — Evidence-Grounded Q&A with Source Tracing

A lightweight Retrieval-Augmented Generation (RAG) prototype that answers questions from a clinical/XAI document and prints **verifiable source snippets** (page + excerpt). Built as a portfolio demo to showcase **embedding-based retrieval, vector search, grounded prompting, and reliability checks**.

---

## What this repo contains

- `notebooks/RAG_demo.ipynb` — end-to-end Colab notebook:
  - PDF ingestion → chunking → embeddings → vector store (Chroma) → retrieval → grounded Q&A (Groq LLM)
  - Source tracing (`SOURCES`) for each answer
  - Negative test query to validate abstention when evidence is missing

- `assets/` — screenshots of key outputs (recommended for quick review)

---

## Why this matters

Many GenAI applications fail when the model “sounds confident” but cannot justify the answer. This demo enforces **grounding**:
- The model must answer **only** using retrieved evidence.
- If the answer is not present in retrieved context, it must say:  
  **“I don't know based on the provided documents.”**

---

## Pipeline overview

1. **PDF ingestion**  
   Load a PDF into text pages.

2. **Chunking**  
   Split text into overlapping chunks (prevents important sentences from being cut off).

3. **Embeddings (local)**  
   Convert each chunk into a semantic vector with Sentence-Transformers.

4. **Vector store (local)**  
   Store vectors + metadata (page number) in ChromaDB.

5. **Retrieval + grounded generation**  
   Retrieve Top-K relevant chunks and generate an answer constrained to that context (Groq).

6. **Source tracing**  
   Print which chunks/pages were used.

---

## Demo outputs (add your screenshots)

> Recommended screenshots to include in `assets/`:
- A normal question showing `ANSWER` + `SOURCES`
- The negative test question showing abstention (“I don’t know…”)
- The anti-hallucination prompt template or retriever configuration

Example placeholders:

![Answer + Sources](assets/screenshot_answer_sources.png)

![Negative Test](assets/screenshot_negative_test.png)

---

## Tech stack

- **Python** (Google Colab)
- **LangChain** (orchestration)
- **Sentence-Transformers** (local embeddings)
- **ChromaDB** (local vector database)
- **Groq LLM** (fast generation)

---

## How to run (Google Colab)

1. Open `notebooks/RAG_demo.ipynb` in Colab
2. Run the install cell(s)
3. Upload a PDF when prompted
4. Paste your **Groq API key** when prompted
5. Run the remaining cells to view:
   - `ANSWER`
   - `SOURCES` (page + excerpt)
   - Negative test behavior

### Notes
- HuggingFace authentication (`HF_TOKEN`) is **optional** for public embedding models.
- If you see dependency warnings in Colab, they are usually safe to ignore as long as the notebook runs.

---

## Reliability checks included

- **Anti-hallucination prompt**: forces the model to abstain when evidence is missing.
- **Negative test query**: verifies the model does not invent facts not present in the document.
- **Source tracing**: each answer includes the supporting chunks/pages for manual verification.

---

## Limitations & next steps

Current scope:
- Single-document RAG
- Basic “stuff” chain (simple context stuffing)

Planned improvements:
- Retriever tuning (Top-K, MMR), chunk-size experiments
- Simple evaluation (answer coverage, abstention rate, retrieval hit-rate)
- Multi-document collections (guidelines + policies)
- Cleaner citations formatting in the final answer

---

## Security

- Do **not** commit API keys.
- This repo reads the Groq key at runtime (prompted input) rather than storing it in code.

---

## Acknowledgements

This notebook follows common RAG patterns from public documentation and examples in IST-736, customized for clinical/XAI document QA with source tracing and negative-test validation.