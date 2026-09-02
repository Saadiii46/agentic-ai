# Module Documentation: Vector Databases & MCP in Agentic AI

Welcome, students! In this module, we will explore two crucial components that make modern Agentic AI systems powerful, smart, and connected: **Vector Databases** and the **Model Context Protocol (MCP)**. 

Since you have already covered the basics of agents in class, we will look at *how* agents remember things (Vector Databases) and *how* agents interact with external tools and data sources safely (MCP).

---

## Part 1: Vector Databases in Agentic AI

### 1. What is a Vector Database?
Imagine trying to search through millions of documents for the concept of "friendship." A traditional database looks for the exact word "friendship." But what if a document uses the words "loyalty," "bond," or "close relationship"? A traditional database would miss them.

A **Vector Database** stores data (text, images, audio) as **numbers** (called **vectors** or **embeddings**). 
* **Embeddings:** AI models (like embedding models) convert words or sentences into long lists of numbers that represent their *meaning*. 
* **Semantic Search:** Words with similar meanings are placed close to each other in a multi-dimensional mathematical space. When an agent searches a vector database, it finds things that are *conceptually similar*, not just exact word matches.

### 2. Why Do Agents Need Vector Databases?
Standard AI models (like LLMs) have a "context window limit"—they can only read a certain amount of text at one time. They also forget everything once a chat session ends. Vector databases act as the **Long-Term Memory** for Agentic AI.

* **Retrieval-Augmented Generation (RAG):** When an agent needs to answer a question about proprietary school documents or company policies, it searches the vector database to retrieve only the most relevant paragraphs and feeds them to the LLM.
* **Experience & Reflection:** Advanced agents can store their past successes and failures in a vector database. When facing a new task, the agent can search its memory: *"Have I solved a problem like this before? How did I do it?"*

---

## Part 2: MCP (Model Context Protocol) in Agentic AI

### 1. What is MCP?
As developers built more AI agents, they ran into a messy problem: Every time they wanted to connect an LLM to a tool (like GitHub, a SQL database, or Google Drive), they had to write custom APIs and connectors. If they changed the LLM, they had to rewrite the connectors.

**MCP (Model Context Protocol)** is an open standard created by Anthropic that acts like a **"USB-C port for AI applications."** 
* Just like USB-C lets any mouse, keyboard, or monitor plug into any laptop, **MCP lets any AI agent safely connect to any data source or tool** using a standardized protocol.

### 2. How MCP Works
An MCP architecture consists of two main parts:
1. **MCP Hosts (The Agents/LLMs):** The AI application that wants to access data or execute tools (e.g., Claude Desktop, a custom LangChain agent).
2. **MCP Servers:** Lightweight programs that expose specific data or tools (e.g., a File System MCP Server, a Database MCP Server, a Slack MCP Server).

Instead of writing custom integration code for every tool, an agent simply speaks the MCP language to discover what tools are available and how to use them.

---

## Chapter Summary
* **Vector Databases:** Serve as the long-term, semantic memory for agents, enabling them to search through vast amounts of information using meaning rather than exact keywords.
* **MCP (Model Context Protocol):** Acts as a universal standard (like USB-C) that allows agents to securely and easily plug into various external tools and data sources without custom boilerplate code.
