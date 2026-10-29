# Лекция 02 – Промпт‑инжиниринг: принципы, шаблоны, стиль и техники

## 1. Что такое промпт‑инжиниринг?

Prompt‑engineering — это дисциплина точного формулирования задачи для больших языковых моделей (LLM). В отличие от «магических фраз», здесь применяется строгий инженерный подход, аналогичный написанию технического задания или API‑контракта.

## 2. Трёхкомпонентный шаблон

![Three-Component Prompt Template](images/lecture02_three_component_template.png "Трёхкомпонентный шаблон: Контекст/Персона → Роль → Задача/Инструкция → Формат вывода")

| Компонент | Что описывает | Как оформить (пример) |
|----------|---------------|----------------------|
| **Контекст / Persona** | «Кем» является модель, в какой предметной области работает, какие ограничения соблюдает. | `Ты — senior TypeScript‑разработчик. Контекст: микросервисы на Python 3.12, PostgreSQL 16, без ORM.` |
| **Роль** | Явный «линз‑фокус», задающий экспертизу, тон и границы. Формула: **Ты — [роль] + [аудитория] + [ограничение]**. | `Ты — senior backend‑разработчик. Объясни JWT junior‑разработчику, знающему HTTP, без кулинарных аналогий.` |
| **Задача / Instruction** | Одно конкретное действие, сформулированное глаголом. | `Сгенерируй тесты для функции «calculateTax».` |
| **Формат вывода** | Жёстко заданный контракт (JSON, Markdown‑таблица, CSV, нумерованный список). | `Ответ в JSON: {"testName":"calculateTax‑edge‑case‑1","input":{"salary":120000},"expected":24000,"description":"Проверка границы 100 kUSD"}` |

> **Mini‑template**
> `Ты — [роль] + [аудитория] + [ограничение]. Контекст: [вводные]. Задача: [одно действие]. Формат: [структура ответа].`

## 3. Руководство по стилю написания промптов (выдержка)

| Principle | Description |
|----------|-------------|
| **Clarity** | Use simple, unambiguous language. |
| **Brevity** | Keep system prompts ≤ 30 tokens, user prompts ≤ 150 tokens when possible. |
| **Consistency** | Re‑use the same role formula (`Ты — [роль] + [аудитория] + [ограничение]`). |
| **Explicit format** | Never add “Here is the JSON:” – output must start directly with the contract. |
| **Versioning** | Store prompts in a `prompts/` directory, name them `v01_<purpose>.md`. |
| **Ясность** | Используйте простой, однозначный язык |
| **Краткость** | Держите системные промпты ≤ 30 токенов, пользовательские ≤ 150 токенов, когда возможно |
| **Последовательность** | Переиспользуйте одну и ту же формулу роли (`Ты — [роль] + [аудитория] + [ограничение]`) |
| **Явный формат** | Никогда не добавляйте «Вот JSON:» – вывод должен начинаться сразу с контракта |
| **Версионирование** | Храните промпты в директории `prompts/`, называйте их `v01_<назначение>.md` |

### Соглашения об именовании

- **System prompts** → `system_<module>.md`  
- **User prompts / examples** → `example_<scenario>.md`  
- **Role definitions** → `role_<name>.md`
- **Системные промпты** → `system_<модуль>.md`
- **Пользовательские промпты / примеры** → `example_<сценарий>.md`
- **Определения ролей** → `role_<имя>.md`
 **Системные промпты** → `system_<модуль>.md`
 **Пользовательские промпты / примеры** → `example_<сценарий>.md`
 **Определения ролей** → `role_<имя>.md`

### Правила форматирования

1. **Code blocks** – always use triple backticks with language tag (`json`, `python`).  
2. **JSON contracts** – keys must be `snake_case`, values typed (`string`, `int`, `float`).  
3. **Markdown tables** – use pipe (`|`) syntax, no extra spaces.
1. **Блоки кода** – всегда используйте тройные обратные кавычки с тегом языка (`json`, `python`)
2. **JSON‑контракты** – ключи в `snake_case`, значения типизированы (`string`, `int`, `float`)
3. **Markdown‑таблицы** – используйте синтаксис с вертикальной чертой (`|`), без лишних пробелов

