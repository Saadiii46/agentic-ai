# 💻 Practical Guide: Building an Agent with Tools using Python

In this practical module, we will implement custom tools in code using **LangChain**—one of the industry-standard frameworks for Agentic AI. We will construct an AI agent equipped with a custom geometry calculation tool and a mock stock price retrieval tool.

---

## 🛠️ Prerequisites

Ensure your Python development environment is properly configured and install the required dependencies:

```bash
pip install langchain langchain-openai openai python-dotenv
```

---

## 📝 Step-by-Step Implementation

Below is a complete, production-ready Python script demonstrating how to define custom tools using decorators, initialize an LLM, construct a prompt template, and execute the agentic loop.

```python
import os
from dotenv import load_dotenv
from langchain.agents import AgentExecutor, create_openai_tools_agent
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_core.tools import tool
from langchain_openai import ChatOpenAI

# 1. Load environment variables (e.g., OPENAI_API_KEY)
load_dotenv()

# 2. Define Custom Tools using the @tool decorator
# 💡 IMPORTANT: The docstring and type hints are read by the LLM 
# to determine WHAT the tool does and WHEN it should be invoked.

@tool
def calculate_rectangle_area(length: float, width: float) -> str:
    """Calculates the area of a rectangle given its length and width.
    Use this tool whenever you need to find the area of a rectangular space.
    """
    area = length * width
    return f"The area is {area} square units."

@tool
def get_fake_stock_price(ticker: str) -> str:
    """Gets the current stock price for a given company ticker symbol.
    Example tickers: AAPL, TSLA, GOOGL.
    """
    ticker = ticker.upper()
    # Mock database of prices for demonstration purposes
    prices = {"AAPL": "$185.50", "TSLA": "$240.20", "GOOGL": "$140.10"}
    
    return prices.get(ticker, f"Sorry, ticker symbol '{ticker}' not found.")

# Group our tools into a callable list
tools = [calculate_rectangle_area, get_fake_stock_price]

# 3. Initialize the LLM (The Agent's Brain)
llm = ChatOpenAI(model="gpt-4o-mini", temperature=0)

# 4. Create the Prompt Template for the Agent
# Note: 'agent_scratchpad' tracks intermediate steps (Thought -> Tool Call -> Observation).
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant with access to specialized tools. Use them when necessary."),
    MessagesPlaceholder("chat_history", optional=True),
    ("human", "{input}"),
    MessagesPlaceholder("agent_scratchpad"),
])

# 5. Construct the Agent
agent = create_openai_tools_agent(llm, tools, prompt)

# 6. Wrap the agent in an AgentExecutor to handle the execution loop
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

# --- 🚀 LET'S TEST IT! ---
if __name__ == "__main__":
    print("=" * 60)
    print("--- Test 1: Using the Rectangle Area Tool ---")
    print("=" * 60)
    response1 = agent_executor.invoke({"input": "What is the area of a room that is 12.5 meters long and 4 meters wide?"})
    print("\n✅ Final Output:", response1["output"])

    print("\n" + "=" * 60)
    print("--- Test 2: Using the Stock Price Tool ---")
    print("=" * 60)
    response2 = agent_executor.invoke({"input": "Can you check the current stock price for AAPL?"})
    print("\n✅ Final Output:", response2["output"])
```

---

## 🎯 Running the Script

1. Create a `.env` file in your project root and add your OpenAI API key:
   ```env
   OPENAI_API_KEY=your_openai_api_key_here
   ```
2. Run your Python script:
   ```bash
   python agent_with_tools.py
   ```
3. Observe `verbose=True` in action as the agent reasons, calls the appropriate tool, and returns the final calculated answer!
