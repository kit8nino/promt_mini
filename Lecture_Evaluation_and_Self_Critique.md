# Lecture: Evaluation and Self‑Critique of LLM Outputs  

## 1. Why evaluation matters  

Even with a perfect prompt, LLMs can produce **hallucinations**, syntax errors, or sub‑optimal solutions. Systematic evaluation helps you:  

- Detect factual errors.  
- Verify that the output conforms to the required format (JSON, Markdown, etc.).  
- Improve the prompt iteratively.  

## 2. Built‑in evaluation techniques  

| Technique | How it works | When to use |
|-----------|--------------|-------------|
| **Self‑Evaluation** | After generating the answer, ask the model to review its own output. | Quick sanity check, especially for code or JSON. |
| **Critic Prompt** | Provide a second prompt that asks the model to critique the previous answer. | When you need a deeper analysis (security review, performance). |
| **Unit‑Test Generation** | Ask the model to generate tests for its own code and run them. | For code generation tasks. |
| **Schema Validation** | Validate JSON against a schema (e.g., using `jsonschema` in Python). | Whenever JSON is the contract. |
| **Metric‑based scoring** | Compute BLEU, ROUGE, or custom similarity scores against a reference. | For natural‑language generation tasks. |

## 3. Self‑Evaluation prompt pattern  

```
Assistant: <generated answer>  

Please evaluate your answer:
- Is the JSON syntactically valid?
- Does it contain all required fields?
- Are there any factual inaccuracies?
Provide a short verdict (✅ or ❌) and, if ❌, list the problems.
```  

### Example  

```
{
  "name": "Alice",
  "age": "twenty five"
}
```  

**Self‑Evaluation:**  

- ❌ JSON is syntactically valid, but the `age` field should be an integer, not a string.  

## 4. Constructive Criticism  

Ask the model to **critique** without being overly polite, which often yields more direct feedback:  

```
Criticize my code and list all potential bugs or inefficiencies.
```  

## 5. Automated pipelines (pseudo‑code)  

```python
def generate_and_check(prompt):
    answer = llm.complete(prompt)
    eval_prompt = f"{answer}\n\nPlease evaluate your answer and return a JSON verdict."
    verdict = llm.complete(eval_prompt)
    return answer, verdict
```  

Integrate this function into your CI/CD pipeline for generated code.  

## 6. Best practices  

- **Never trust the first output** – always run at least one evaluation step.  
- **Separate evaluation from generation** – keep the two prompts distinct to avoid the model “double‑checking” its own mistakes.  
- **Log both the answer and the evaluation** for later analysis.  

---  

*Next lecture: Retrieval‑Augmented Generation (RAG) and External Knowledge.*  
