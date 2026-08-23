# ReAct Agents  & SQL Agent 

## ReAct Agent
- **ReAct = Reasoning + Acting.** LLM reasons about a task, takes an action (tool call), observes the result, and repeats until satisfied.
- LLMs alone can't access personal/org data, send emails, or query databases — so we give them **tools** (functions), grouped into a **toolkit**.
- **Tool binding** = connecting an LLM to a toolkit so it can call tools when needed.
- Flow: User input → LLM checks if it can answer directly → if not, picks a tool → tool runs → result goes back to LLM → LLM decides if done or needs another tool call → final answer.
- LLM never executes tools itself — it only *requests* a tool call (with arguments); the framework runs it and returns a `ToolMessage`.

```python
from langchain.agents import create_agent
from langchain_community.tools import DuckDuckGoSearchRun

agent = create_agent(model="gpt-5-mini", tools=toolkit)
result = agent.invoke({"messages": [{"role": "user", "content": "latest stock market news"}]})
```

- Built-in tools available via LangChain integrations (DuckDuckGo, Wikipedia, Gmail, etc.) — no need to build from scratch.
- Custom tools: use `@tool` decorator + a **docstring/description** (critical — LLM picks tools based on their description).
- Manual binding: `llm.bind_tools(toolkit)` — shows the LLM proposing a tool call instead of an answer.

## SQL ReAct Agent
- Built using `SQLDatabase` (from a connection URI) + `SQLDatabaseToolkit` (LLM + DB) + `create_agent`.
- Built-in SQL tools: list tables, get schema, generate query, run query.
- LLM uses the DB schema as context to write and execute correct SQL — good column/table descriptions improve accuracy (reduces hallucination).
- Must pass input as `{"messages": [...]}`, not a plain string.

```python
from langchain_community.utilities import SQLDatabase
from langchain_community.agent_toolkits import SQLDatabaseToolkit
from langchain.agents import create_agent

db = SQLDatabase.from_uri("sqlite:///salesDB/sales.db")
toolkit = SQLDatabaseToolkit(db=db, llm=llm_openai)
sql_agent = create_agent(model=llm_openai, tools=toolkit)

result = sql_agent.invoke({"messages": [{"role": "user", "content": "total sales for tablet?"}]})
```

<img width="986" height="473" alt="image" src="https://github.com/user-attachments/assets/d154c75f-3da1-4c08-8690-85f48d4ddbdf" />

<img width="1561" height="905" alt="image" src="https://github.com/user-attachments/assets/9e54768d-8b98-4414-9ddd-20f922dc0c0c" />




