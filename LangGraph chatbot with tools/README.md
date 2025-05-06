
# LangGraph Chatbot with Wikipedia and Arxiv Tools (Gemma 2-9b)

This project implements a conversational AI chatbot using `LangGraph`, powered by the `Groq` LLM (Gemma2-9b-it). The chatbot is enhanced with two external tools for dynamic information retrieval:
- **Wikipedia**: For general knowledge questions.
- **Arxiv**: For scientific paper lookups.

## 🔧 Features
- Integrates Wikipedia and Arxiv using Langchain's community tools.
- Handles user input, decides whether tool invocation is needed, and responds accordingly.
- Uses Groq API with the `Gemma2-9b-it` model for intelligent responses.
- Demonstrates a state-machine architecture using `LangGraph`.

## 📦 Requirements

Install the required packages:

```bash
pip install arxiv wikipedia langchain langgraph langchain-groq
```

If running in Colab, also enable the `userdata` secret manager:

```python
from google.colab import userdata
```

## 🧠 How It Works

1. The system receives user input.
2. It checks if the question needs an external tool (Wikipedia or Arxiv).
3. Calls the appropriate tool and fetches the result.
4. Responds to the user using the `Gemma2-9b-it` model from Groq.

## 🧪 Sample Interaction

**Input:**
```
Who is the president of India?
```

**Output:**
```
The current president of India is Droupadi Murmu.
```

## 🔑 Environment Variables

Set the following environment variables before running:

```python
os.environ["LANGSMITH_API_KEY"] = "your_langsmith_api_key"
os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_PROJECT"] = "Courselanggraph"
```

And pass your `GROQ_API_KEY` from Google Colab's `userdata` or manually set it.

## 📂 Structure

- `Wikipedia_tool` - Queries Wikipedia
- `Arxiv_tool` - Searches scientific papers
- `ChatGroq` - Handles LLM invocation
- `LangGraph` - Builds state machine for chatbot flow

## 🚀 Run the Bot

Stream the output using:

```python
events = graph.stream({'messages': [('user', user_input)]}, stream_mode='values')
for event in events:
    event["messages"][-1].pretty_print()
```

## 📜 License

MIT License
