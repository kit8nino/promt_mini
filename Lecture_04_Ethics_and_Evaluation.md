# Lecture 04 – Ethics, Safety, and Evaluation of LLM Outputs  

## 1. Ethics and Safety in Generative AI  

### 1.1 Why ethics matters  

Large language models are powerful but can cause real‑world harm if misused.  
- **Bias** – models inherit societal biases present in training data.  
- **Privacy** – they may regurgitate personally identifiable information (PII).  
- **Misinformation** – hallucinations can spread false facts.  
- **Security** – prompts can be crafted to produce malicious code or instructions.  

### 1.2 Types of risk  

| Risk category | Description | Example |
|--------------|-------------|---------|
| **Bias & Fairness** | Systematic prejudice against protected groups. | Model consistently associates certain jobs with a gender. |
| **Privacy Leakage** | Generation of user‑specific data that was present in training corpora. | Outputting a real email address that appeared in a public forum. |
| **Hallucination / Misinformation** | Confidently stating false statements. | Claiming a nonexistent scientific law. |
| **Malicious Use** | Generating phishing emails, code exploits, or disallowed content. | Producing a script that bypasses authentication. |
| **Environmental Impact** | Excessive compute for fine‑tuning or inference. | Running large batch jobs without cost/energy awareness. |

### 1.3 Mitigation techniques  

1. **Dataset curation** – filter out hateful, toxic, or PII‑containing text.  
2. **Bias testing** – use benchmark suites (e.g., `StereoSet`, `BiasBench`) and report metrics.  
3. **Prompt safeguards** – prepend system prompts that forbid disallowed content.  
4. **Output filtering** – post‑process with classifiers (toxicity, PII detectors).  
5. **Human‑in‑the‑loop review** – especially for high‑risk domains (medical, legal).  
6. **Rate limiting & usage monitoring** – detect abuse patterns.  
7. **Model distillation** – smaller, purpose‑trained models reduce attack surface.  

### 1.4 Practical checklist for developers  

- [ ] Have you added a **system prompt** that explicitly bans illegal or harmful content?  
- [ ] Do you run **automated tests** for bias (e.g., gendered occupation prompts)?  
- [ ] Is **PII detection** enabled on model outputs before storage or display?  
- [ ] Are **rate limits** configured on your API endpoints?  
- [ ] Have you documented **fallback behavior** when the model refuses to answer?  
- [ ] Do you log **prompt‑output pairs** for auditability (respecting privacy)?  

## 2. Evaluation and Self‑Critique  

### 2.1 Why evaluation matters  

Even with a perfect prompt, LLMs can produce **hallucinations**, syntax errors, or sub‑optimal solutions. Systematic evaluation helps you:

- Detect factual errors.  
- Verify that the output conforms to the required format (JSON, Markdown, etc.).  
- Improve the prompt iteratively.  

### 2.2 Built‑in evaluation techniques  

| Technique | How it works | When to use |
|-----------|--------------|-------------|
| **Self‑Evaluation** | After generating the answer, ask the model to review its own output. | Quick sanity check, especially for code or JSON. |
| **Critic Prompt** | Provide a second prompt that asks the model to critique the previous answer. | When you need a deeper analysis (security review, performance). |
| **Unit‑Test Generation** | Ask the model to generate tests for its own code and run them. | For code generation tasks. |
| **Schema Validation** | Validate JSON against a schema (e.g., using `jsonschema` in Python). | Whenever JSON is the contract. |
| **Metric‑based scoring** | Compute BLEU, ROUGE, or custom similarity scores against a reference. | For natural‑language generation tasks. |

### 2.3 Self‑Evaluation prompt pattern  

```
Assistant: <generated answer>  

Please evaluate your answer:
- Is the JSON syntactically valid?
- Does it contain all required fields?
- Are there any factual inaccuracies?
Provide a short verdict (✅ or ❌) and, if ❌, list the problems.
```  

### 2.4 Automated evaluation pipeline (pseudo‑code)

```python
def generate_and_check(prompt):
    answer = llm.complete(prompt)
    eval_prompt = f"{answer}\n\nPlease evaluate your answer and return a JSON verdict."
    verdict = llm.complete(eval_prompt)
    return answer, verdict
```

