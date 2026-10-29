# Lecture 03 – Task Decomposition, Prompt Chaining, and Retrieval‑Augmented Generation (RAG)

## 1. Task Decomposition for AI Agents

### 1.1 Why Decomposition Matters

- **Одно действие** – задача должна описываться глаголом («сгенерировать», «проанализировать», «сравнить»).  
- **Ясный контракт** – модель знает, что от неё требуется, и выдаёт предсказуемый результат.  
- **Контекст + Инструкция + Формат** – три базовых блока, без которых модель будет «угадывать» ответ.

### 1.2 Standard Decomposition Template

```
Ты — [роль/персона].
Контекст: [доменно‑специфическая информация, ограничения].
Задача: [одно конкретное действие].
Формат вывода: [структура ответа, например JSON, Markdown‑таблица].
```

#### Example

```
Ты — senior Python‑разработчик.  
Контекст: CSV содержит столбцы `name`, `age`.  
Задача: прочитать файл и вернуть список словарей `{name, age}`.  
Формат вывода: JSON‑массив.
```

### 1.3 Decomposition Workflow

1. **Identify the final goal** (what the user ultimately wants).  
2. **Break it into atomic sub‑tasks** – each satisfies the template above.  
3. **Chain the sub‑tasks**: feed the output of step N as the input to step N+1.  
4. **Specify the output format for every step** – this simplifies parsing and aggregation.

#### Full Example (CSV → Filter → JSON string)

| Шаг | Промпт (по шаблону) |
|-----|---------------------|
| 1 | Ты — senior Python‑разработчик.<br>Контекст: CSV содержит столбцы `name`, `age`.<br>Задача: прочитать CSV и вернуть список словарей `{name, age}`.<br>Формат вывода: JSON‑массив. |
| 2 | Ты — senior Python‑разработчик.<br>Контекст: получен массив из шага 1.<br>Задача: отфильтровать элементы, где `age` > 18.<br>Формат вывода: JSON‑массив. |
| 3 | Ты — senior Python‑разработчик.<br>Контекст: отфильтрованный массив из шага 2.<br>Задача: вывести его как строку JSON.<br>Формат вывода: строка JSON. |

## 2. Retrieval‑Augmented Generation (RAG)

### 2.1 What is RAG?

RAG combines a **retriever** (search over a document collection) with a **generator** (LLM). The retriever fetches relevant passages, which are then injected into the prompt as additional context.

#### Why use RAG?

- **Up‑to‑date information** – the model can answer questions about recent events.  
- **Domain specificity** – you can provide proprietary documentation, API specs, or code bases.  
- **Reduced hallucinations** – the model grounds its answer in real text.

### 2.2 Basic RAG workflow

1. **Query** – the user’s question.  
2. **Retriever** – e.g., BM25, dense vector search (FAISS, Elasticsearch). Returns top‑k passages.  
3. **Prompt construction** – prepend retrieved passages to the user prompt, optionally with a system prompt that explains the role of the passages.  
4. **Generation** – LLM produces the final answer.

### 2.3 Implementation sketch (Python)

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
    system = "Ты — senior backend‑разработчик. Используй приведённые ниже фрагменты как авторитетный источник."
    prompt = f"{system}\nRetrieved passages:\n" + "\n".join(f"{i+1}. {p}" for i, p in enumerate(passages)) + f"\n\nUser: {query}"
    response = openai.ChatCompletion.create(messages=[{"role":"user","content":prompt}])
    return response.choices[0].message.content
```

### 2.4 Tips & pitfalls (RAG)

| Tip | Reason |
|-----|--------|
| **Limit the number of passages** (3‑5) | Keeps token usage low and reduces noise. |
| **Add a clear instruction** that passages are *authoritative* | Prevents the model from ignoring them. |
| **Validate the final output** (JSON schema, unit tests) | Guarantees that retrieved data was used correctly. |
| **Refresh the index** when documents change | Keeps the knowledge base current. |

## 3. Combining Decomposition & RAG

You can retrieve external knowledge **at each step** of a chained workflow. For example:

1. **Step 1** – Retrieve API documentation, then ask the model to generate a function skeleton.  
2. **Step 2** – Retrieve performance benchmarks, then ask the model to suggest optimizations.  
3. **Step 3** – Retrieve security guidelines, then ask the model to audit the generated code.

This pattern yields highly grounded, modular solutions that are easier to test and verify.

---  

*Use the decomposition template to structure complex requests, and enrich each atomic step with RAG when external facts are required.*  
