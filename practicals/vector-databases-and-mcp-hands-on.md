# 🛠️ Practical Guide: Building Agent Memory with Vector Databases & MCP

Welcome to the hands-on practical module! Here, you will write functional code to implement an **AI Agent Memory System** using a vector database, and explore a conceptual client-server architecture for the **Model Context Protocol (MCP)**.

---

## Part 1: Building Agent Memory with ChromaDB

Let’s write a Python script using `chromadb`—a lightweight, beginner-friendly vector database—to see how an AI agent can store and retrieve knowledge based on semantic meaning.

### Step 1: Install Required Libraries
Open your terminal and install the required packages:
```bash
pip install chromadb sentence-transformers
```

### Step 2: Run the Agent Memory Python Script
Create a file named `agent_memory.py` and add the following code:

```python
import chromadb

def run_agent_memory_demo():
    print("🚀 Initializing Agent Vector Database Client...")
    
    # 1. Initialize the Vector Database client (saves in memory for this demo)
    client = chromadb.Client()

    # 2. Create a "collection" (analogous to a table in a relational database)
    agent_memory = client.create_collection(name="student_agent_memory")

    # 3. Add some documents (knowledge base) to the agent's memory
    print("📥 Storing knowledge into agent memory...")
    agent_memory.add(
        documents=[
            "Alice is studying Computer Science and loves Python.",
            "Bob is majoring in Graphic Design and loves Photoshop.",
            "Agentic AI is a system where AI models act autonomously to achieve goals.",
            "Vector databases store data as numerical embeddings for similarity search."
        ],
        ids=["doc1", "doc2", "doc3", "doc4"]
    )

    # 4. The Agent receives a user query and searches its vector database for meaning
    query = "Tell me about autonomous AI systems."
    print(f"\n🔍 User Query: '{query}'")

    results = agent_memory.query(
        query_texts=[query],
        n_results=1 # Request the single most relevant document
    )

    # 5. Output the retrieved memory
    print("\n✨ Agent Retrieved Memory:")
    print(results['documents'][0][0])

if __name__ == "__main__":
    run_agent_memory_demo()
```

> **Observation:** Notice how the query `"Tell me about autonomous AI systems."` successfully retrieves `"Agentic AI is a system..."` without relying on exact keyword matching!

---

## Part 2: Simulating Model Context Protocol (MCP) Flow

While full production MCP servers involve asynchronous JSON-RPC communication over standard input/output (stdio) or HTTP, we can understand the underlying protocol structure through a clear Python simulation.

### MCP Client-Server Interaction Script
Create a file named `mcp_simulation.py` and run it:

```python
import json

class MCPServerSimulator:
    """Simulates an MCP Server providing secure local filesystem tools."""
    
    def handle_request(self, request_json):
        request = json.loads(request_json)
        
        if request.get("protocol") == "mcp" and request.get("action") == "call_tool":
            tool_name = request.get("tool_name")
            args = request.get("arguments", {})
            
            if tool_name == "read_file":
                path = args.get("path")
                # Simulated secure file read
                if path == "./notes.txt":
                    return json.dumps({
                        "status": "success",
                        "content": "Agentic AI uses loops of Perception, Reasoning, and Action."
                    })
                else:
                    return json.dumps({"status": "error", "message": "File not found"})
                    
        return json.dumps({"status": "error", "message": "Unknown protocol or action"})


class MCPClient:
    """Simulates an AI Agent acting as an MCP Client."""
    
    def __init__(self, server: MCPServerSimulator):
        self.server = server
        
    def query_tool(self, tool_name, arguments):
        # Construct standardized MCP request
        mcp_request = {
            "protocol": "mcp",
            "action": "call_tool",
            "tool_name": tool_name,
            "arguments": arguments
        }
        
        # Send request to server
        response_json = self.server.handle_request(json.dumps(mcp_request))
        return json.loads(response_json)

def run_mcp_demo():
    print("🔌 Initializing MCP Simulation...")
    
    server = MCPServerSimulator()
    client = MCPClient(server)
    
    # Agent requests file content through standard MCP interface
    print("🤖 Agent requesting file content via MCP...")
    response = client.query_tool("read_file", {"path": "./notes.txt"})
    
    print("\n📦 MCP Server Response:")
    print(json.dumps(response, indent=2))

if __name__ == "__main__":
    run_mcp_demo()
```

---

## 🎯 Practical Takeaways
1. **ChromaDB Integration:** You can easily spin up local vector stores to give your Python agents semantic search capabilities.
2. **Standardized Protocol (MCP):** MCP replaces custom integration spaghetti code with clean, standardized client-server tool calls.
