# LangGraph Function Calling — ReAct Agent

A ReAct (Reasoning + Acting) agent built with LangGraph and LangChain that uses function calling to answer questions with real-time web search and custom tools.

## How It Works

The agent follows a ReAct loop:

1. **Agent Reason** — The LLM receives the user message and decides whether to call a tool or respond directly.
2. **Act** — If a tool is needed, it executes the tool and feeds the result back to the agent.
3. **Repeat** — The loop continues until the agent has enough information to give a final answer.

```
[User Input] → agent_reason → (tool needed?) → act → agent_reason → ... → END
```

## Project Structure

```
├── main.py          # Entry point; defines the graph and runs the agent
├── nodes.py         # Agent reasoning node and tool node definitions
├── react.py         # LLM setup, tool definitions, and tool binding
├── pyproject.toml   # Project dependencies (managed by Poetry)
├── .env             # API keys (not committed)
└── flow.png         # Auto-generated graph visualization
```

## Tools

| Tool | Description |
|------|-------------|
| `TavilySearch` | Real-time web search (returns top 1 result) |
| `triple` | Custom tool that triples a given number |

## Prerequisites

- Python >= 3.10
- [Poetry](https://python-poetry.org/docs/#installation)
- OpenAI API key
- Tavily API key (get one at [tavily.com](https://tavily.com))

## Setup

**1. Clone the repo**
```bash
git clone https://github.com/StefanFinder/LangGraph-function-calling.git
cd LangGraph-function-calling
```

**2. Install dependencies**
```bash
poetry install
```

**3. Configure environment variables**

Create a `.env` file in the project root:
```
OPENAI_API_KEY=your_openai_api_key
TAVILY_API_KEY=your_tavily_api_key
```

## Running the Agent

```bash
poetry run python main.py
```

The agent will answer: *"What is the temperature in New York? List it and then triple it."*

To ask your own question, edit the `HumanMessage` content in `main.py`:
```python
res = app.invoke(MessagesState(messages=[HumanMessage(content="Your question here")]))
```

## Dependencies

- `langgraph` — Agent graph orchestration
- `langchain` — Core LangChain framework
- `langchain-openai` — OpenAI LLM integration
- `langchain-tavily` — Tavily web search tool
- `python-dotenv` — Environment variable loading
- `black` / `isort` — Code formatting
