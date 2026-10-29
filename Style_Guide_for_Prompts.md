# Style Guide for Prompt Writing  

## 1. General principles  

| Principle | Description |
|----------|-------------|
| **Clarity** | Use simple, unambiguous language. Avoid idioms that can be interpreted in multiple ways. |
| **Brevity** | Every token costs context. Keep system prompts ≤ 30 tokens, user prompts ≤ 150 tokens when possible. |
| **Consistency** | Re‑use the same role formula (`Ты — [роль] + [аудитория] + [ограничение]`) across all prompts. |
| **Explicit format** | Never add “Here is the JSON:” – output must start directly with the contract. |
| **Versioning** | Store prompts in a `prompts/` directory, name them `v01_<purpose>.md`. Include a short changelog at the top. |

## 2. Naming conventions  

- **System prompts** → `system_<module>.md`  
- **User prompts / examples** → `example_<scenario>.md`  
- **Role definitions** → `role_<name>.md`  

## 3. Formatting rules  

1. **Code blocks** – always use triple backticks with language tag (`json`, `python`).  
2. **JSON contracts** – keys must be snake_case, values typed (`string`, `int`, `float`).  
3. **Markdown tables** – use pipe (`|`) syntax, no extra spaces.  

### Example of a well‑formatted prompt  

```markdown
System:
Ты — senior backend‑разработчик. Твоя задача — отвечать в формате JSON без лишних слов.

User:
Input: Напиши функцию, которая принимает список чисел и возвращает их среднее.
```json
{
  "function_name": "mean",
  "code": "def mean(nums):\n    return sum(nums) / len(nums)",
  "description": "Вычисляет среднее арифметическое"
}
```  

## 4. Common pitfalls to avoid  

| Pitfall | Why it’s bad | Fix |
|---------|--------------|-----|
| Adding “Here is the JSON:” before the object | Breaks parsers that expect raw JSON. | Start directly with `{`. |
| Mixing languages in the same prompt (e.g., Russian system prompt, English user prompt) without clear separation | Confuses tokenisation and may lead to language‑mix errors. | Keep language consistent or explicitly state language switch. |
| Over‑loading the system prompt with many rules | Consumes context, model may ignore later rules. | Keep system prompt short; move extra rules to the user prompt or examples. |

## 5. Review checklist (before committing a prompt)  

- [ ] Role formula present and correct.  
- [ ] Task expressed as a single verb.  
- [ ] Output format defined and adhered to.  
- [ ] No extra explanatory text before the contract.  
- [ ] Token count ≤ 75 % of model’s context window.  

---  

*Follow this guide to keep your prompts clean, maintainable, and production‑ready.*  