## 4. Техники промпт‑инжиниринга

![Prompt Engineering Techniques Overview](images/lecture02_techniques_overview.png "Техники промпт‑инжиниринга: Few-Shot, CoT, Ask-Before-Answer, Self-Consistency, Self-Evaluation, RAG, Prompt-Chaining, Role Anchoring")

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
| **Few‑Shot / One‑Shot** | Калибрует модель на примерах ввода‑вывода | Включите 2‑5 пар `Input:` / `Output:` в начале промпта |
| **Chain‑of‑Thought (CoT)** | Просит модель рассуждать шаг за шагом | Добавьте `think step‑by‑step` или `Разбей задачу на шаги и объясни каждый` |
| **Ask‑Before‑Answer** | Модель задаёт уточняющие вопросы перед ответом | Включите `Ask questions for clarification before generating your answer` |
| **Self‑Consistency** | Запустите один промпт несколько раз, выберите самый частый ответ | Выполните 3‑5 запусков, затем агрегируйте (большинство голосов, медиана) |
| **Self‑Evaluation** | После генерации модель проверяет свой вывод | Добавьте `Please evaluate your answer and point out possible issues` |
| **Конструктивная критика** | Прямая просьба о честной критике | `Criticize my code` или `Provide a critical review of the solution` |
| **Эмоциональный промптинг** | Повышает воспринимаемую важность задачи | `This is very important for my career` или `I need a perfect answer` |
| **Retrieval‑Augmented Generation (RAG)** | Добавляет внешние документы в контекст | `Use the following excerpts …` и включите релевантный текст |
| **Prompt‑Chaining** | Разбивает большую задачу на последовательные шаги, передавая результат каждого в следующий | Формат: `Step 1 → … → Step N` |
| **Role Anchoring** | Поддерживает персону модели стабильной в длинных диалогах | Включите неизменяемый блок идентичности в системный промпт |
| **Few‑Shot / One‑Shot** | Калибрует модель на примерах ввода‑вывода | Включите 2‑5 пар `Input:` / `Output:` в начале промпта |
| **Chain‑of‑Thought (CoT)** | Просит модель рассуждать шаг за шагом | Добавьте `think step‑by‑step` или `Разбей задачу на шаги и объясни каждый` |
| **Ask‑Before‑Answer** | Модель задаёт уточняющие вопросы перед ответом | Включите `Ask questions for clarification before generating your answer` |
| **Self‑Consistency** | Запустите один промпт несколько раз, выберите самый частый ответ | Выполните 3‑5 запусков, затем агрегируйте (большинство голосов, медиана) |
| **Self‑Evaluation** | После генерации модель проверяет свой вывод | Добавьте `Please evaluate your answer and point out possible issues` |
| **Конструктивная критика** | Прямая просьба о честной критике | `Criticize my code` или `Provide a critical review of the solution` |
| **Эмоциональный промптинг** | Повышает воспринимаемую важность задачи | `This is very important for my career` или `I need a perfect answer` |
| **Retrieval‑Augmented Generation (RAG)** | Добавляет внешние документы в контекст | `Use the following excerpts …` и включите релевантный текст |
| **Prompt‑Chaining** | Разбивает большую задачу на последовательные шаги, передавая результат каждого в следующий | Формат: `Step 1 → … → Step N` |
| **Role Anchoring** | Поддерживает персону модели стабильной в длинных диалогах | Включите неизменяемый блок идентичности в системный промпт |
| **Few‑Shot / One‑Shot** | Калибрует модель на примерах ввода‑вывода | Включите 2‑5 пар `Input:` / `Output:` в начале промпта |
| **Chain‑of‑Thought (CoT)** | Просит модель рассуждать шаг за шагом | Добавьте `think step‑by‑step` или `Разбей задачу на шаги и объясни каждый` |
| **Ask‑Before‑Answer** | Модель задаёт уточняющие вопросы перед ответом | Включите `Ask questions for clarification before generating your answer` |
| **Self‑Consistency** | Запустите один промпт несколько раз, выберите самый частый ответ | Выполните 3‑5 запусков, затем агрегируйте (большинство голосов, медиана) |
| **Self‑Evaluation** | После генерации модель проверяет свой вывод | Добавьте `Please evaluate your answer and point out possible issues` |
| **Конструктивная критика** | Прямая просьба о честной критике | `Criticize my code` или `Provide a critical review of the solution` |
| **Эмоциональный промптинг** | Повышает воспринимаемую важность задачи | `This is very important for my career` или `I need a perfect answer` |
| **Retrieval‑Augmented Generation (RAG)** | Добавляет внешние документы в контекст | `Use the following excerpts …` и включите релевантный текст |
| **Prompt‑Chaining** | Разбивает большую задачу на последовательные шаги, передавая результат каждого в следующий | Формат: `Step 1 → … → Step N` |
| **Role Anchoring** | Поддерживает персону модели стабильной в длинных диалогах | Включите неизменяемый блок идентичности в системный промпт |
| **Few‑Shot / One‑Shot** | Калибрует модель на примерах ввода‑вывода | Включите 2‑5 пар `Input:` / `Output:` в начале промпта |
| **Chain‑of‑Thought (CoT)** | Просит модель рассуждать шаг за шагом | Добавьте `think step‑by‑step` или `Разбей задачу на шаги и объясни каждый` |
| **Ask‑Before‑Answer** | Модель задаёт уточняющие вопросы перед ответом | Включите `Ask questions for clarification before generating your answer` |
| **Self‑Consistency** | Запустите один промпт несколько раз, выберите самый частый ответ | Выполните 3‑5 запусков, затем агрегируйте (большинство голосов, медиана) |
| **Self‑Evaluation** | После генерации модель проверяет свой вывод | Добавьте `Please evaluate your answer and point out possible issues` |
| **Конструктивная критика** | Прямая просьба о честной критике | `Criticize my code` или `Provide a critical review of the solution` |
| **Эмоциональный промптинг** | Повышает воспринимаемую важность задачи | `This is very important for my career` или `I need a perfect answer` |
| **Retrieval‑Augmented Generation (RAG)** | Добавляет внешние документы в контекст | `Use the following excerpts …` и включите релевантный текст |
| **Prompt‑Chaining** | Разбивает большую задачу на последовательные шаги, передавая результат каждого в следующий | Формат: `Step 1 → … → Step N` |
| **Role Anchoring** | Поддерживает персону модели стабильной в длинных диалогах | Включите неизменяемый блок идентичности в системный промпт |

