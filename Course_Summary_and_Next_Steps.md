# Course Summary and Next Steps  

## 1. What we covered  

| Module | Key take‑aways |
|--------|---------------|
| **Lecture 0** | High‑level view of Generative AI, core concepts, limitations. |
| **Lecture 1** | Architecture of LLMs: tokens, attention, context window, hallucinations. |
| **Lecture 1b** | Interaction of system prompt, user prompt, and context window. |
| **Lecture 2** | Prompt‑engineering intro: three‑component template, role formula, few‑shot, good vs. bad prompts. |
| **Lecture 3** | Prompt‑engineering techniques (few‑shot, CoT, RAG, chaining, etc.). |
| **Lecture 4** | Retrieval‑Augmented Generation (RAG) basics and implementation sketch. |
| **Lecture 5** | Task decomposition, prompt chaining, atomic sub‑tasks. |
| **Lecture 6** | Ethics and safety considerations, risk categories, mitigation checklist. |
| **Lecture 7** | Evaluation, self‑critique, automated pipelines, schema validation. |
| **Lab 0** | Intro to Generative AI – simple API call and token usage. |
| **Lab 1** | Tokenization, context window limits, visualisation. |
| **Lab 2** | Build your first prompt with three‑component template and few‑shot. |
| **Lab 3** | Task decomposition and prompt chaining in a notebook. |
| **Lab 4** | Retrieval‑Augmented Generation – indexing, retrieval, prompt construction. |
| **Lab 5** | Ethics reflection – bias, privacy, hallucination, malicious‑use safeguards. |

## 2. Practical labs recap  

| Lab | Goal |
|-----|------|
| **Lab 0** | Make a single API call, print response, view token usage. |
| **Lab 1** | Inspect tokenization, measure context length, visualise sliding window. |
| **Lab 2** | Construct a prompt using the three‑component template, add few‑shot, request step‑by‑step answer. |
| **Lab 3** | Decompose a complex request, implement prompt chaining, validate JSON output. |
| **Lab 4** | Implement a basic RAG pipeline with FAISS and OpenAI API. |
| **Lab 5** | Reflect on ethical considerations and write mitigation strategies. |

## 3. Next steps for learners  

1. **Fine‑tuning / Instruction‑tuning** – learn how to adapt a base model to a specific domain.  
2. **Agent frameworks** – explore LangChain, LlamaIndex, or Auto‑GPT for building multi‑step agents.  
3. **Production deployment** – containerisation, scaling, monitoring hallucinations, cost optimisation.  
4. **Advanced evaluation** – use human‑in‑the‑loop, A/B testing, and automated metrics (BLEU, ROUGE, CodeBLEU).  
5. **Career paths** – roles such as Prompt Engineer, LLM‑Ops Engineer, AI Product Manager, and Research Engineer.  

## 4. Recommended resources  

- **“The Prompt Report”** (arXiv, 2025) – deep dive into taxonomy of techniques.  
- **LangChain Documentation** – building LLM‑driven applications.  
- **OpenAI Cookbook** – practical recipes for prompt engineering, RAG, and evaluation.  
- **“Responsible AI Practices”** – Google AI Blog.  
- **“AI Ethics Guidelines Global Inventory”** – UNESCO.  

## 5. Closing thoughts  

Prompt engineering is a **human‑in‑the‑loop** discipline: the better you can formalise a task, the more reliable the model’s output becomes. Treat prompts as **code** – version them, test them, and refactor them.  

*Good luck building the next generation of AI‑augmented software!*  
