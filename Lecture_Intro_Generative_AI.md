# Lecture: Introduction to Generative AI (Merged)

<!-- merged from Lecture_0_Intro_to_Generative_AI.md -->
## 1. What is Generative AI?  

Generative AI – a class of models that can create new content (text, code, images, audio) from a prompt. The most common type today is the **large language model (LLM)**, which predicts the next token in a sequence of tokens.  

## 2. Why does it matter for developers?  

- **Code assistance** – tools like GitHub Copilot, CodeLlama, StarCoder.  
- **Documentation & summarisation** – automatically generate API docs, release notes.  
- **Idea generation** – brainstorm architectures, write tests, create UI copy.  

## 3. Core concepts (high‑level)  

| Concept | Short description |
|--------|-------------------|
| **Token** | Smallest unit the model works with (word piece, sub‑word, punctuation). |
| **Context window** | Number of recent tokens the model can attend to (e.g., 4 096 tokens). |
| **Prompt** | The text you give the model – it can contain system instructions, user request, examples. |
| **Temperature / Top‑p** | Sampling parameters that control randomness vs. determinism. |
| **Fine‑tuning / Instruction‑tuning** | Adapting a base model to a specific domain or style. |

## 4. Example: From prompt to code  

```
User: Write a Python function that returns the factorial of a number.
Model: 
```python
def factorial(n):
    if n < 0:
        raise ValueError("Negative numbers are not allowed")
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result
```  
```

## 5. Limitations  

- **Hallucinations** – the model can produce plausible‑looking but incorrect statements.  
- **Context length** – information outside the window is forgotten.  
- **Biases** – outputs reflect biases present in training data.  

## 6. What you will learn in this course  

1. Architecture of LLMs (tokens, attention, context).  
2. Prompt engineering fundamentals (context, task, format).  
3. Advanced techniques (few‑shot, CoT, RAG, chaining).  
4. Task decomposition and building AI‑agents.  
5. Evaluation, self‑critique, and production‑ready pipelines.  

---  

### Related Labs
- **Lab 0** – Intro to Generative AI (`Lab_0_Intro_to_Generative_AI.ipynb`)

---  

*Proceed to Lecture 1: LLM Architecture.*  
