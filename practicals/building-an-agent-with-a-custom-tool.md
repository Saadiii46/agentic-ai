# 🧪 Practical: Building an Agent with a Custom Tool

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

> A Python script using LangChain and OpenAI that equips an AI agent with a custom tool to count the number of letters in a given word using the ReAct framework.

By completing this practical, you will learn how to:

- Set up the environment and install required AI libraries (`langchain`, `openai`).
- Define a custom Python function and convert it into an AI tool using the `@tool` decorator.
- Initialize an LLM and an agent executor with zero-shot ReAct description.
- Analyze the agent's step-by-step console output (Thought -> Action -> Observation).

---

## 🧠 What You Should Know Before Starting

Before beginning this practical, you should understand:

- Basic Python programming (functions, decorators).
- How Large Language Models (LLMs) operate.
- The concept of Agentic AI and why agents need external tools.

If these concepts are unfamiliar, review:

- Introduction to Agentic AI
- What are Tools in Agentic AI? (Theory Lesson)

---

# 🏗️ What Are We Building?

We are building a simple yet powerful Agentic AI application. When a user asks the agent a question like *"Can you tell me how many letters are in the word 'Agentic'?"*, the agent will not just guess. Instead, it will use its reasoning loop to recognize it needs a tool, call our custom `calculate_word_length` function, observe the output, and respond accurately.

### Final Result

```text
> Entering new AgentExecutor chain...
Thought: I need to find the number of letters in the word 'Agentic'. I should use the calculate_word_length tool for this.
Action: calculate_word_length
Action Input: Agentic
Observation: 7
Thought: I now know the final answer.
Final Answer: There are 7 letters in the word 'Agentic'.

> Finished chain.

Final Response from Agent:
There are 7 letters in the word 'Agentic'.
```

---

## 💻 Step-by-Step Implementation

### Step 1: Install Prerequisites
Make sure you have installed the required libraries:
```bash
pip install langchain langchain-openai openai
```

### Step 2: Write the Python Code
Create a file named `agent_tool_practical.py` and add the following code:

```python
import os
from langchain.agents import AgentType, initialize_agent
from langchain.chat_models import ChatOpenAI
from langchain.tools import tool

# 1. Set up your OpenAI API Key (Replace with your actual key or set it in your environment)
os.environ["OPENAI_API_KEY"] = "your-openai-api-key-here"

# 2. Define a Custom Tool using the @tool decorator
@tool
def calculate_word_length(word: str) -> int:
    """Calculates and returns the number of letters in a given word."""
    length = len(word)
    return length

# 3. Create a list of tools available for the agent
tools = [calculate_word_length]

# 4. Initialize the LLM (The "Brain" of the agent)
llm = ChatOpenAI(temperature=0, model="gpt-3.5-turbo")

# 5. Initialize the Agent with the LLM and the Tools
agent_executor = initialize_agent(
    tools=tools,
    llm=llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True  # verbose=True lets us see the agent's "Thought -> Action -> Observation" process!
)

# 6. Run the Agent
response = agent_executor.run("Can you tell me how many letters are in the word 'Agentic'?")
print("\nFinal Response from Agent:")
print(response)
```
