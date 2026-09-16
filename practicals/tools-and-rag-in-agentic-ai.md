# 🧪 Practical: Building a Tool-Using Agent with RAG

> **Topic:** Tools & RAG in Agentic AI
>
> **Difficulty:** Beginner
>
> **Technology:** Python
>
> **Estimated Time:** 30 Minutes

---

## 🎯 Practical Objective

In this practical, you will build:

> A modular Python script that simulates an AI agent capable of selecting custom tools (a RAG search tool and a calculator tool) based on user requests using a ReAct-style reasoning loop.

By completing this practical, you will learn how to:

- Define custom software functions as tools for an AI agent.
- Simulate RAG document retrieval within an agentic workflow.
- Implement a basic reasoning loop where the agent decides when and how to invoke tools.
- Chain multiple tools together to solve multi-step user queries.

---

## 🧠 What You Should Know Before Starting

Before beginning this practical, you should understand:

- Basic Python syntax (functions, dictionaries, conditional statements).
- The concept of Large Language Models (LLMs) and their limitations regarding static knowledge.
- The fundamentals of Retrieval-Augmented Generation (RAG).

If these concepts are unfamiliar, review:

- Tools & RAG in Agentic AI Theory Lesson.
- Introduction to Python Functions and Logic.

---

# 🏗️ What Are We Building?

We are building a Python-based simulation of an autonomous AI agent. The agent receives natural language prompts, reasons about what information or computation is needed, retrieves data from a local mock RAG database (university handbook), performs math calculations when necessary, and delivers an informed final answer.

### Final Result

```text
--- User Prompt: 'What are the rules regarding the exam fee?' ---
🤖 Agent Thought: I need to check the university handbook.
🔍 Tool Output (RAG Retrieval): The late fee for final exams is $50 if submitted after Dec 1st.
💡 Final Answer: The late fee for final exams is $50 if submitted after Dec 1st.

--- User Prompt: 'Can you calculate my total exam cost if my base fee is $100 and I have an exam fee penalty?' ---
🤖 Agent Thought: I need to check the university handbook.
🔍 Tool Output (RAG Retrieval): The late fee for final exams is $50 if submitted after Dec 1st.
🤖 Agent Thought: The user wants a total. Let me use the calculator tool.
🧮 Tool Output (Calculator): Final calculated total is $150
💡 Final Answer: Based on the handbook, the late fee is $50. With your base fee, your total is $150.
```

---

## 💻 Step-by-Step Implementation

### Step 1: Define Custom Tools

First, we define our custom tools: one for simulating RAG database search (`search_university_handbook`) and one for arithmetic (`calculate_total_fee`).

```python
def search_university_handbook(query: str) -> str:
    """Simulates RAG: Searches the university handbook for relevant info."""
    database = {
        "exam fee": "The late fee for final exams is $50 if submitted after Dec 1st.",
        "library hours": "The campus library is open from 8:00 AM to 10:00 PM on weekdays."
    }
    
    for key, value in database.items():
        if key in query.lower():
            return value
    return "No relevant information found in the university handbook."

def calculate_total_fee(base_fee: int, late_fee: int) -> int:
    """A simple math tool for the agent."""
    return base_fee + late_fee
```

### Step 2: Implement the Agent Logic

Next, we create the agent function that processes prompts, reasons, calls tools, and generates responses.

```python
def simple_agent(user_prompt: str):
    print(f"\n--- User Prompt: '{user_prompt}' ---")
    
    if "fee" in user_prompt.lower() or "library" in user_prompt.lower():
        print("🤖 Agent Thought: I need to check the university handbook.")
        
        retrieved_info = search_university_handbook(user_prompt)
        print(f"🔍 Tool Output (RAG Retrieval): {retrieved_info}")
        
        if "total" in user_prompt.lower() and "50" in retrieved_info:
            print("🤖 Agent Thought: The user wants a total. Let me use the calculator tool.")
            total = calculate_total_fee(100, 50)
            print(f"🧮 Tool Output (Calculator): Final calculated total is ${total}")
            print(f"💡 Final Answer: Based on the handbook, the late fee is $50. With your base fee, your total is ${total}.")
        else:
            print(f"💡 Final Answer: {retrieved_info}")
            
    else:
        print("🤖 Agent Thought: I don't need a special tool for this.")
        print("💡 Final Answer: Hello! How can I help you with university policies today?")
```

### Step 3: Test and Run the Agent

Finally, invoke the agent with different test prompts.

```python
# Test the Agent
simple_agent("What are the rules regarding the exam fee?")
simple_agent("Can you calculate my total exam cost if my base fee is $100 and I have an exam fee penalty?")
```
