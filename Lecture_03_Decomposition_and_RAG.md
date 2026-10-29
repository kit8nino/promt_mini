# Лекция 03 – Декомпозиция задач, чейнинг промптов и генерация с дополнением извлечёнными данными (RAG)

## 1. Декомпозиция задач для ИИ‑агентов

### 1.1 Почему декомпозиция важна

- **Одно действие** – задача должна описываться глаголом («сгенерировать», «проанализировать», «сравнить»)
- **Ясный контракт** – модель знает, что от неё требуется, и выдаёт предсказуемый результат
- **Контекст + Инструкция + Формат** – три базовых блока, без которых модель будет «угадывать» ответ

### 1.2 Стандартный шаблон декомпозиции

```
Ты — [роль/персона].
Контекст: [доменно‑специфическая информация, ограничения].
Задача: [одно конкретное действие].
Формат вывода: [структура ответа, например JSON, Markdown‑таблица].
```

#### Example
#### Пример
#### Пример

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
## 2. Генерация с дополнением извлечёнными данными (RAG)

### 2.1 What is RAG?
### 2.1 Что такое RAG?

RAG сочетает **ретривер** (поиск по коллекции документов) с **генератором** (LLM). Ретривер находит релевантные фрагменты, которые затем внедряются в промпт как дополнительный контекст.

#### Why use RAG?
#### Зачем использовать RAG?

- **Up‑to‑date information** – the model can answer questions about recent events.  
- **Domain specificity** – you can provide proprietary documentation, API specs, or code bases.  
- **Актуальная информация** – модель может отвечать на вопросы о недавних событиях.
- **Предметная специфика** – вы можете предоставлять закрытую документацию, спецификации API, кодовые базы.
- **Меньше галлюцинаций** – модель подкрепляет ответ реальным текстом.

### 2.2 Basic RAG workflow
### 2.2 Базовый рабочий процесс RAG

1. **Query** – the user’s question.  
2. **Retriever** – e.g., BM25, dense vector search (FAISS, Elasticsearch). Returns top‑k passages.  
3. **Prompt construction** – prepend retrieved passages to the user prompt, optionally with a system prompt that explains the role of the passages.  
![RAG Architecture](images/lecture03_rag_architecture.png "RAG Architecture: Query → Retriever (Vector Search) → Top-k Passages → Prompt Construction → LLM Generator → Answer")

![RAG Workflow Steps](images/lecture03_rag_workflow.png "RAG Workflow: 1. Query 2. Retriever 3. Prompt Construction 4. Generation")

1. **Query** – the user’s question.
2. **Retriever** – e.g., BM25, dense vector search (FAISS, Elasticsearch). Returns top‑k passages.
3. **Prompt construction** – prepend retrieved passages to the user prompt, optionally with a system prompt that explains the role of the passages.
4. **Generation** – LLM produces the final answer.
1. **Запрос** – вопрос пользователя.
2. **Ретривер** – например, BM25, плотный векторный поиск (FAISS, Elasticsearch). Возвращает top‑k фрагментов.
3. **Построение промпта** – добавьте извлечённые фрагменты к пользовательскому промпту, опционально с системным промптом, объясняющим их роль.
4. **Генерация** – LLM выдаёт финальный ответ.

### 2.3 Implementation sketch (Python)
### 2.3 Схема реализации (Python)

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
### 2.4 Советы и подводные камни (RAG)

| Tip | Reason |
|-----|--------|
| **Limit the number of passages** (3‑5) | Keeps token usage low and reduces noise. |
| **Add a clear instruction** that passages are *authoritative* | Prevents the model from ignoring them. |
| **Validate the final output** (JSON schema, unit tests) | Guarantees that retrieved data was used correctly. |
| **Refresh the index** when documents change | Keeps the knowledge base current. |
| **Ограничьте количество фрагментов** (3‑5) | Держит потребление токенов низким и уменьшает шум |
| **Добавьте чёткую инструкцию**, что фрагменты *авторитетны* | Предотвращает игнорирование их моделью |
| **Валидируйте итоговый вывод** (JSON‑схема, юнит‑тесты) | Гарантирует, что извлечённые данные использованы правильно |
| **Обновляйте индекс** при изменении документов | Поддерживает базу знаний актуальной |

## 3. Комбинирование декомпозиции и RAG

![Decomposition + RAG Pipeline](images/lecture03_decomposition_rag_pipeline.png "Декомпозиция + RAG: На каждом шаге извлекается релевантный контекст → Обработка → Передача к следующему шагу")

Вы можете извлекать внешние знания **на каждом шаге** цепочечного рабочего процесса. Например:

1. **Шаг 1** – Извлеките документацию API, затем попросите модель сгенерировать скелет функции
2. **Шаг 2** – Извлеките бенчмарки производительности, затем попросите модель предложить оптимизации
3. **Шаг 3** – Извлеките руководства по безопасности, затем попросите модель зааудить сгенерированный код

Этот паттерн даёт высоко обоснованные, модульные решения, которые легче тестировать и верифицировать.

---  

*Используйте шаблон декомпозиции для структурирования сложных запросов и обогащайте каждый атомарный шаг RAG, когда нужны внешние факты.*

*Use the decomposition template to structure complex requests, and enrich each atomic step with RAG when external facts are required.*  





![Decomposition + RAG Pipeline](images/lecture03_decomposition_rag_pipeline.png "Декомпозиция + RAG: На каждом шаге извлекается релевантный контекст → Обработка → Передача к следующему шагу")
