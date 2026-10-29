# Lecture 3 – Retrieval‑Augmented Generation (RAG) and Practical Examples

## 1. Why Retrieval‑Augmented Generation?
Standard LLMs generate text solely from their internal parameters, which can lead to:
- **Hallucinations** – fabricating facts not present in training data.  
- **Stale Knowledge** – inability to reflect recent information.

RAG addresses these issues by **retrieving** relevant external documents and feeding them into the generation process.

## 2. Core RAG Architecture
