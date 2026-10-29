# Lecture 1b: Context, Prompt Interaction, and the System Prompt  

## 1. The role of the context window  

- The **context window** is the set of the most recent tokens that the model can attend to when generating the next token.  
- Modern LLMs typically have windows of **2 048 – 4 096 tokens**.  
- Anything that falls outside this window is **invisible** to the model and cannot influence the answer.  

### Why it matters for prompting  

| Situation | Effect if the information is inside the window | Effect if it is outside |
|-----------|-----------------------------------------------|------------------------|
| Long code base | Model can reference earlier definitions, variable names, imports. | Model “forgets” earlier definitions → broken code. |
| Multi‑turn conversation | Model remembers previous user messages and its own answers. | Model loses the thread, may repeat or contradict itself. |

## 2. System Prompt vs. User Prompt  

- **System prompt** (also called *system message* or *system instruction*) sets the **persona, role, and high‑level behavior** of the model.  
- **User prompt** contains the concrete request, data, or question.  

```
System: Ты — senior backend‑разработчик. Твоя задача — давать короткие, технически точные ответы без лишних пояснений.  
User: Как реализовать кеширование запросов в FastAPI?
```  

The system prompt is processed **once** at the beginning of the session and stays at the start of the token sequence, ensuring the model always has the role information in its context.  

## 3. Practical tips  

1. **Place the system prompt first** – it guarantees the role stays within the context window for the longest possible time.  
2. **Keep the system prompt concise** – every token counts; a 30‑token system prompt is usually enough.  
3. **Refresh the role** if you anticipate a very long interaction: re‑send a short reminder (`You are still the senior backend‑developer`).  

## 4. Example of a well‑structured interaction  

```
System: Ты — senior backend‑разработчик. Твоя задача — давать ответы в формате JSON, без лишних слов.  
User: Input: Напиши функцию, которая принимает список чисел и возвращает их сумму.  
Assistant: 
{
  "function_name": "sum_numbers",
  "code": "def sum_numbers(nums):\n    return sum(nums)",
  "description": "Суммирует список чисел"
}
```  

Notice how the **system prompt** defines the role and output format, while the **user prompt** supplies the concrete task.  

---  

*After this short bridge, continue with Lecture 2: Prompt Engineering Introduction.*  
