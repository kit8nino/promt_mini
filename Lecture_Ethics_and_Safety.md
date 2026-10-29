# Lecture: Ethics and Safety in Generative AI  

## 1. Why ethics matters  

Large language models are powerful but can cause real‑world harm if misused.  
- **Bias** – models inherit societal biases present in training data.  
- **Privacy** – they may regurgitate personally identifiable information (PII).  
- **Misinformation** – hallucinations can spread false facts.  
- **Security** – prompts can be crafted to produce malicious code or instructions.  

Understanding these risks is the first step toward responsible AI development.  

## 2. Types of risk  

| Risk category | Description | Example |
|--------------|-------------|---------|
| **Bias & Fairness** | Systematic prejudice against protected groups. | Model consistently associates certain jobs with a gender. |
| **Privacy Leakage** | Generation of user‑specific data that was present in training corpora. | Outputting a real email address that appeared in a public forum. |
| **Hallucination / Misinformation** | Confidently stating false statements. | Claiming a nonexistent scientific law. |
| **Malicious Use** | Generating phishing emails, code exploits, or disallowed content. | Producing a script that bypasses authentication. |
| **Environmental Impact** | Excessive compute for fine‑tuning or inference. | Running large batch jobs without cost/energy awareness. |

## 3. Mitigation techniques  

1. **Dataset curation** – filter out hateful, toxic, or PII‑containing text.  
2. **Bias testing** – use benchmark suites (e.g., `StereoSet`, `BiasBench`) and report metrics.  
3. **Prompt safeguards** – prepend system prompts that forbid disallowed content.  
4. **Output filtering** – post‑process with classifiers (toxicity, PII detectors).  
5. **Human‑in‑the‑loop review** – especially for high‑risk domains (medical, legal).  
6. **Rate limiting & usage monitoring** – detect abuse patterns.  
7. **Model distillation** – smaller, purpose‑trained models reduce attack surface.  

## 4. Practical checklist for developers  

- [ ] Have you added a **system prompt** that explicitly bans illegal or harmful content?  
- [ ] Do you run **automated tests** for bias (e.g., gendered occupation prompts)?  
- [ ] Is **PII detection** enabled on model outputs before storage or display?  
- [ ] Are **rate limits** configured on your API endpoints?  
- [ ] Have you documented **fallback behavior** when the model refuses to answer?  
- [ ] Do you log **prompt‑output pairs** for auditability (respecting privacy)?  

## 5. Further reading  

1. **“Responsible AI Practices”** – Google AI Blog.  
2. **“AI Ethics Guidelines Global Inventory”** – UNESCO.  
3. **“The Red Teaming Book”** – OpenAI (2024).  
4. **“Evaluating and Reducing Gender Bias in Language Models”** – ACL 2023.  
5. **“Privacy‑Preserving Language Modeling”** – arXiv 2024.  

---  

*Ethical AI is a continuous process: iterate, monitor, and improve.*  

## Wrap‑up

This lecture highlighted the key risk categories associated with generative AI and presented concrete mitigation strategies. By integrating these practices into your development workflow, you help ensure that AI systems are safe, fair, and trustworthy.  
