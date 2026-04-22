# Assignments for CS146S: The Modern Software Developer

This is the home of the assignments for [CS146S: The Modern Software Developer](https://themodernsoftware.dev), taught at Stanford University fall 2025.

## Repo Setup
These steps work with Python 3.12.

1. Install Anaconda
   - Download and install: [Anaconda Individual Edition](https://www.anaconda.com/download)
   - Open a new terminal so `conda` is on your `PATH`.

2. Create and activate a Conda environment (Python 3.12)
   ```bash
   conda create -n cs146s python=3.12 -y
   conda activate cs146s
   ```

3. Install Poetry
   ```bash
   curl -sSL https://install.python-poetry.org | python -
   ```

4. Install project dependencies with Poetry (inside the activated Conda env)
   From the **repository root**: [^1]
   
   ```bash
   poetry install --no-interaction
   ```

[^1]: The `poetry install` command is used to install the dependencies of a Python project that are listed in the `pyproject.toml` file.



# Progress Checklist

## Week 1: Introduction to Coding LLMs and AI Development

**Topics**

- Course logistics
- What is an LLM actually
- How to prompt effectively

**Reading**

- [ ] [Deep Dive into LLMs](https://www.youtube.com/watch?v=7xTGNNLPyMI)
- [ ] [Prompt Engineering Overview](https://cloud.google.com/discover/what-is-prompt-engineering)
- [ ] [Prompt Engineering Guide](https://www.promptingguide.ai/techniques)
- [ ] [AI Prompt Engineering: A Deep Dive](https://www.youtube.com/watch?v=T9aRN5JkmL8)
- [ ] [How OpenAI Uses Codex](https://cdn.openai.com/pdf/6a2631dc-783e-479b-b1a4-af0cfbd38630/how-openai-uses-codex.pdf)

**Assignment**

- [ ] [LLM Prompting Playground](https://github.com/mihail911/modern-software-dev-assignments/tree/master/week1)

**Slides**

- [ ] **Mon 9/22:** Introduction and how an LLM is made - [Slides](https://docs.google.com/presentation/d/1zT2Ofy88cajLTLkd7TcuSM4BCELvF9qQdHmlz33i4t0/edit?usp=sharing)
- [ ] **Fri 9/26:** Power prompting for LLMs - [Slides](https://docs.google.com/presentation/d/1MIhw8p6TLGdbQ9TcxhXSs5BaPf5d_h77QY70RHNfeGs/edit?usp=drive_link)



## Week 2: The Anatomy of Coding Agents

**Topics**

- Agent architecture and components



