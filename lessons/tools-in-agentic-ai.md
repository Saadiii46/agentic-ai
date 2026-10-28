# Tools in Agentic AI

> **Learning Level:** BEGINNER
>
> **Category:** Agentic AI Concepts & Architecture
>
> **Estimated Learning Time:** 30 minutes

---

## 🎯 Learning Objectives

By the end of this topic, students will be able to:

- Understand what tools are and why AI agents need them.
- Identify the limitations of standalone Large Language Models (LLMs).
- Explain the ReAct (Reason + Act) framework for tool usage.
- Create and use custom tools in Python using LangChain and OpenAI.

---

## 🧠 What is Tools in Agentic AI?

### Simple Definition

Tools are functions, APIs, or software programs provided to an AI agent so it can interact with the outside world, perform computations, and fetch real-time information.

### In Simple Words

A standard LLM is like a brilliant human brain trapped in a room with no internet or calculator. **Tools are its hands, eyes, and calculator.** They give the LLM the ability to take actions beyond just generating text.

### Real-World Analogy

Imagine hiring a brilliant research assistant who knows a lot of facts, but has no internet access, no watch, and no calculator. If you ask them for today's weather or to multiply 4567 by 89, they cannot reliably answer from memory alone. If you hand them a smartphone and a calculator (tools), they can look up the weather and do the math instantly.

---

## 🔍 Why Do We Need Tools?

Explain:

- Standard LLMs have a knowledge cutoff and lack real-time data access.
- They cannot check live weather, look up today's stock prices, or query internal databases.
- They struggle with precise mathematical calculations because they predict words rather than calculating numbers.
- Tools bridge the gap between static LLMs and dynamic real-world environments.

---

## 🏗️ How Does It Work?

Most tool-using agents operate on the **ReAct (Reason + Act)** framework.

### Step 1 — Thought

The agent thinks about what to do based on the user's prompt. *"The user is asking for the weather in Tokyo. I don't know the live weather, but I have a weather tool."*

### Step 2 — Action

The agent decides to use a specific tool and formats the required inputs. *"Call `get_weather(city='Tokyo')`."*

### Step 3 — Observation

The system executes the tool and returns the result to the agent. *"The tool returned: 22°C and Rainy."*

### Step 4 — Final Answer

The agent processes the observation and responds to the user. *"The weather in Tokyo right now is 22°C and rainy."*

---

## 📐 Core Concepts

### 1. LLM as a Reasoning Engine

The Large Language Model acts as the core brain that plans tasks, selects tools, and interprets outputs.

### 2. Tool Definitions and Docstrings

Tools require clear names, parameters, and docstrings. The LLM reads the docstring to understand *when* and *how* to use the tool.

### 3. Agent Execution Loops

The loop connecting Thought, Action, and Observation that allows multi-step problem solving.

---

## 💻 Coding Example

> **Goal:** Give an LLM a custom multiplication tool using LangChain and OpenAI.

### Code

```python
import os
from langchain.agents import AgentType, initialize_agent
from langchain_community.tools import tool
from langchain_openai import ChatOpenAI

# 1. Set up your OpenAI API Key
os.environ["OPENAI_API_KEY"] = "your-openai-api-key-here"

# 2. Define a Custom Tool using the @tool decorator
@tool
def multiply_numbers(a: int, b: int) -> int:
    """Useful for multiplying two integers together. Use this when you need to do multiplication."""
    return a * b

tools = [multiply_numbers]

# 3. Initialize the LLM
llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)

# 4. Initialize the Agent
agent = initialize_agent(
    tools=tools,
    llm=llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)

# 5. Run the Agent
response = agent.run("What is 4567 multiplied by 89?")
print("\nFinal Answer:", response)
```
