# Lecture 01 – Introduction to Generative AI, LLM Architecture, and Context

## 1. What is Generative AI?

Generative AI – a class of models that can create new content (text, code, images, audio) from a prompt. The most common type today is the **large language model (LLM)**, which predicts the next token in a sequence of tokens.

### Why does it matter for developers?

- **Code assistance** – tools like GitHub Copilot, CodeLlama, StarCoder.  
- **Documentation & summarisation** – automatically generate API docs, release notes.  
- **Idea generation** – brainstorm architectures, write tests, create UI copy.

## 2. Core concepts (high‑level)

| Concept | Short description |
|--------|-------------------|
| **Token** | Smallest unit the model works with (word piece, sub‑word, punctuation). |
| **Context window** | Number of recent tokens the model can attend to (e.g., 4 096 tokens). |
| **Prompt** | The text you give the model – it can contain system instructions, user request, examples. |
| **Temperature / Top‑p** | Sampling parameters that control randomness vs. determinism. |
| **Fine‑tuning / Instruction‑tuning** | Adapting a base model to a specific domain or style. |

## 3. LLM Architecture (Устройство LLM и особенности её работы)

### 3.1 Что такое LLM (Large Language Model) на самом деле

LLM — это огромный статистический вычислитель, построенный на принципе предсказания следующего элемента последовательности. Вместо того чтобы «понимать» язык, модель оценивает условную вероятность появления каждого возможного токена, учитывая уже наблюдаемые токены. Эта задача полностью эквивалентна задаче авторегрессионного фильтра в радиофизике, где текущий отсчёт сигнала предсказывается на основе предыдущих отсчётов.

### 3.2 Токен и токенизация

Токен — это элементарная единица, с которой работает модель. Токенизация преобразует произвольный текст в последовательность индексов из фиксированного словаря, аналогично дискретизации сигнала.

### 3.3 Контекстное окно

Контекстное окно — это ограниченный набор последних токенов, которые модель может одновременно учитывать. Практически все современные LLM используют окна длиной от 2 048 до 4 096 токенов. Ограничение обусловлено квадратичной сложностью механизма внимания.

### 3.4 Веса и параметры

Веса — это элементы матриц, которые преобразуют входные эмбеддинги в более абстрактные представления. Параметры — общее количество обучаемых скалярных величин в модели; в современных LLM их число измеряется в миллиардах и триллионах.

### 3.5 Механизм внимания

Механизм внимания (attention) вычисляет взаимную корреляцию между всеми парами токенов внутри контекстного окна, используя запросы (Q), ключи (K) и значения (V).

### 3.6 Природа галлюцинаций

Галлюцинации возникают из‑за того, что модель оперирует лишь вероятностным распределением, а не истинной семантикой. При ограниченном контекстном окне модель может «забыть» важные детали, что усиливает вероятность генерации неверного текста.

## 4. Context Window & System Prompt (Lecture 1b)

### 4.1 The role of the context window

- The **context window** is the set of the most recent tokens that the model can attend to when generating the next token.  
- Modern LLMs typically have windows of **2 048 – 4 096 tokens**.  
- Anything that falls outside this window is **invisible** to the model and cannot influence the answer.

### 4.2 Why it matters for prompting

| Situation | Effect if the information is inside the window | Effect if it is outside |
|-----------|-----------------------------------------------|------------------------|
| Long code base | Model can reference earlier definitions, variable names, imports. | Model “forgets” earlier definitions → broken code. |
| Multi‑turn conversation | Model remembers previous user messages and its own answers. | Model loses the thread, may repeat or contradict itself. |

### 4.3 System Prompt vs. User Prompt

- **System prompt** sets the **persona, role, and high‑level behavior** of the model.  
- **User prompt** contains the concrete request, data, or question.

```
System: Ты — senior backend‑разработчик. Твоя задача — давать короткие, технически точные ответы без лишних пояснений.  
User: Как реализовать кеширование запросов в FastAPI?
```

The system prompt is processed **once** at the beginning of the session and stays at the start of the token sequence, ensuring the model always has the role information in its context.

### 4.4 Practical tips

1. **Place the system prompt first** – guarantees the role stays within the context window for the longest possible time.  
2. **Keep the system prompt concise** – every token counts; a 30‑token system prompt is usually enough.  
3. **Refresh the role** if you anticipate a very long interaction: resend a short reminder (`You are still the senior backend‑developer`).

---  

*This lecture provides the foundational knowledge required before diving into prompt engineering techniques.*  
