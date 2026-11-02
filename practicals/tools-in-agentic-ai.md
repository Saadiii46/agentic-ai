# 🧪 Practical: Building Your First Agent with a Tool

> **Topic:** Tools in Agentic AI
>
> **Difficulty:** BEGINNER
>
> **Technology:** Python, LangChain, OpenAI
>
> **Estimated Time:** 30 minutes

---

## 🎯 Practical Objective

In this practical, you will build:

> A custom Python-powered agent using LangChain that uses a user-defined tool to calculate string lengths and answer user queries correctly through the ReAct framework.

By completing this practical, you will learn how to:

- Install and set up LangChain and OpenAI libraries in Python.
- Create custom tools using the `@tool` decorator.
- Initialize an LLM agent with `ZERO_SHOT_REACT_DESCRIPTION`.
- Inspect the agent's internal Thought, Action, and Observation loop (`verbose=True`).

---

## 🧠 What You Should Know Before Starting

Before beginning this practical, you should understand:

- Basic Python programming and functions.
- What LLMs and API keys are.
- The fundamental concept of Agentic AI tools and the ReAct loop.

If these concepts are unfamiliar, review:

- Introduction to Agentic AI
- Tools in Agentic AI Theory Module

---

# 🏗️ What Are We Building?

We are building a Python script that instantiates an OpenAI chat model, equips it with a custom string length calculator function as a tool, and executes a query requiring the tool's execution.

### Prerequisites

Make sure you have installed the required libraries in your environment:
```bash
pip install langchain langchain-openai langchain-community
```

### The Code Example

```python
import os
from langchain.agents import AgentType, initialize_agent
from langchain.chat_models import ChatOpenAI
from langchain.tools import tool

# 1. Set your OpenAI API Key (Replace with your actual key or set it in your environment)
os.environ["OPENAI_API_KEY"] = "your-openai-api-key-here"

# 2. Define a custom tool using the @tool decorator
@tool
def calculate_string_length(text: str) -> int:
    """Calculates and returns the number of characters in a given text string."""
    print(f"\n[TOOL LOG] Running calculate_string_length with input: '{text}'")
    return len(text)

# 3. Gather our tools into a list
tools = [calculate_string_length]

# 4. Initialize the LLM (The "Brain" of our agent)
llm = ChatOpenAI(temperature=0, model="gpt-3.5-turbo")

# 5. Initialize the Agent with the LLM and the tools
agent = initialize_agent(
    tools=tools,
    llm=llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True  # verbose=True lets us see the agent's "thinking" process
)

# 6. Run the agent with a prompt that requires the tool
response = agent.run("Can you tell me how many characters are in the word 'AgenticAI'?")
print("\n--- Final Answer ---")
print(response)
```

### Final Result

```text
> Entering new AgentExecutor chain...
Thought: I need to find the number of characters in the word 'AgenticAI'. I should use the calculate_string_length tool for this.
Action: calculate_string_length
Action Input: AgenticAI

[TOOL LOG] Running calculate_string_length with input: 'AgenticAI'
Observation: 9
Thought: I now know the final answer.
Final Answer: The word 'AgenticAI' contains 9 characters.
--- Final Answer ---
The word 'AgenticAI' contains 9 characters.
```
