# Prompt Engineering Course

## Overview

This repository contains a complete, self‑contained curriculum for learning prompt engineering with large language models (LLMs).  
It includes theory lectures, practical labs (Jupyter notebooks), a prompt style guide, and reference material.  
All content is written in a mix of Russian and English to reflect the original course material.

## Table of Contents

### Lectures

- [Lecture 01 – Introduction, Architecture, and Context](Lecture_01_Intro_and_Architecture.md)
- [Lecture 02 – Prompt Engineering: Principles, Templates, Style Guide, and Techniques](Lecture_02_Prompt_Engineering.md)
- [Lecture 03 – Task Decomposition, Prompt Chaining, and Retrieval‑Augmented Generation (RAG)](Lecture_03_Decomposition_and_RAG.md)
- [Lecture 04 – Ethics, Safety, and Evaluation of LLM Outputs](Lecture_04_Ethics_and_Evaluation.md)

### Labs

- [Lab 0: Intro to Generative AI](Lab_0_Intro_to_Generative_AI.ipynb) — first API call and token usage
- [Lab 1: Tokenization and Context Window](Lab_1_Tokenization_and_Context.ipynb) — tokenization, context limits, sliding window
- [Lab 2: Build Your First Prompt](Lab_2_Build_Your_First_Prompt.ipynb) — three‑component template, few‑shot, step‑by‑step
- [Lab 3: Task Decomposition and Prompt Chaining](Lab_3_Task_Decomposition.ipynb) — atomic steps and JSON validation
- [Lab 4: Retrieval‑Augmented Generation (RAG) Implementation](Lab_4_RAG_Implementation.ipynb) — embeddings, FAISS, retrieval‑augmented prompt
- [Lab 5: Ethics & Safety Reflection](Lab_5_Ethics_Reflection.md) — written reflection, no code required

### Reference

- [Style Guide for Prompts](Style_Guide_for_Prompts.md)
- [Appendix: Role Anchors and Persona Techniques](Appendix_Role_Anchors.md)
- [Course Summary and Next Steps](Course_Summary_and_Next_Steps.md)

## Repository Layout

```
.
├── README.md
├── Lecture_01_Intro_and_Architecture.md
├── Lecture_02_Prompt_Engineering.md
├── Lecture_03_Decomposition_and_RAG.md
├── Lecture_04_Ethics_and_Evaluation.md
├── Lab_0_Intro_to_Generative_AI.ipynb
├── Lab_1_Tokenization_and_Context.ipynb
├── Lab_2_Build_Your_First_Prompt.ipynb
├── Lab_3_Task_Decomposition.ipynb
├── Lab_4_RAG_Implementation.ipynb
├── Lab_5_Ethics_Reflection.md
├── Style_Guide_for_Prompts.md
├── Appendix_Role_Anchors.md
└── Course_Summary_and_Next_Steps.md
```

## Getting Started

1. **Clone the repository**
   ```bash
   git clone https://github.com/kit8nino/promt_mini.git
   cd promt_mini
   ```

2. **Set up the Python environment**
   ```bash
   python -m venv venv
   source venv/bin/activate   # on Windows: venv\Scripts\activate
   pip install openai tiktoken matplotlib numpy sentence-transformers faiss-cpu jupyterlab
   ```

3. **Add your LLM API key**  
   Export the key as an environment variable:
   ```bash
   export OPENAI_API_KEY=sk-...   # Linux/macOS
   set OPENAI_API_KEY=sk-...      # Windows CMD
   ```

4. **Run the notebooks**  
   Launch Jupyter Lab / Notebook:
   ```bash
   jupyter lab
   ```
   Open any `Lab_*.ipynb` file and execute the cells sequentially. Lab 5 is a Markdown reflection task and does not require running code.

5. **Read the lectures**  
   Open the `.md` files in any Markdown viewer or directly on GitHub.

## Contributing

- Fork the repository and create a new branch for your changes.
- Follow the [Style Guide for Prompts](Style_Guide_for_Prompts.md) when adding or modifying prompt examples.
- Keep notebooks tidy: clear output cells before committing.
- Update the [Course Summary](Course_Summary_and_Next_Steps.md) if you add new material.

## License

This material is provided under the MIT License.
