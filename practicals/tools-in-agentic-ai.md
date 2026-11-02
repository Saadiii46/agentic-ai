# 🧪 Practical: Building a Tool-Using Agent

> **Topic:** Tools in Agentic AI
>
> **Difficulty:** Beginner
>
> **Technology:** Python / AI Agent Concepts
>
> **Estimated Time:** 45 minutes

---

## 🎯 Practical Objective

In this practical, you will build:

> A conceptual implementation of a tool-using AI agent that calculates the area of a rectangle using a custom Python function.

By completing this practical, you will learn how to:

- Define a custom Python function to act as an AI tool.
- Describe a tool using JSON schema for LLM tool-calling.
- Simulate the ReAct execution flow between an LLM and a Python function.
- Process tool outputs and format final user responses.

---

## 🧠 What You Should Know Before Starting

Before beginning this practical, you should understand:

- Basic Python functions (`def`, parameters, return values).
- The concept of LLMs and how they generate text.
- The ReAct (Reason + Act) framework for agentic workflows.

If these concepts are unfamiliar, review:

- Introduction to Large Language Models
- Tools in Agentic AI (Theory)

---

# 🏗️ What Are We Building?

We are building a step-by-step demonstration of how an AI model interacts with external tools. 
When a user asks: *"Hey, what is the area of a room that is 10 meters long and 5 meters wide?"*, the agent will:
1. Recognize it needs external calculation help.
2. Generate a structured tool call.
3. Execute the Python function `calculate_rectangle_area(10, 5)`.
4. Return the result back to the LLM to formulate a user-friendly response.

### Final Result

```text
User: Hey, what is the area of a room that is 10 meters long and 5 meters wide?
Agent Thought: I need to calculate the area using the rectangle area tool.
Tool Call: calculate_rectangle_area(length=10, width=5)
Tool Output: 50
Final Answer: The area of the room is 50 square meters.
```