Integrate this function into CI/CD pipelines for generated code.

### 2.5 Best practices  

- **Never trust the first output** – always run at least one evaluation step.  
- **Separate evaluation from generation** – keep the two prompts distinct to avoid the model “double‑checking” its own mistakes.  
- **Log both the answer and the evaluation** for later analysis.  

---  

*By combining rigorous ethical safeguards with systematic evaluation, you can deploy LLM‑powered systems that are both useful and responsible.*  

![AI Ethics Risk Categories](images/lecture04_ethics_risks.png "AI Ethics Risk Categories: Bias & Fairness, Privacy Leakage, Hallucination, Malicious Use, Environmental Impact")


![Risk Categories Detail](images/lecture04_risk_categories.png "Risk Categories with Examples: Bias (gendered jobs), Privacy (PII), Hallucination (false facts), Malicious (exploits), Environmental (compute)")


![Mitigation Techniques](images/lecture04_mitigation_techniques.png "Mitigation Techniques: Dataset Curation, Bias Testing, Prompt Safeguards, Output Filtering, Human-in-the-loop, Rate Limiting, Model Distillation")


![Evaluation Pipeline](images/lecture04_evaluation_pipeline.png "Evaluation Pipeline: Generate → Self-Evaluate / Critic Prompt / Unit Tests / Schema Validation → Verdict → Log")


![Evaluation Techniques](images/lecture04_evaluation_techniques.png "Evaluation Techniques: Self-Evaluation, Critic Prompt, Unit-Test Generation, Schema Validation, Metric-based Scoring")


![Automated Evaluation Flow](images/lecture04_automated_evaluation_flow.png "Automated Evaluation Flow: Generate → Evaluate → Verdict → CI/CD Integration")


![AI Ethics Risk Categories](images/lecture04_ethics_risks.png "AI Ethics Risk Categories: Bias & Fairness, Privacy Leakage, Hallucination, Malicious Use, Environmental Impact")


![Risk Categories Detail](images/lecture04_risk_categories.png "Risk Categories with Examples: Bias (gendered jobs), Privacy (PII), Hallucination (false facts), Malicious (exploits), Environmental (compute)")


![Mitigation Techniques](images/lecture04_mitigation_techniques.png "Mitigation Techniques: Dataset Curation, Bias Testing, Prompt Safeguards, Output Filtering, Human-in-the-loop, Rate Limiting, Model Distillation")


![Evaluation Pipeline](images/lecture04_evaluation_pipeline.png "Evaluation Pipeline: Generate → Self-Evaluate / Critic Prompt / Unit Tests / Schema Validation → Verdict → Log")


![Evaluation Techniques](images/lecture04_evaluation_techniques.png "Evaluation Techniques: Self-Evaluation, Critic Prompt, Unit-Test Generation, Schema Validation, Metric-based Scoring")


![Automated Evaluation Flow](images/lecture04_automated_evaluation_flow.png "Automated Evaluation Flow: Generate → Evaluate → Verdict → CI/CD Integration")


![AI Ethics Risk Categories](images/lecture04_ethics_risks.png "AI Ethics Risk Categories: Bias & Fairness, Privacy Leakage, Hallucination, Malicious Use, Environmental Impact")


![Risk Categories Detail](images/lecture04_risk_categories.png "Risk Categories with Examples: Bias (gendered jobs), Privacy (PII), Hallucination (false facts), Malicious (exploits), Environmental (compute)")


![Mitigation Techniques](images/lecture04_mitigation_techniques.png "Mitigation Techniques: Dataset Curation, Bias Testing, Prompt Safeguards, Output Filtering, Human-in-the-loop, Rate Limiting, Model Distillation")


![Evaluation Pipeline](images/lecture04_evaluation_pipeline.png "Evaluation Pipeline: Generate → Self-Evaluate / Critic Prompt / Unit Tests / Schema Validation → Verdict → Log")


![Evaluation Techniques](images/lecture04_evaluation_techniques.png "Evaluation Techniques: Self-Evaluation, Critic Prompt, Unit-Test Generation, Schema Validation, Metric-based Scoring")


![Automated Evaluation Flow](images/lecture04_automated_evaluation_flow.png "Automated Evaluation Flow: Generate → Evaluate → Verdict → CI/CD Integration")

