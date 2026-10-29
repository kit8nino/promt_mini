# Lecture: Retrieval‑Augmented Generation (RAG) and External Knowledge  

## 1. What is RAG?  

Retrieval‑Augmented Generation combines a **retriever** (search over a document collection) with a **generator** (LLM). The retriever fetches relevant passages, which are then injected into the prompt as additional context.  

### Why use RAG?  

- **Up‑to‑date information** – the model can answer questions about recent events that are not in its training data.  
- **Domain specificity** – you can provide proprietary documentation, API specs, or code bases.  
- **Reduced hallucinations** – the model grounds its answer in real text.  

## 2. Basic workflow  

1. **Query** – the user’s question.  
2. **Retriever** – e.g., BM25, dense vector search (FAISS, Elasticsearch). Returns top‑k passages.  
3. **Prompt construction** – prepend retrieved passages to the user prompt, optionally with a system prompt that explains the role of the passages.  
4. **Generation** – LLM produces the final answer.  

```
System: Ты — senior backend‑разработчик. Используй приведённые ниже фрагменты документации, чтобы ответить на вопрос.  
Retrieved passages:  
1. "FastAPI позволяет объявлять зависимости через Depends(...)."  
2. "SQLAlchemy 2.0 поддерживает асинхронные сессии через async_session."  

User: Как реализовать асинхронный доступ к базе в FastAPI?
```  

## 3. Implementation sketch (Python)  

```python
from sentence_transformers import SentenceTransformer
import faiss
import openai  # or any LLM client

# 1. Encode documents
model = SentenceTransformer('all-MiniLM-L6-v2')
doc_embeddings = model.encode(corpus)          # corpus = list of strings
index = faiss.IndexFlatL2(doc_embeddings.shape[1])
index.add(doc_embeddings)

def retrieve(query, k=3):
    q_emb = model.encode([query])
    _, I = index.search(q_emb, k)
    return [corpus[i] for i in I[0]]

def generate_answer(query):
    passages = retrieve(query)
    system = "Ты — senior backend‑разработчик. Используй приведённые ниже фрагменты."
    prompt = f"{system}\nRetrieved passages:\n" + "\n".join(f"{i+1}. {p}" for i, p in enumerate(passages)) + f"\n\nUser: {query}"
    response = openai.ChatCompletion.create(messages=[{"role":"user","content":prompt}])
    return response.choices[0].message.content
```  

## 4. Tips & pitfalls  

| Tip | Reason |
|-----|--------|
| **Limit the number of passages** (3‑5) | Keeps token usage low and reduces noise. |
| **Add a clear instruction** that passages are *authoritative* | Prevents the model from ignoring them. |
| **Validate the final output** (JSON schema, unit tests) | Guarantees that retrieved data was used correctly. |
| **Refresh the index** when documents change | Keeps the knowledge base current. |

## 5. Advanced variations  

- **Hybrid retrieval** – combine sparse (BM25) and dense (vector) scores.  
- **Re‑ranking** – use a second LLM to rank retrieved passages before injection.  
- **Iterative RAG** – after the first answer, ask the model if it needs more context and perform another retrieval step.  

---  

*RAG is a bridge between pure LLM generation and traditional information retrieval. In the next lecture we will wrap up the course and look at next steps.*  
