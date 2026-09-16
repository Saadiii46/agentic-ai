# Tools in Agentic AI

> **Learning Level:** BEGINNER
>
> **Category:** Agentic AI
>
> **Estimated Learning Time:** 30 minutes

---

## 🎯 Learning Objectives

By the end of this topic, students will be able to:

- Understand what tools are in the context of Agentic AI.
- Explain why Large Language Models (LLMs) need external tools.
- Describe the ReAct (Reason + Act) framework used by AI agents.
- Recognize how tools extend the capabilities of AI systems beyond text generation.

---

## 🧠 What is Tools in Agentic AI?

### Simple Definition

Tools in Agentic AI are external functions, APIs, calculators, databases, or software programs that an AI agent can use to interact with the outside world and perform tasks it couldn't do alone.

### In Simple Words

Imagine you are a human chef cooking a new recipe. You have a great brain (your knowledge), but to actually make the dish, you need tools: a knife to chop vegetables, a measuring cup for liquids, and an oven to bake. Similarly, an AI model needs tools to perform real-world actions.

### Real-World Analogy

Think of a brilliant mathematician sitting in a room with only a pen and paper. They can solve many problems from memory, but if you ask them to compute the 50th decimal digit of pi or check today's stock price, they need a calculator or a computer terminal. Tools act as that computer terminal for an AI agent.

---

## 🔍 Why Do We Need Tools?

Explain:

- Large Language Models (LLMs) like GPT-4 or Llama are amazing at writing, summarizing, and reasoning. However, they have major limitations:
1. **They can't calculate math accurately:** LLMs guess words based on patterns, so they often fail at complex arithmetic.
2. **They don't know real-time data:** Their training data has a cutoff date. They don't know today's weather or stock prices.
3. **They can't take action:** An LLM alone cannot send an email, book a flight, or save a file to your computer.

By giving an AI model Tools, we give it "hands" and "eyes" to solve these problems!

---

## 🏗️ How Does It Work?

Usually, agents use a loop called ReAct (Reason + Act) to use tools effectively:

### Step 1 — Thought
The agent thinks about what it needs to do. (*"The user wants to know the weather in Tokyo. I don't know this, so I need to use the Weather Tool."*)

### Step 2 — Action
The agent calls the tool with the correct inputs. (`weather_tool(city="Tokyo")`)

### Step 3 — Observation
The tool returns the result. (*"The weather in Tokyo is 15°C and Rainy."*)

### Step 4 — Final Answer
The agent uses the observation to answer the user. (*"The current weather in Tokyo is 15°C and rainy."*)

---

## 📐 Core Concepts

### 1. External Functions & APIs
Custom Python functions or web services that execute specific tasks and return data back to the agent.

### 2. The ReAct Framework
An iterative loop of reasoning, acting, and observing that enables dynamic problem-solving.

### 3. Tool Descriptions & Docstrings
Natural language explanations attached to tools that instruct the LLM on *when* and *how* to invoke them.

---

## 💻 Coding Example

> **Goal:** Create a custom tool that calculates how many letters are in a word and give it to an agent using LangChain.

### Code

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
