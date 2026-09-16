# Tools & RAG in Agentic AI

> **Learning Level:** Beginner
>
> **Category:** Agentic AI, Core Concepts
>
> **Estimated Learning Time:** 25 Minutes

---

## 🎯 Learning Objectives

By the end of this topic, students will be able to:

- Understand what AI tools are and why Large Language Models need them.
- Explain the ReAct (Reason + Act) loop used by agents.
- Understand Retrieval-Augmented Generation (RAG) and static knowledge limitations.
- Understand how Agentic RAG enhances standard RAG with autonomous reasoning.

---

## 🧠 What is Tools & RAG in Agentic AI?

### Simple Definition

Tools and RAG are mechanisms that extend an AI's capabilities beyond its static training memory by allowing it to interact with external software functions and private knowledge bases.

### In Simple Words

Think of a standard Large Language Model (LLM) like ChatGPT as a brilliant student locked in a room with no internet access, no calculator, and no watch. **Tools** are software functions or APIs we give to an AI agent so it can interact with the outside world. **RAG (Retrieval-Augmented Generation)** is a technique where we connect an LLM to an external knowledge base (like a folder of PDF documents, a database, or a website) to fetch live or private information before answering.

### Real-World Analogy

Imagine a librarian who has memorized thousands of book titles (the LLM's training memory). If you ask for today's weather or the university's private late fee policy from yesterday, the librarian cannot rely on memory alone. Instead, the librarian uses a calculator for math, checks the daily newspaper (Tool), and looks up the physical university handbook (RAG) to give you an accurate answer.

---

## 🔍 Why Do We Need Tools & RAG?

Explain:

- What problem does it solve? LLMs suffer from static knowledge and hallucination when asked about real-time or private information.
- Why was this concept introduced? To allow AI models to act as autonomous problem-solvers rather than mere text predictors.
- What difficulties exist without it? Without tools and RAG, AI models cannot perform exact calculations, fetch live data, or access secure private documentation.
- Where is it commonly used? In customer support agents, enterprise search systems, automated assistants, and code execution environments.

---

## 🏗️ How Does It Work?

Explain the concept step-by-step.

### Step 1 — Reasoning (ReAct Loop)

The agent analyzes the user prompt to determine if external help is required.

### Step 2 — Action & Retrieval

The agent selects the appropriate tool (such as a database search or RAG retriever) and executes it with specific inputs.

### Step 3 — Observation

The agent reviews the output returned by the tool or retrieval system.

### Step 4 — Final Generation

The agent synthesizes the retrieved facts and tool outputs into a clear, accurate final response for the user.

---

## 📐 Core Concepts

### 1. AI Tools

Software functions or APIs given to an AI agent to interact with the outside world, such as calculators, web search engines, and email senders.

### 2. Retrieval-Augmented Generation (RAG)

A technique that connects an LLM to an external knowledge base, retrieving relevant paragraphs and handing them to the model along with the prompt.

### 3. Agentic RAG

An advanced approach where the AI acts as an intelligent worker, actively rewriting search queries, searching multiple databases, and cross-checking facts before answering.

---

## 💻 Coding Example

> **Goal:** Build a beginner-friendly conceptual script showing how an Agent selects tools and integrates RAG retrieval.

### Code

```python
# A beginner-friendly conceptual script showing how an Agent selects tools.

# 1. Define our Custom Tools
def search_university_handbook(query: str) -> str:
    """Simulates RAG: Searches the university handbook for relevant info."""
    database = {
        "exam fee": "The late fee for final exams is $50 if submitted after Dec 1st.",
        "library hours": "The campus library is open from 8:00 AM to 10:00 PM on weekdays."
    }
    
    # Simple keyword matching for demonstration
    for key, value in database.items():
        if key in query.lower():
            return value
    return "No relevant information found in the university handbook."

def calculate_total_fee(base_fee: int, late_fee: int) -> int:
    """A simple math tool for the agent."""
    return base_fee + late_fee

# 2. Simulate the Agent's Decision-Making Process
def simple_agent(user_prompt: str):
    print(f"\n--- User Prompt: '{user_prompt}' ---")
    
    # Step 1: Reasoning (Agent figures out what tool to use)
    if "fee" in user_prompt.lower() or "library" in user_prompt.lower():
        print("🤖 Agent Thought: I need to check the university handbook.")
        
        # Step 2: Action (Calling the RAG tool)
        retrieved_info = search_university_handbook(user_prompt)
        print(f"🔍 Tool Output (RAG Retrieval): {retrieved_info}")
        
        # If the user wants to calculate math based on that:
        if "total" in user_prompt.lower() and "50" in retrieved_info:
            print("🤖 Agent Thought: The user wants a total. Let me use the calculator tool.")
            total = calculate_total_fee(100, 50) # Assuming $100 base fee
            print(f"🧮 Tool Output (Calculator): Final calculated total is ${total}")
            print(f"💡 Final Answer: Based on the handbook, the late fee is $50. With your base fee, your total is ${total}.")
        else:
            print(f"💡 Final Answer: {retrieved_info}")
            
    else:
        print("🤖 Agent Thought: I don't need a special tool for this.")
        print("💡 Final Answer: Hello! How can I help you with university policies today?")

# 3. Test the Agent
simple_agent("What are the rules regarding the exam fee?")
simple_agent("Can you calculate my total exam cost if my base fee is $100 and I have an exam fee penalty?")
```
