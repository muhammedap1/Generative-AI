
# 🤖 LangGraph Groq Chatbot

This is a conversational chatbot built using [LangGraph](https://python.langchain.com/docs/langgraph/), [LangChain](https://www.langchain.com/), and [Groq API](https://console.groq.com/). It utilizes the `Gemma2-9b-it` language model for fast and intelligent replies in a dynamic graph-based architecture.

---

## 📂 Project Structure

```text
.
├── chatbot_code.ipynb  # Your notebook file
├── README.md           # This file
```

---

## 🚀 Features

- Uses `LangGraph` to manage state in a flow-based architecture.
- Powered by Groq’s `Gemma2-9b-it` for high-performance inference.
- Keeps message history and context via `StateGraph`.
- Console-based interface (in Colab or local Jupyter/terminal).
- Graceful exit with `'quit'` or `'q'`.

---

## 📦 Requirements

Install these in your environment:

```bash
pip install langgraph langchain langchain-groq
```

Also install in Google Colab:

```python
!pip install langgraph langchain langchain-groq
```

---

## 🔐 Environment Setup (in Google Colab)

Make sure you store your secrets securely via:

```python
from google.colab import userdata

groq_api_key = userdata.get('groq_api_key')
langsmith = userdata.get('langsmith_api_key')
```

Then:

```python
import os

os.environ["LANGSMITH_API_KEY"] = langsmith
os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_PROJECT"] = "Courselanggraph"
```

---

## 🧠 Model Initialization

```python
from langchain_groq import ChatGroq

llm = ChatGroq(groq_api_key=groq_api_key, model_name="Gemma2-9b-it")
```

---

## ⚙️ LangGraph Construction

```python
from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages
from typing import Annotated
from typing_extensions import TypedDict

class State(TypedDict):
    messages: Annotated[list, add_messages]

graph_builder = StateGraph(State)

def chatbot(state: State):
    return {"messages": llm.invoke(state["messages"])}

graph_builder.add_node("chatbot", chatbot)
graph_builder.add_edge(START, "chatbot")
graph_builder.add_edge("chatbot", END)

graph = graph_builder.compile()
```

---

## 💬 Running the Chatbot

```python
while True:
    user_input = input("User: ")
    if user_input.lower() in ["quit", "q"]:
        print("Thank you for using our chatbot. Goodbye!")
        break
    for event in graph.stream({'messages': ('user', user_input)}):
        for value in event.values():
            print("bot:", value["messages"].content)
```

---

## 📝 Notes

- Ensure `groq_api_key` and `langsmith_api_key` are correctly added via Colab's secrets.
- LangGraph lets you manage conversation as flows, which is more powerful than sequential chains.

---

## 🧪 Example

```bash
User: Hello, who are you?
bot: I am an AI chatbot built using LangGraph and Groq!
```

