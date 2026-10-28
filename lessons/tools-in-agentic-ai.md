# Tools in Agentic AI

> **Learning Level:** Beginner
>
> **Category:** Agentic AI
>
> **Estimated Learning Time:** 45 minutes

---

## 🎯 Learning Objectives

By the end of this topic, students will be able to:

- Understand what tools are in the context of Agentic AI.
- Explain why LLMs need tools to interact with the external world.
- Describe the ReAct (Reasoning and Acting) loop used by agents.
- Create custom tools using Python and LangChain.

---

## 🧠 What is a Tool in Agentic AI?

### Simple Definition

A tool is a function or capability provided to an AI agent so it can perform actions beyond its internal training data.

### In Simple Words

Think of an AI model like GPT-4 as a brilliant brain trapped inside a computer. It knows a lot of facts, but it cannot calculate complex numbers accurately on the fly, check today's weather, or access private databases. Tools give this brain hands and eyes to interact with the world.

### Real-World Analogy

Imagine a world-class mathematician sitting in a room with only a pen and paper. They are extremely smart (like an LLM), but if you ask them to calculate the exact product of two 10-digit numbers instantly, they might make a mistake. If you give them a calculator (a Tool), they can use it to get the exact answer immediately.

---

## 🔍 Why Do We Need Tools?

Explain:

- What problem does it solve? LLMs suffer from hallucinations, lack real-time data, and cannot perform precise deterministic tasks (like math or API calls).
- Why was this concept introduced? To transition AI from passive conversational chatbots to active problem-solving agents.
- What difficulties exist without it? AI remains isolated to static pre-trained knowledge and cannot execute actions.
- Where is it commonly used? Web search assistants, coding agents, database querying bots, and automated workflow agents.

---

## 🏗️ How Does It Work?

Agents typically use tools using a pattern called **ReAct (Reasoning and Acting)**:

### Step 1 — Thought
The user asks a question. The agent thinks about what it needs to do and whether it requires external assistance.

### Step 2 — Action
The agent decides to use a specific tool with specific inputs generated from its reasoning.

### Step 3 — Observation
The tool runs, executes the task in the external environment or code, and returns the result back to the agent.

### Step 4 — Final Answer
The agent analyzes the tool's result and formulates a complete response to the user.

---

## 📐 Core Concepts

### 1. Docstrings Matter
When you write a tool using decorators like `@tool`, the docstring (the text inside `""" ... """` under the function) is crucial. The AI reads this description to understand *when* and *how* to use your tool.

### 2. Type Hints Matter
Type hints such as `a: int, b: int` help the AI know what kind of inputs (numbers, text, etc.) the tool expects and validates them before execution.

### 3. Extendability
You can turn *anything* into a tool—searching Google, querying a SQL database, or sending an email.

---

## 💻 Coding Example

> **Goal:** Create an AI agent equipped with a custom multiplier tool using LangChain.

### Code

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
