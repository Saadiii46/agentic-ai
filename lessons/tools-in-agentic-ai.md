# Tools in Agentic AI

> **Learning Level:** Beginner
>
> **Category:** Agentic AI
>
> **Estimated Learning Time:** 30 minutes

---

## 🎯 Learning Objectives

By the end of this topic, students will be able to:

- Understand the limitations of standard Large Language Models (LLMs).
- Define what a "tool" is in the context of Agentic AI.
- Explain the ReAct (Reason + Act) framework for tool usage.
- Describe how tools are registered and called by an AI agent.

---

## 🧠 What is Tools in Agentic AI?

### Simple Definition

Tools are functions, APIs, or software programs provided to an AI agent so it can interact with the outside world and perform real-world tasks.

### In Simple Words

A standard LLM is like a brilliant brain trapped in a room without internet or accessories. It can chat and write, but it cannot browse live websites, perform precise math on its own, or check databases. **Tools** are the hands and feet we give to the AI agent so it can use a calculator, look up live news, or schedule meetings.

### Real-World Analogy

Think of a human working at a desk:
* **The LLM (Brain):** Knows *how* to think, plan, and decide.
* **The Tools (Accessories):** A calculator for math, a web browser for current news, or a calendar app to schedule meetings.

---

## 🔍 Why Do We Need Tools in Agentic AI?

Explain:

- What problem does it solve? Standard LLMs are limited to their training cutoff date and lack execution capabilities.
- Why was this concept introduced? To transition AI from a passive text-generator into an active, capable **agent**.
- What difficulties exist without it? Inability to fetch real-time data, perform deterministic calculations, or interact with external systems.
- Where is it commonly used? Web search assistants, customer support automation, software engineering agents, and enterprise data retrieval systems.

---

## 🏗️ How Does It Work?

Explain the concept step-by-step using the ReAct (Reason + Act) framework.

### Step 1 — Thought
The agent thinks about what to do next based on the user request. (*"The user is asking for the weather in Tokyo today. I don't know live weather, so I need a tool."*)

### Step 2 — Action
The agent selects the appropriate tool and provides input. (*Calls `get_weather(city="Tokyo")`*)

### Step 3 — Observation
The tool executes and returns data. (*Returns: "22°C, Sunny"*)

### Step 4 — Response
The agent uses that data to formulate the final answer for the user. (*"The weather in Tokyo today is 22°C and sunny!"*)

---

## 📐 Core Concepts

### 1. The LLM as the Orchestrator
The LLM acts as the decision-maker, analyzing prompts and determining whether internal knowledge is sufficient or if an external tool is required.

### 2. Tool Schema and Descriptions
AI models do not inherently understand Python code; they rely heavily on structured metadata (names, descriptions, parameter types, and required fields) to know when and how to invoke a tool.

### 3. The Execution Loop
The iterative process where the model requests a tool call, the execution environment runs the function, and the output is fed back into the conversation context for the model to process.

---

## 💻 Coding Example

> **Goal:** Define a custom tool and describe it for an AI agent to use.

### Code

```python
# Step 1: Define our custom tool function
def calculate_rectangle_area(length: float, width: float) -> float:
    """Calculates the area of a rectangle given length and width."""
    return length * width
```
