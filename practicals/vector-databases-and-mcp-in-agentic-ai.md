# 🧪 Practical: Building a Mini Vector Memory for an Agent & Exploring MCP Conceptual Frameworks

> **Topic:** Vector Databases & Model Context Protocol (MCP) in Agentic AI
>
> **Difficulty:** INTERMEDIATE
>
> **Technology:** Python, LangChain, FAISS, OpenAI
>
> **Estimated Time:** 45 minutes

---

## 🎯 Practical Objective

In this practical, you will build:

> A Python script using `langchain` and an in-memory vector store (`FAISS`) that gives an AI agent long-term memory and retrieval-augmented generation (RAG) capabilities, alongside exploring the conceptual architecture of MCP tools.

By completing this practical, you will learn how to:

- Initialize and populate an in-memory vector database using FAISS and OpenAI embeddings.
- Configure a retriever to search agent memories based on semantic similarity.
- Build a complete RAG chain combining prompt templates, LLMs, and memory retrieval.
- Understand how MCP servers expose tools, resources, and prompts to AI agents via standard schemas.

---

## 🧠 What You Should Know Before Starting

Before beginning this practical, you should understand:

- Basic Python programming and running scripts in a terminal.
- Fundamental concepts of LLMs and prompt templates.
- How API keys and environment variables work.

If these concepts are unfamiliar, review:

- Introduction to LangChain
- Working with LLM Prompts and Chains

---

# 🏗️ What Are We Building?

We are building a university AI Agent script that stores facts about courses, schedules, and staff in a vector database. When a user asks a question, the agent retrieves the exact relevant fact from its vector memory and formulates a precise answer. We will also explore how MCP standardizes tool definitions for agent ecosystems.

### Final Result

```text
Initializing Vector Database memory...

User Query: What time does the library close?
Agent Response: The university library closes at 10:00 PM on weekdays.
```

---

## 🛠️ Step-by-Step Implementation

### Step 1: Install Required Libraries
Open your terminal and install the required packages:
```bash
pip install langchain langchain-openai faiss-cpu openai
```
*(Note: Make sure you have your OpenAI API key set in your environment variables: `export OPENAI_API_KEY="your-key-here"`)*

### Step 2: Python Code Example for Vector Memory
Create a file named `agent_memory.py` and add the following code:

```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import OpenAIEmbeddings, ChatOpenAI
from langchain_community.vectorstores import FAISS
from langchain_core.runnables import RunnablePassthrough
from langchain_core.output_parsers import StrOutputParser

# 1. Simulate facts that our Agent needs to remember
agent_memories = [
    "Student ID 1045 belongs to Alex Smith, majoring in Computer Science.",
    "The university library closes at 10:00 PM on weekdays.",
    "The AI Agent assignment is due on Friday at midnight via GitHub.",
    "Professor Davis specializes in Reinforcement Learning and Multi-Agent Systems."
]

print("Initializing Vector Database memory...")
# 2. Convert text into vectors and store them in a Vector Database (FAISS)
embeddings = OpenAIEmbeddings()
vector_database = FAISS.from_texts(agent_memories, embedding=embeddings)

# 3. Create a retriever from our vector database
retriever = vector_database.as_retriever(search_kwargs={"k": 1})

# 4. Setup the LLM for our Agent
llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)

# 5. Define the prompt template for the Agent
template = """You are a helpful university AI Agent. Use the following context retrieved from your memory database to answer the student's question. If you don't know, say you don't know.

Context:
{context}

Question: {question}
Answer:"""

prompt = ChatPromptTemplate.from_template(template)

# 6. Build the Agent RAG Chain
rag_chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)

# 7. Test the Agent's Memory Retrieval
query = "What time does the library close?"
print(f"\nUser Query: {query}")
response = rag_chain.invoke(query)
print(f"Agent Response: {response}")
```

### Step 3: MCP Conceptual Tool Definition Example
Examine how an MCP server dynamically exposes tools to agents using standard JSON schemas:

```json
{
  "name": "read_student_database",
  "description": "Reads student records from the university SQLite database.",
  "inputSchema": {
    "type": "object",
    "properties": {
      "student_id": {
        "type": "string",
        "description": "The 4-digit student ID number."
      }
    },
    "required": ["student_id"]
  }
}
```
