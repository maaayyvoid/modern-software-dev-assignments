# Week 1 — Prompting Techniques

You will practice multiple prompting techniques by crafting prompts to complete specific tasks. Each task’s instructions are at the top of its corresponding source file.

## Installation
Make sure you have first done the installation described in the top-level `README.md`. 

## Ollama installation
We will be using a tool to run different state-of-the-art LLMs locally on your machine called [Ollama](https://ollama.com/). Use one of the following methods:

- macOS (Homebrew):
  ```bash
  brew install --cask ollama 
  ollama serve
  ```

- Linux (recommended):
  ```bash
  curl -fsSL https://ollama.com/install.sh | sh
  ```

- Windows:
  Download and run the installer from [ollama.com/download](https://ollama.com/download).

Verify installation:
```bash
ollama -v
```

Before running the test scripts, make sure you have the following models pulled. You only need to do this once (unless you remove the models later): [^1]
```bash
ollama run mistral-nemo:12b
ollama run llama3.1:8b
```

[^1]: `ollama run` will pull the model first, so there is no need to use `ollama pull` first

## Techniques and source files

- K-shot prompting — `week1/k_shot_prompting.py`
- Chain-of-thought — `week1/chain_of_thought.py`
- Tool calling — `week1/tool_calling.py`
- Self-consistency prompting — `week1/self_consistency_prompting.py`
- RAG (Retrieval-Augmented Generation) — `week1/rag.py`
- Reflexion — `week1/reflexion.py\`

###  K-shot prompting





### Chain-of-thought

#### My initial attempt:

I did not assign anything to the system prompt. 

Result:

Success at first attempt. :)



However if I need to use Chain of thought, I need to finish the task, so... 



#### My second attempt: 

```python
YOUR_SYSTEM_PROMPT = "Explain your thought process in calculating the answer:"
```

The reason I did that is because this is from the material https://cloud.google.com/discover/what-is-prompt-engineering . In Chain of Thought, to ask the model to explain its reasoning process.

Result:

It's taking forever!



So I asked ChatGPT what went wrong. Here are the problems:

First, it encourages unbounded reasoning, for a problem like `3^12345 mod 100` , this can lead to repeated reasoning loops, sometimes never cleanly terminating.

Secondly, it conflicts with the output requirement `then give the final answer on the last line as "Answer: <number>`. Because the model prioritize reasoning over formatting(Really?) Due to the long context of the result, it may delay or forget the final answer. 

Thirdly, smaller models like `llama3.1:8b` especially don't manage long reasoning well. And are more likely to degrade in quality with long outputs.



#### My third attempt:

```python
YOUR_SYSTEM_PROMPT = """
Give concise step by step explanation.
Give the final answer on the last line as "Answer: <number>".
Don't repeat yourself.
"""
```

Result:

```
Running test 1 of 5
Expected output: Answer: 43
Actual output: Answer: 1
Running test 2 of 5
SUCCESS
```

One good thing about this time, is that it's faster than previous attempt. However, the first test gave a wrong answer!

Curiouser and curiouser... Is it because "concise" didn't give much constraint? So in the first attempt, it actually started rambling in the end..?

So I asked ChatGPT again, it's actually not the case.

"Concise" actually means faster, but it could provide less careful reasoning. 



#### My fourth attempt:

```python
YOUR_SYSTEM_PROMPT = """
Give 3-5 step by step explanation.
Verify your result before giving final answer.
Give the final answer on the last line as "Answer: <number>".
"""

```

Improvements of the prompt:

1. bounded reasoning
2. self-checking
3. clear output format

Where I tried to give specific constraints on number of steps. And ask the model to verify the result. The test passed the first time. I am wondering is there any way to further improve this prompt. 





#### My fifth attempt:

1. tried to be more specific about what does "verify your result" means, using math rules.
2. tried to make "step by step explanation" more concise and logical, using "focused on key transformations"
3. tried to improve correctness by saying "get the result right on first attempt."

```python
YOUR_SYSTEM_PROMPT = """
Give 3-5 step by step explanation focused on key transformations.
Verify your result with properties of modular arithmetic before giving final answer on the first attempt.
Give the final answer on the last line as "Answer: <number>".
"""
```



#### ChatGPT's attempt

I asked ChatGPT to provide a prompt and list its reasoning:

```python
YOUR_SYSTEM_PROMPT = """
Solve the problem in about 3-5 clear, logical steps focused on key transformations.
After reasoning, verify your result using properties of modular arithmetic (e.g., check cycles or patterns) before giving the final answer.
Do not skip necessary calculations.
Give the final answer on the last line exactly as "Answer: <number>".
Make sure the first answer you output is correct.
"""
```

| Feature                                                      | Why it helps                                            |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| “about 3-5 clear, logical steps”                             | Keeps reasoning **bounded** but allows **flexibility**  |
| “focused on key transformations”                             | **Avoids trivial details** and redundancy               |
| “After reasoning, verify your result using properties of modular arithmetic” | Makes verification concrete and **domain-specific**     |
| “Do not skip necessary calculations”                         | **Prevents** the model from **taking shortcuts**        |
| “final answer on the last line exactly as ‘Answer: <number>’” | Ensures your **parser** always works                    |
| “Make sure the first answer you output is correct”           | **Encourages correctness** without increasing verbosity |

My thought on this: 

1. “Make sure the first answer you output is correct” is redundant. How to make sure the output is correct? It already said previously that "verify your result using properties of modular arithmetic (e.g., check cycles or patterns) before giving the final answer"
2. I would also like to add "don't repeat yourself" back into the prompt, because I saw repetition in the output, also the second test is taking forever. 
3. I also removed "Do not skip necessary calculations." because this is contradicted to the previous prompt "3-5 steps".

#### My sixth attempt:

```python
YOUR_SYSTEM_PROMPT = """
Solve the problem in about 3-5 clear, logical steps focused on key transformations.
After reasoning, verify your result using properties of modular arithmetic (e.g., check cycles or patterns) before giving the final answer.
Give the final answer on the last line as "Answer: <number>".
Don't repeat yourself.
"""
```

Ok this time, it's also taking forever... I don't understand why.



#### My seventh attempt:

```python
YOUR_SYSTEM_PROMPT = """
Give 3-5 step by step explanation focused on key transformations.
Verify your result with properties of modular arithmetic before giving final answer.
Give the answer in the format exactly as the user asked.
"""
```

| Feature                                                  | Why it helps                                        |
| -------------------------------------------------------- | --------------------------------------------------- |
| Give the answer in the format exactly as the user asked. | Allow the user to specify the format of the result. |




## Deliverables
- Read the task description in each file.
- Design and run prompts (look for all the places labeled `TODO` in the code). That should be the only thing you have to change (i.e. don't tinker with the model). 
- Iterate to improve results until the test script passes.
- Save your final prompt(s) and output for each technique.
- Make sure to include in your submission the completed code for each prompting technique file. ***Double check that all `TODO`s have been resolved.***

## Evaluation rubric (60 pts total)
- 10 for each completed prompt across the 6 different prompting techniques



