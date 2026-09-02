# 🧪 Practical: Vector Databases & MCP Implementation

> **Topic:** Vector Databases & MCP (Model Context Protocol) in Agentic AI
>
> **Difficulty:** INTERMEDIATE
>
> **Technology:** Python, ChromaDB, MCP Concepts
>
> **Estimated Time:** 30 minutes

---

## 🎯 Practical Objective

In this practical, you will build:

> A local vector search script using ChromaDB for agent memory, and a conceptual MCP tool definition schema.

By completing this practical, you will learn how to:

- Initialize a local ChromaDB vector database.
- Store text documents and automatically generate embeddings.
- Query a vector database based on semantic similarity.
- Understand MCP tool definitions and schemas for AI agents.

---

## 🧠 What You Should Know Before Starting

Before beginning this practical, you should understand:

- Basic Python programming.
- How embeddings represent text as numbers.
- The role of AI agents and tool calling.

If these concepts are unfamiliar, review:

- Introduction to Embeddings and Vector Search.
- Agent Tool Use and Protocols.

---

# 🏗️ What Are We Building?

We are building a Python script that stores agent knowledge in a vector database and queries it semantically, alongside a JSON schema demonstration of an MCP tool.

### Final Result

```text
User Query: What programming languages can you use?

Retrieved Context from Vector DB:
I can write Python code, debug errors, and execute scripts.

Agent loaded MCP Tool: get_current_weather
Description: Get the current weather for a given city using the MCP weather server.
```

---

## 💻 Step-by-Step Implementation

### Step 1: Install Required Libraries

Run the following command in your terminal:

```bash
pip install chromadb sentence-transformers
```

### Step 2: Vector Search Implementation

Create a file named `vector_search.py` and add the following code:

```python
import chromadb

# 1. Initialize the Chroma Client (creates a local database)
chroma_client = chromadb.Client()

# 2. Create a collection (like a table in SQL)
collection = chroma_client.create_collection(name="agent_knowledge")

# 3. Add documents (data) to the collection
# Chroma automatically converts these text documents into vectors using a default embedding model!
collection.add(
    documents=[
        "My name is Agent Alpha, and my primary task is data analysis.",
        "I can write Python code, debug errors, and execute scripts.",
        "My operating system is Linux Ubuntu 22.04.",
        "I must always ask for user confirmation before deleting files."
    ],
    ids=["doc1", "doc2", "doc3", "doc4"]
)

# 4. Query the database (Ask a question as an agent would)
results = collection.query(
    query_texts=["What programming languages can you use?"],
    n_results=1 # Return the top 1 most similar document
)

# 5. Print the result
print("User Query: What programming languages can you use?")
print("\nRetrieved Context from Vector DB:")
print(results['documents'][0])
```

### Step 3: MCP Tool Definition Concept

Create a file named `mcp_tool_demo.py` and add the following code:

```python
# Conceptual representation of an MCP Tool Definition
# An MCP server exposes tools in a standardized JSON-RPC format like this:

weather_mcp_tool = {
    "name": "get_current_weather",
    "description": "Get the current weather for a given city using the MCP weather server.",
    "input_schema": {
        "type": "object",
        "properties": {
            "city": {
                "type": "string",
                "description": "The city name, e.g., Tokyo, New York"
            }
        },
        "required": ["city"]
    }
}

# When an Agent needs weather data, it reads this MCP tool definition,
# decides to call it, and sends a standardized request to the MCP Server.
print(f"Agent loaded MCP Tool: {weather_mcp_tool['name']}")
print(f"Description: {weather_mcp_tool['description']}")
```
