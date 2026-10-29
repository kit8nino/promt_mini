# Lecture 02 – Prompt Engineering: Principles, Templates, Style Guide, and Techniques  

## 1. What is Prompt Engineering?  

Prompt‑engineering — это дисциплина точного формулирования задачи для больших языковых моделей (LLM). В отличие от «магических фраз», здесь применяется строгий инженерный подход, аналогичный написанию технического задания или API‑контракта.

## 2. The Three‑Component Template (Трёхкомпонентный шаблон)

| Компонент | Что описывает | Как оформить (пример) |
|----------|---------------|----------------------|
| **Контекст / Persona** | «Кем» является модель, в какой предметной области работает, какие ограничения соблюдает. | `Ты — senior TypeScript‑разработчик. Контекст: микросервисы на Python 3.12, PostgreSQL 16, без ORM.` |
| **Роль** | Явный «линз‑фокус», задающий экспертизу, тон и границы. Формула: **Ты — [роль] + [аудитория] + [ограничение]**. | `Ты — senior backend‑разработчик. Объясни JWT junior‑разработчику, знающему HTTP, без кулинарных аналогий.` |
| **Задача / Instruction** | Одно конкретное действие, сформулированное глаголом. | `Сгенерируй тесты для функции «calculateTax».` |
| **Формат вывода** | Жёстко заданный контракт (JSON, Markdown‑таблица, CSV, нумерованный список). | `Ответ в JSON: {"testName":"calculateTax‑edge‑case‑1","input":{"salary":120000},"expected":24000,"description":"Проверка границы 100 kUSD"}` |

> **Mini‑template**  
> `Ты — [роль] + [аудитория] + [ограничение]. Контекст: [вводные]. Задача: [одно действие]. Формат: [структура ответа].`

## 3. Style Guide for Prompt Writing (excerpt)

| Principle | Description |
|----------|-------------|
| **Clarity** | Use simple, unambiguous language. |
| **Brevity** | Keep system prompts ≤ 30 tokens, user prompts ≤ 150 tokens when possible. |
| **Consistency** | Re‑use the same role formula (`Ты — [роль] + [аудитория] + [ограничение]`). |
| **Explicit format** | Never add “Here is the JSON:” – output must start directly with the contract. |
| **Versioning** | Store prompts in a `prompts/` directory, name them `v01_<purpose>.md`. |

### Naming conventions

- **System prompts** → `system_<module>.md`  
- **User prompts / examples** → `example_<scenario>.md`  
- **Role definitions** → `role_<name>.md`

### Formatting rules

1. **Code blocks** – always use triple backticks with language tag (`json`, `python`).  
2. **JSON contracts** – keys must be `snake_case`, values typed (`string`, `int`, `float`).  
3. **Markdown tables** – use pipe (`|`) syntax, no extra spaces.

## 4. Prompt Engineering Techniques

| Technique | What it does | How to apply |
|-----------|--------------|--------------|
| **Few‑Shot / One‑Shot** | Calibrates the model with input‑output examples. | Include 2‑5 `Input:` / `Output:` pairs at the start of the prompt. |
| **Chain‑of‑Thought (CoT)** | Asks the model to reason step‑by‑step. | Add `think step‑by‑step` or `Разбей задачу на шаги и объясни каждый`. |
| **Ask‑Before‑Answer** | Model asks clarifying questions before answering. | Include `Ask questions for clarification before generating your answer`. |
| **Self‑Consistency** | Run the same prompt multiple times, pick the most common answer. | Execute 3‑5 runs, then aggregate (majority vote, median). |
| **Self‑Evaluation** | After generation, the model checks its own output. | Append `Please evaluate your answer and point out possible issues`. |
| **Constructive Criticism** | Direct request for honest critique. | `Criticize my code` or `Provide a critical review of the solution`. |
| **Emotional Prompting** | Increases perceived importance of the task. | `This is very important for my career` or `I need a perfect answer`. |
| **Retrieval‑Augmented Generation (RAG)** | Adds external documents to the context. | `Use the following excerpts …` and include relevant text. |
| **Prompt‑Chaining** | Breaks a large task into sequential steps, feeding each result into the next. | Format as `Step 1 → … → Step N`. |
| **Role Anchoring** | Keeps the model’s persona stable over long dialogs. | Include an immutable identity block in the system prompt. |

### Combining techniques

1. **Few‑Shot + CoT** – show a few‑shot example that also demonstrates step‑by‑step reasoning.  
2. **Ask‑Before‑Answer + Self‑Evaluation** – first clarify, then evaluate the final answer.  
3. **RAG + Prompt‑Chaining** – each chain step can retrieve additional context before generation.

## 5. Practical Prompt Example (Russian system prompt, English user request)

```
System:
Ты — senior backend‑разработчик. Твоя задача — отвечать в формате JSON без лишних слов.

User:
Input: Напиши функцию, которая принимает список чисел и возвращает их среднее.
```

**Expected model output**

```json
{
  "function_name": "mean",
  "code": "def mean(nums):\n    return sum(nums) / len(nums)",
  "description": "Вычисляет среднее арифметическое"
}
```

---  

*With these principles, templates, and techniques you can craft reliable, parsable prompts for a wide range of tasks.*  
