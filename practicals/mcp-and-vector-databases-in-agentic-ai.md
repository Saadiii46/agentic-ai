# 🛠️ Hands-On Practical: MCP & Vector Database Memory for Agents

In this practical guide, we will walk through two hands-on conceptualizations and examples:
1. **MCP Setup Conceptualization** (File System Reader)
2. **Building a Vector Memory for an Agent using ChromaDB**

---

## Part 1: Conceptualizing an MCP Setup

Imagine you want your AI agent to read files from your local computer safely.

1. **The MCP Server:** You run a local server (created using the MCP SDK) that only has permission to read files inside a specific folder called `/documents`.
2. **The Agent (Client):** The agent decides it needs to read `notes.txt`. It sends an MCP request: `tools/call -> read_file(path="notes.txt")`.
3. **The Result:** The server executes the safe read, returns the text to the agent, and the agent uses it to answer your question.

> 💡 *Note: In Python, you can build an MCP server using the official `mcp` library, defining tools with simple decorators like `@mcp.tool()`.*

---

## Part 2: Building a Simple Vector Memory for an Agent

Let's write a complete Python script using **ChromaDB** (a popular, lightweight vector database) and sentence embeddings to give an AI agent a searchable memory.

### 📋 Prerequisites (Terminal installation)
```bash
pip install chromadb sentence-transformers
```

### 💻 Python Code Implementation
```python
import chromadb

# 1. Initialize the Chroma Vector Database client (saves locally in memory/disk)
chroma_client = chromadb.Client()

# 2. Create a collection (think of this as a table or folder for memories)
agent_memory = chroma_client.create_collection(name="agent_long_term_memory")

# 3. Add some memories/facts that the agent learns over time
agent_memory.add(
    documents=[
        "User prefers Python for backend development.",
        "User's favorite coffee is iced caramel macchiato.",
        "The primary database used in the project is PostgreSQL."
    ],
    ids=["doc1", "doc2", "doc3"]
)

# 4. Simulate the Agent searching its memory based on a user prompt
# Notice how the query doesn't use exact keywords from the documents!
query = "What does the user like to drink?"

results = agent_memory.query(
    query_texts=[query],
    n_results=1 # Give me the top 1 most relevant memory
)

# 5. Output what the agent retrieved
print("=" * 50)
print("🤖 AGENT MEMORY RETRIEVAL SYSTEM")
print("=" * 50)
print(f"User Query: {query}")
print(f"Agent Retrieved Memory: {results['documents'][0][0]}")
print("=" * 50)
```

### 🔍 Explanation of the Practical Code:
1. **`chromadb.Client()`**: Sets up our vector database engine.
2. **`agent_memory.add(...)`**: Behind the scenes, ChromaDB automatically converts our plain-text sentences into mathematical vectors (embeddings) using a default embedding model.
3. **`agent_memory.query(...)`**: When we ask *"What does the user like to drink?"*, the database compares the vector of our question to all stored vectors and instantly finds that it matches *"User's favorite coffee is iced caramel macchiato"*—even though the words "drink" and "coffee" are completely different!