### Комбинирование техник

![Technique Combinations](images/lecture02_technique_combinations.png "Комбинирование техник: Few-Shot + CoT, Ask-Before-Answer + Self-Evaluation, RAG + Prompt-Chaining")

1. **Few‑Shot + CoT** – show a few‑shot example that also demonstrates step‑by‑step reasoning.  
2. **Ask‑Before‑Answer + Self‑Evaluation** – first clarify, then evaluate the final answer.  
3. **RAG + Prompt‑Chaining** – each chain step can retrieve additional context before generation.
1. **Few‑Shot + CoT** – покажите few‑shot пример, который также демонстрирует пошаговое рассуждение
2. **Ask‑Before‑Answer + Self‑Evaluation** – сначала уточните, затем оцените итоговый ответ
3. **RAG + Prompt‑Chaining** – каждый шаг цепочки может извлекать дополнительный контекст перед генерацией

## 5. Практический пример промпта (русский системный промпт, английский запрос пользователя)

```
System:
Ты — senior backend‑разработчик. Твоя задача — отвечать в формате JSON без лишних слов.

User:
Input: Напиши функцию, которая принимает список чисел и возвращает их среднее.
```


```json
{
  "function_name": "mean",
  "code": "def mean(nums):\n    return sum(nums) / len(nums)",
  "description": "Вычисляет среднее арифметическое"
}
```

---  






*С этими принципами, шаблонами и техниками вы можете создавать надёжные, парсимые промпты для широкого спектра задач.*


![Technique Combinations](images/lecture02_technique_combinations.png "Комбинирование техник: Few-Shot + CoT, Ask-Before-Answer + Self-Evaluation, RAG + Prompt-Chaining")

