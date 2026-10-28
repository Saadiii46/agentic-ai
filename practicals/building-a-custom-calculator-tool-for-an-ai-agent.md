# 🧪 Practical: Building a Custom Calculator Tool for an AI Agent

> **Topic:** Tools in Agentic AI
>
> **Difficulty:** Beginner
>
> **Technology:** Python, LangChain, OpenAI
>
> **Estimated Time:** 30 minutes

---

## 🎯 Practical Objective

In this practical, you will build:

> A Python-based AI agent using LangChain that uses a custom calculator function as a tool to accurately multiply large integers.

By completing this practical, you will learn how to:

- Install and configure LangChain and OpenAI dependencies.
- Write a custom tool using the `@tool` decorator with appropriate docstrings and type hints.
- Initialize a ChatOpenAI model and an agent executor.
- Inspect the ReAct loop and tool invocation process using `verbose=True`.

---

## 🧠 What You Should Know Before Starting

Before beginning this practical, you should understand:

- Basic Python functions, type hinting, and docstrings.
- The fundamental concept of LLMs and API keys.
- What an AI agent is and how the ReAct loop works.

If these concepts are unfamiliar, review:

- Introduction to AI Agents
- Python Type Hints and Docstrings

---

# 🏗️ What Are We Building?

We are building a Python script that sets up an AI agent. When given a complex math question like multiplying two large numbers, the agent will recognize its own limitation, invoke our custom `multiply_numbers` tool, and return the exact correct result.

### Final Result

```text
> Entering new AgentExecutor chain...
Invoking: `multiply_numbers` with `{'a': 4567, 'b': 8932}`
res: 40792144
I have calculated the result of 4567 multiplied by 8932, which is 40,792,144.

> Finished chain.

--- Final Response ---
4567 multiplied by 8932 is 40,792,144.
```

---

## 🛠️ Step-by-Step Implementation

### Step 1: Install Prerequisites
Run the following command in your terminal:
```bash
pip install langchain langchain-openai openai python-dotenv
```

### Step 2: Write the Agent Code
Create a file named `agent_tool.py` and add the following code:

```python
import os
from langchain.agents import AgentType, initialize_agent
from langchain_core.tools import tool
from langchain_openai import ChatOpenAI

# 1. Set your OpenAI API key
os.environ["OPENAI_API_KEY"] = "your-openai-api-key-here"

# 2. Define a Custom Tool using the @tool decorator
@tool
def multiply_numbers(a: int, b: int) -> int:
    """Multiplies two integers together and returns the result."""
    return a * b

# 3. Collect our tools in a list
tools = [multiply_numbers]

# 4. Initialize the Language Model (The "Brain" of the Agent)
llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)

# 5. Initialize the Agent with the LLM and the Tools
agent_executor = initialize_agent(
    tools=tools,
    llm=llm,
    agent=AgentType.OPENAI_FUNCTIONS,
    verbose=True
)

# 6. Run the Agent
response = agent_executor.invoke({"input": "What is 4567 multiplied by 8932?"})

print("\n--- Final Response ---")
print(response["output"])
```

### Step 3: Run the Script
Execute your script using python:
```bash
python agent_tool.py
```
Observe how the agent invokes the tool instead of guessing the math!
