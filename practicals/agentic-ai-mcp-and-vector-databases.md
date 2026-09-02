<div align="center">

# 🧪 Practical Lab: MCP Simulation & Vector Database Memory

<img src="https://img.shields.io/badge/Language-Python-blue?style=for-the-badge&logo=python" alt="Python">
<img src="https://img.shields.io/badge/Libraries-ChromaDB%20%7C%20Sentence--Transformers-orange?style=for-the-badge" alt="Libraries">

*Hands-on code labs to simulate an MCP tool-use architecture and build a semantic long-term memory system using ChromaDB.*

</div>

---

## 📑 Lab Outline
1. [Lab 1: Simulating an MCP Architecture](#lab-1-simulating-an-mcp-architecture)
   - Step 1: Create a simple Tool Server (`weather_server.py`)
   - Step 2: The Agent Using the Tool
2. [Lab 2: Using a Vector Database with Python](#lab-2-using-a-vector-database-with-python)
   - Step 1: Install Dependencies
   - Step 2: Write the Python Code for Agent Memory
   - Understanding the Results

---

## Lab 1: Simulating an MCP Architecture

In an MCP system, you have:
1. **The MCP Host/Client:** The AI application (like Claude Desktop or an AI agent you build).
2. **The MCP Server:** A small program that exposes data or tools (e.g., a server that reads your local weather or files).

Below is a simple Python example showing how an AI Agent (Client) might request data from a custom local tool (acting like a basic MCP server concept).

### Step 1: Create a simple Tool Server (`weather_server.py`)
```python
# This simulates an MCP server that provides a tool to the AI agent
import json

def get_current_weather(location: str):
    """Tool function to fetch weather data."""
    # In a real MCP setup, this is exposed securely to the AI agent
    weather_data = {
        "New York": "Sunny, 25°C",
        "London": "Rainy, 14°C",
        "Tokyo": "Cloudy, 18°C"
    }
    return weather_data.get(location, "Location not found")

# Simulating how an agent discovers available tools
def list_tools():
    return [
        {
            "name": "get_current_weather",
            "description": "Get the current weather for a given city.",
            "parameters": {"type": "object", "properties": {"location": {"type": "string"}}}
        }
    ]
```

### Step 2: The Agent Using the Tool
```python
# Simulating the AI Agent making a decision to use the MCP tool
print("AI Agent initialized...")
available_tools = list_tools()
print(f"Agent discovered tools: {[t['name'] for t in available_tools]}")

# The user asks a question
user_query = "What is the weather like in Tokyo?"
print(f"\nUser: {user_query}")

# The Agent decides to call the 'get_current_weather' tool
tool_to_call = "get_current_weather"
argument = "Tokyo"

if tool_to_call == "get_current_weather":
    result = get_current_weather(argument)
    print(f"Agent received context from MCP Server -> Result: {result}")
    print(f"AI Final Response: The current weather in Tokyo is {result}.")
```

---

## Lab 2: Using a Vector Database with Python

Let's build a mini memory system for an AI agent using **ChromaDB** (a popular, beginner-friendly open-source vector database) and sentence transformers.

### Step 1: Install Dependencies (Run in your terminal)
```bash
pip install chromadb sentence-transformers
```

### Step 2: Write the Python Code for Agent Memory
```python
import chromadb

# 1. Initialize the Vector Database (In-memory for testing)
chroma_client = chromadb.Client()

# 2. Create a collection (think of this like a table or folder for memories)
agent_memory = chroma_client.get_or_create_collection(name="agent_long_term_memory")

# 3. Add some "memories" (facts the agent has learned)
agent_memory.add(
    documents=[
        "User's favorite programming language is Python.",
        "Project Alpha deadline is scheduled for November 15th.",
        "User prefers dark mode for all user interfaces.",
        "The database password is encrypted using AES-256."
    ],
    ids=["doc1", "doc2", "doc3", "doc4"]
)

print("Memories successfully stored in Vector Database!\n")

# 4. Simulate an Agent querying its memory based on a user question
user_question = "What does the user like to code in?"
print(f"User Query: '{user_question}'")

# Search the vector database for the most semantically similar document
results = agent_memory.query(
    query_texts=[user_question],
    n_results=1 # Get the single best match
)

print("\n--- Vector Database Search Result ---")
print(f"Retrieved Memory: {results['documents'][0][0]}")
```

### 💡 What happened here?
Even though the user didn't use the exact words *"favorite programming language"*, the vector database understood the semantic meaning of the question and successfully retrieved the correct memory (`doc1`) from its storage. This is how modern AI agents remember context across different chat sessions!

---
*Happy Coding & Experimenting with your AI Agents!*
