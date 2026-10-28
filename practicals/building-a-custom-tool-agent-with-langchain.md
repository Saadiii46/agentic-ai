# 🧪 Practical: Building a Custom Tool Agent with LangChain

> **Topic:** Tools in Agentic AI
>
> **Difficulty:** BEGINNER
>
> **Technology:** Python, LangChain, OpenAI
>
> **Estimated Time:** 20 minutes

---

## 🎯 Practical Objective

In this practical, you will build:

> A Python script that initializes an AI agent equipped with a custom multiplication tool using LangChain and OpenAI.

By completing this practical, you will learn how to:

- Set up the necessary LangChain and OpenAI libraries.
- Write custom Python functions and decorate them with `@tool`.
- Craft effective docstrings for agent tools.
- Initialize and execute a ReAct agent with `verbose=True` to observe internal thought processes.

---

## 🧠 What You Should Know Before Starting

Before beginning this practical, you should understand:

- Basic Python programming (functions, decorators, and variables).
- What Large Language Models and AI Agents are.
- The ReAct (Reason + Act) loop concept.

If these concepts are unfamiliar, review:

- Introduction to Agentic AI
- Tools in Agentic AI (Theory)

---

# 🏗️ What Are We Building?

We are building a demonstration script that shows how an LLM overcomes its inability to perform complex calculations by dynamically invoking a custom Python function (tool).

### Final Result

When you run the script with a prompt like `"What is 4567 multiplied by 89?"`, the console will display the agent's thought process, the tool invocation, the observation, and the final natural language answer.

---

## 🛠️ Step-by-Step Implementation

### Step 1: Install Prerequisites

Make sure you have installed the necessary libraries in your Python environment:

```bash
pip install langchain langchain-openai langchain-community
```

### Step 2: Write the Python Code

Create a file named `agent_tool_demo.py` and add the following code:

```python
import os
from langchain.agents import AgentType, initialize_agent
from langchain_community.tools import tool
from langchain_openai import ChatOpenAI

# 1. Set up your OpenAI API Key (Replace with your actual key or set it as an environment variable)
os.environ["OPENAI_API_KEY"] = "your-openai-api-key-here"

# 2. Define a Custom Tool using the @tool decorator
# The docstring is extremely important! The LLM reads the docstring to know WHEN and HOW to use the tool.
@tool
def multiply_numbers(a: int, b: int) -> int:
    """Useful for multiplying two integers together. Use this when you need to do multiplication."""
    return a * b

# Put all available tools in a list
tools = [multiply_numbers]

# 3. Initialize the LLM (We use GPT-3.5 or GPT-4)
llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)

# 4. Initialize the Agent
# We use ZERO_SHOT_REACT_DESCRIPTION, which means the agent looks at the tool descriptions 
# and decides on the fly which one to use.
agent = initialize_agent(
    tools=tools,
    llm=llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True  # Setting verbose=True lets us see the agent's "Thoughts" and "Actions" in the console!
)

# 5. Run the Agent with a task that requires the tool
response = agent.run("What is 4567 multiplied by 89?")
print("\nFinal Answer:", response)
```

### Step 3: Run Your Script

Execute your Python script:

```bash
python agent_tool_demo.py
```

Observe how the agent thinks, calls the tool, receives the observation, and provides the final answer!
