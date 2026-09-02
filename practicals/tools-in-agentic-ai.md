# 🧪 Practical: Building a Simple Tool-Using Agent with LangChain

> **Topic:** Tools in Agentic AI
>
> **Difficulty:** Intermediate
>
> **Technology:** Python, LangChain, OpenAI
>
> **Estimated Time:** 30 minutes

---

## 🎯 Practical Objective

In this practical, you will build:

> A Python script utilizing LangChain where an AI Agent dynamically decides to use a custom rectangle area calculator tool to solve user prompts.

By completing this practical, you will learn how to:

- Set up and configure LangChain with OpenAI.
- Define custom tools in Python using the `@tool` decorator.
- Write effective tool docstrings so LLMs understand when to invoke them.
- Initialize and execute an agent using the Zero-Shot ReAct description framework.

---

## 🧠 What You Should Know Before Starting

Before beginning this practical, you should understand:

- Basic Python programming (functions, decorators, environment variables).
- Fundamentals of Large Language Models and API keys.
- The concept of Agentic AI and the ReAct (Reason + Act) loop.

If these concepts are unfamiliar, review:

- Introduction to Agentic AI
- Python Decorators & Functions

---

# 🏗️ What Are We Building?

We are building a Python script (`agent_tool_demo.py`) that sets up an autonomous AI Agent. When given a prompt involving room measurements, the agent will analyze the request, reason that it needs a calculation tool, call our custom Python function (`calculate_rectangle_area`), observe the output, and present a well-structured final answer to the user.

### Prerequisites & Installation

Run this command in your terminal to install the necessary packages:
```bash
pip install langchain langchain-openai openai python-dotenv
```

### Complete Source Code

Create a file named `agent_tool_demo.py` and paste the following code:

```python
import os
from dotenv import load_dotenv
from langchain.agents import AgentType, initialize_agent
from langchain_core.tools import tool
from langchain_openai import ChatOpenAI

# 1. Load environment variables (Make sure to set your OPENAI_API_KEY)
load_dotenv()

# 2. Define a Custom Tool using the @tool decorator
# The docstring is extremely important! The AI reads this to know WHEN and HOW to use the tool.
@tool
def calculate_rectangle_area(length: float, width: float) -> float:
    """Calculates the area of a rectangle given its length and width.
    Use this tool when you need to find the area of a rectangle.
    """
    area = length * width
    return area

# Collect our tools in a list
tools = [calculate_rectangle_area]

# 3. Initialize the LLM (The "Brain" of the Agent)
llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)

# 4. Initialize the Agent
agent = initialize_agent(
    tools=tools,
    llm=llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True  # verbose=True lets us see the Thought -> Action -> Observation loop!
)

# 5. Run the Agent with a prompt that requires the tool
if __name__ == "__main__":
    prompt = "I have a room that is 12.5 meters long and 4.2 meters wide. What is the total area, and can you explain how you got it?"
    
    print(f"User Prompt: {prompt}\n")
    response = agent.run(prompt)
    
    print("\nFinal Answer from Agent:")
    print(response)
```

### Final Result & Terminal Output (`verbose=True`)

When you run the script, your terminal will display the agent's internal reasoning loop:

```text
User Prompt: I have a room that is 12.5 meters long and 4.2 meters wide. What is the total area, and can you explain how you got it?

[1m> Entering new AgentExecutor chain...[0m
[32;1m[1mThought: I need to calculate the area of a rectangle given its length and width. I can use the calculate_rectangle_area tool.[0m
[36;1m[1mAction: calculate_rectangle_area[0m
[36;1m[1mAction Input: {'length': 12.5, 'width': 4.2}[0m
[33;1m[1mObservation: 52.5[0m
[32;1m[1mThought: I now know the final answer. The area is 52.5 square meters.[0m
[32;1m[1mFinal Answer: The total area of the room is 52.5 square meters. This was calculated by multiplying the length (12.5 meters) by the width (4.2 meters).[0m

Final Answer from Agent:
The total area of the room is 52.5 square meters. This was calculated by multiplying the length (12.5 meters) by the width (4.2 meters).
```
