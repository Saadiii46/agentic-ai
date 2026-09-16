# Tools in Agentic AI

> **Learning Level:** Beginner
>
> **Category:** Agentic AI
>
> **Estimated Learning Time:** 45 minutes

---

## 🎯 Learning Objectives

By the end of this topic, students will be able to:

- Explain the limitations of LLMs operating in isolation.
- Define what tools are in the context of Agentic AI.
- Understand the ReAct (Reason + Act) framework for tool utilization.
- Identify common types of tools used by AI agents and their use cases.

---

## 🧠 What is Tools in Agentic AI?

### Simple Definition

Tools in Agentic AI are functions, APIs, or software programs provided to Large Language Models (LLMs) to allow them to perform actions beyond generating text and interact with the external world.

### In Simple Words

While LLMs are amazing at writing, summarizing, and reasoning, they live in a bubble without internet access, real-time data, or execution environments. Tools give agents "hands" and "eyes" to interact with the outside world, transforming a passive chatbot into an active problem-solver.

### Real-World Analogy

Imagine you are a brilliant chef (the LLM). You know every recipe in the world, but your hands are tied behind your back. If someone asks you to bake a cake, you can *describe* how to do it, but you can't actually do it. **Tools** are your kitchen utensils (oven, whisk, measuring cup)—they allow you to actually *bake* the cake.

---

## 🔍 Why Do We Need Tools in Agentic AI?

LLMs have three major weaknesses that tools fix:

- **Outdated Knowledge:** LLMs only know what they were trained on. A **Web Search Tool** lets them find real-time information.
- **Math & Logic Errors:** LLMs struggle with precise calculations. A **Calculator Tool** lets them compute numbers accurately.
- **No Digital Action:** LLMs cannot send emails or modify databases on their own. **API Tools** allow them to take real-world actions.

Without tools, AI agents are restricted entirely to their pre-trained parameters and static datasets, unable to fetch live data or effect changes in external systems.

---

## 🏗️ How Does It Work?

Most modern agents use a loop called **ReAct (Reason + Act)** to use tools effectively. It works in four steps:

### Step 1 — Thought
The agent thinks about what it needs to do. ("The user wants to know the weather in Tokyo. I don't know this, so I need a tool.")

### Step 2 — Action
The agent calls the appropriate tool with the required parameters. (`weather_tool(city="Tokyo")`)

### Step 3 — Observation
The agent looks at the tool's output. ("The tool returned: 22°C and Rainy.")

### Step 4 — Final Answer
The agent uses that output to answer the user. ("It is currently 22°C and rainy in Tokyo.")

---

## 📐 Core Concepts

### 1. LLM Isolation
LLMs lack real-time access and executable capabilities by default, necessitating external integrations.

### 2. The ReAct Framework
The cyclical process of Reasoning, Acting, Observing, and Answering that drives tool-enabled AI agents.

### 3. Tool Registration & Descriptions
The mechanism of describing functions and APIs using JSON schemas or framework abstractions so the LLM understands when and how to invoke them.

---

## 💻 Coding Example

> **Goal:** Create and register a custom calculator tool for an agent to perform accurate multiplication.

### Code

```python
# A simple Python function that acts as our tool
def multiply_numbers(a: float, b: float) -> float:
    """Multiplies two numbers together and returns the result."""
    return a * b

# When a user asks: "What is 4598 multiplied by 342?"
# 1. LLM Reasoning: "I shouldn't guess this. I should use the multiply_numbers tool."
# 2. Tool Call: The LLM generates a request: multiply_numbers(a=4598, b=342)
# 3. Execution: Our Python code runs the function and gets 1,572,516.
# 4. Response: The agent reports back to the user.
```
