# Prompt Engineering Course

## Overview
This repository contains a complete, self‑contained curriculum for learning prompt engineering with large language models (LLMs).  
It includes theory lectures, practical labs (Jupyter notebooks), style guides, and reference material.  
All content is written in a mix of Russian and English to reflect the original course material.

## Table of Contents

### Lectures
- [Lecture 0: Introduction to Generative AI](Lecture_0_Intro_to_Generative_AI.md)
- [Lecture 1: LLM Architecture](Lecture_1_LLM_Architecture.md)
- [Lecture 1b: Context and Prompt Interaction](Lecture_1b_Context_and_Prompt_Interaction.md)
- [Lecture 2: Prompt Engineering Introduction](Lecture_Prompt_Engineering_Intro.md)
- [Lecture 3: Task Decomposition](Lecture_Task_Decomposition.md)
- [Lecture 4: Evaluation and Self‑Critique](Lecture_Evaluation_and_Self_Critique.md)
- [Lecture 5: Retrieval‑Augmented Generation (RAG)](Lecture_RAG_and_External_Knowledge.md)
- [Lecture 6: Ethics and Safety](Lecture_Ethics_and_Safety.md)

### Labs
- [Lab 0: Intro to Generative AI](Lab_0_Intro_to_Generative_AI.ipynb)
- [Lab 1: Tokenization and Context Window](Lab_1_Tokenization_and_Context.ipynb)
- [Lab 2: Build Your First Prompt](Lab_2_Build_Your_First_Prompt.ipynb)
- [Lab 3: Task Decomposition and Prompt Chaining](Lab_3_Task_Decomposition.ipynb)

### Course Summary
- [Course Summary and Next Steps](Course_Summary_and_Next_Steps.md)

## Getting Started

1. **Clone the repository**  
   ```bash
   git clone https://github.com/your-org/prompt-engineering-course.git
   cd prompt-engineering-course
   ```

2. **Set up the Python environment**  
   ```bash
   python -m venv venv
   source venv/bin/activate   # on Windows: venv\\Scripts\\activate
   pip install -r requirements.txt
   ```
   *If `requirements.txt` is not present, install the basics:*  
   ```bash
   pip install openai tiktoken matplotlib
   ```

3. **Add your LLM API key**  
   Export the key as an environment variable:  
   ```bash
   export OPENAI_API_KEY=sk-...   # Linux/macOS
   set OPENAI_API_KEY=sk-...    # Windows CMD
   ```

4. **Run the notebooks**  
   Launch Jupyter Lab / Notebook:  
   ```bash
   jupyter lab
   ```
   Open any `Lab_*.ipynb` file and execute the cells sequentially.

5. **Read the lectures**  
   Open the `.md` files in any markdown viewer or directly on GitHub.

## Contributing

- Fork the repository and create a new branch for your changes.  
- Follow the **Style Guide for Prompts** when adding or modifying prompt examples.  
- Keep notebooks tidy: clear output cells before committing.  
- Update the **Course Summary** if you add new material.

## License

This material is provided under the MIT License. See `LICENSE` for details.

---  

*Happy prompting!*  
