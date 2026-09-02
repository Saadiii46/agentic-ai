# Practical Guide: Vector Databases & MCP in Agentic AI

Welcome to the hands-on practical section! In this guide, we will implement two hands-on components:
1. **Vector Database Memory System** using ChromaDB and Sentence Transformers.
2. **Model Context Protocol (MCP)** conceptual architecture implementation in Python.

---

## Part 1: Vector Database with Python & ChromaDB

Let's build a simple memory system for an agent using **ChromaDB** (a popular, lightweight vector database) and **Sentence Transformers**.

### Step 1: Install Dependencies
Open your terminal and run:
```bash
pip install chromadb sentence-transformers
```

### Step 2: Write the Python Code
Create a file named `agent_memory.py` and paste the following code:

```python
import chromadb
from sentence_transformers import SentenceTransformer

# 1. Initialize the embedding model (converts text to vectors)
print("Loading embedding model...")
embedding_model = SentenceTransformer('all-MiniLM-L6-v2')

# 2. Initialize ChromaDB (creates a local vector database)
chroma_client = chromadb.Client()

# Create a collection (like a table in traditional databases)
collection = chroma_client.create_collection(name="agent_long_term_memory")

# 3. Add some memories/knowledge for our agent
agent_memories = [
    "The university library closes at 10 PM on weekdays.",
    "To reset your student portal password, visit the IT helpdesk in Building B.",
    "Professor Smith's office hours are on Tuesdays from 2 PM to 4 PM.",
    "The cafeteria serves vegan pizza every Wednesday."
]

# Convert texts to vectors (embeddings)
embeddings = embedding_model.encode(agent_memories).tolist()

# Store them in the vector database with unique IDs
collection.add(
    documents=agent_memories,
    embeddings=embeddings,
    ids=["doc1", "doc2", "doc3", "doc4"]
)

print("\n--- Memory Successfully Stored in Vector Database! ---\n")

# 4. Simulate an Agent Querying its Memory
# Let's say a user asks the agent: "Where do I go to fix my password?"
query_text = "Where do I go to fix my password?"

# Convert the query into a vector
query_embedding = embedding_model.encode([query_text]).tolist()

# Search the vector database for the closest match
results = collection.query(
    query_embeddings=query_embedding,
    n_results=1 # Get the top 1 result
)

print(f"Agent Query: '{query_text}'")
print(f"Retrieved Memory from Vector DB: {results['documents'][0][0]}")
```

### Run it:
```bash
python agent_memory.py
```
> **Note:** Notice how the agent didn't look for the word "fix" or "password" strictly, but understood the semantic meaning to retrieve the IT helpdesk rule.

---

## Part 2: Understanding MCP Concepts with Python

While setting up a full production MCP server requires specific configuration, let's look at the **conceptual architecture** of how an Agent interacts with an MCP Server using Python pseudo-code.

### Conceptual Code Structure:
In an MCP setup, the client (Agent) asks the server: *"What tools do you have?"* The server responds with a list of capabilities, and the agent can then call them.

```python
# Conceptual MCP Client-Server Interaction
class SimpleMCPServer:
    """This represents an external tool provider (e.g., a local file reader)."""
    def list_tools(self):
        return [
            {
                "name": "read_file",
                "description": "Reads the content of a local text file.",
                "parameters": {"filepath": "string"}
            }
        ]

    def call_tool(self, tool_name, arguments):
        if tool_name == "read_file":
            # Safely execute the tool action
            return f"Contents of {arguments['filepath']}: [Sample File Data]"

# --- Agent Side ---
server = SimpleMCPServer()

# 1. Agent discovers available tools dynamically via MCP standard
available_tools = server.list_tools()
print(f"Agent discovered MCP Tool: {available_tools[0]['name']}")

# 2. Agent decides to use the tool based on user request
tool_result = server.call_tool("read_file", {"filepath": "syllabus.txt"})
print(f"Agent received data via MCP: {tool_result}")
```

### Why MCP is a Game-Changer for Agents:
* **Standardization:** You don't need unique code for every AI model provider.
* **Security:** MCP servers run locally or in secure sandboxes, preventing rogue agents from executing arbitrary harmful code on your machine.
* **Modularity:** You can plug and unplug data sources seamlessly.
