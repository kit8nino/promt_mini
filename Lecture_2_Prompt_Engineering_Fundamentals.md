# Lecture 2 – Prompt Engineering: Foundations, Techniques, and Evaluation

## 1. What Is Prompt Engineering?
Prompt engineering is the practice of crafting inputs (prompts) that guide large language models (LLMs) to produce desired outputs. It blends linguistic intuition with an understanding of model behavior.

## 2. Core Principles (from the original introductions)
- **Clarity** – Use explicit language; avoid ambiguous phrasing.  
- **Specificity** – Provide concrete details, examples, or formats.  
- **Few‑Shot Demonstrations** – Include a few input‑output pairs to set the pattern.  
- **Role‑Playing** – Assign a persona or role to the model (e.g., “You are a helpful tutor”).

## 3. Prompt Types
| Type | Description | Example |
|------|-------------|---------|
| **Zero‑Shot** | Direct question without examples. | “Explain quantum entanglement.” |
| **One‑Shot** | One example provided. | “Q: What is photosynthesis? A: …” |
| **Few‑Shot** | Multiple examples to establish a pattern. | See Section 4. |
| **Chain‑of‑Thought** | Encourage step‑by‑step reasoning. | “Let’s think step by step…” |
| **Instruction‑Based** | Explicitly tell the model what to do. | “Summarize the following article in 3 bullet points.” |

## 4. Common Techniques
### 4.1 Few‑Shot Prompting
