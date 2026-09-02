# Tools in Agentic AI

> **Learning Level:** Intermediate
>
> **Category:** Agentic AI Frameworks & Tooling
>
> **Estimated Learning Time:** 45 minutes

---

## 🎯 Learning Objectives

By the end of this topic, students will be able to:

- Understand the difference between traditional LLM text generation and Agentic AI.
- Define what a "Tool" is and why external capabilities are crucial for AI agents.
- Explain the ReAct (Reason + Act) loop used by tool-using agents.
- Implement custom tools in Python using LangChain.

---

## 🧠 What is Tools in Agentic AI?

### Simple Definition

Tools in Agentic AI are external functions, APIs, software, or databases that an AI agent can call to perform actions and gather information beyond its internal training data.

### In Simple Words

In traditional Artificial Intelligence (AI), you give a prompt, and the AI generates an answer based solely on what it has memorized during training. An **Agentic AI** system goes a step further. It has a brain (the Large Language Model) and a set of hands to interact with the outside world. Those "hands" are called **Tools**.

### Real-World Analogy

Imagine a brilliant human mathematician who knows formulas by heart. However, if you give them a massive dataset to multiply instantly or ask them for today's live stock prices without internet access, they cannot do it alone. If you hand them a calculator and a browser, they can suddenly solve complex, real-world problems. The calculator and browser are their **tools**.

---

## 🔍 Why Do We Need Tools in Agentic AI?

Even the smartest LLMs have fundamental limitations:
- They cannot check real-time weather or stock prices because their training data is fixed in time.
- They cannot perform complex, error-free math without computational assistance.
- They cannot send emails, query university databases, or book flights.

By giving an LLM access to **Tools**, we bridge the gap between static "text generation" and dynamic "action execution."

---

## 🏗️ How Does It Work? (The ReAct Framework)

Most agentic systems use a loop called **ReAct (Reason + Act)**:

### Step 1 — Thought
The agent thinks about the user's request and decides if it needs an external tool to answer correctly.

### Step 2 — Action
The agent selects the appropriate tool from its toolkit and provides the required inputs (arguments).

### Step 3 — Observation
The tool executes the task (e.g., runs a calculation or calls an API) and returns the result back to the agent.

### Step 4 — Thought / Response
The agent looks at the result and either provides the final answer to the user or triggers another action.

---

## 📐 Core Concepts

### 1. The Agent Brain (LLM)
The core language model responsible for reasoning, planning, and interpreting tool outputs.

### 2. Tool Definition & Docstrings
Functions exposed to the agent. The agent relies heavily on the tool's docstring (`""" ... """`) to understand when and how to invoke it.

### 3. Execution Loop
The iterative cycle where the agent reasons, executes actions via tools, observes outcomes, and determines the final response.

---

## 💻 Coding Example

> **Goal:** Create a custom calculator tool in Python and give it to an AI Agent using LangChain.

### Code

```python
import os
from dotenv import load_dotenv
from langchain.agents import AgentType, initialize_agent
from langchain_core.tools import tool
from langchain_openai import ChatOpenAI

# 1. Load environment variables (Make sure to set your OPENAI_API_KEY)
load_dotenv()

# 2. Define a Custom Tool using the @tool decorator
@tool
def calculate_rectangle_area(length: float, width: float) -> float:
    """Calculates the area of a rectangle given its length and width.
    Use this tool when you need to find the area of a rectangle.
    """
    area = length * width
    return area

tools = [calculate_rectangle_area]

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
if __name__ == "__main__":
    prompt = "I have a room that is 12.5 meters long and 4.2 meters wide. What is the total area?"
    response = agent.run(prompt)
    print("\nFinal Answer from Agent:")
    print(response)
```
