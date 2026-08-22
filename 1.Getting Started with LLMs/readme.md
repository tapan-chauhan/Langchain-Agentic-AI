<img width="1778" height="211" alt="image" src="https://github.com/user-attachments/assets/8b7a8457-70f8-4641-93ae-d35070dd07d4" />

# LLM API Calls: OpenAI SDK vs LangChain

## 1. Making a Direct API Call (OpenAI SDK)

- Import the `OpenAI` client from the `openai` package.
- Use the `os` module to fetch the API key from environment variables — **never hardcode API keys** (bad practice, risk of exposure).
  - `os.environ` returns a dictionary of all environment variables.
  - Use `.get()` (standard/safe way) or square-bracket notation to read a value.
- Create the client by passing the API key — this identifies *who* is making the API call.
- Call `client.chat.completions.create()` with:
  - `messages`: a list with `role` (e.g. `"user"`) and `content` (the actual prompt).
  - `model`: which model to use (e.g. `gpt-5-mini`).
- Print `response` (or `response.content`) to see the generated output.
- This is essentially how ChatGPT itself works under the hood: send a message → LLM generates a response → display it.

## 2. The Problem: Multiple Models, Multiple SDKs

- Different providers require different SDKs:
  - OpenAI → OpenAI Python SDK
  - Claude (by Anthropic) → Anthropic SDK
  - Gemini/Nano Banana → Google SDK
- Each SDK has its **own syntax and conventions**.
- Using 10–30+ models individually means learning and maintaining 10–30+ different SDKs → chaos.

## 3. The Solution: LangChain

- LangChain acts as a **unified wrapper/interface** connecting to many different LLM providers.
- Instead of juggling multiple SDKs, you only need **one SDK (LangChain)** to access all supported models.
- Before agentic frameworks (LangChain, LangGraph, etc.) existed, developers had to manage each provider's SDK separately — much more complex.

### Installing LangChain (OpenAI integration)
```bash
uv add langchain-openai
```
(Make sure your virtual environment is activated first.)

### Basic Usage
```python
from langchain_openai import ChatOpenAI

llm_openai = ChatOpenAI(model="gpt-5-mini", temperature=0)
response = llm_openai.invoke("Tell me a fun fact")
print(response.content)
```

- **Temperature** controls creativity:
  - `0` → deterministic, most probable/serious output.
  - Higher (e.g. `0.7`–`0.9`) → more random/creative output.
  - Use `0` for serious/consistent tasks.

### Response Format
- LangChain returns an **AIMessage** object (raw output), not just plain text.
- Three message types in LangChain:
  1. **AIMessage** — sent by the AI
  2. **SystemMessage**
  3. **HumanMessage**
- Use `.content` to extract just the text from the response.

### Using Other Models via LangChain
```python
from langchain_anthropic import ChatAnthropic

llm_anthropic = ChatAnthropic(model="claude-3-5-sonnet")
```
(Requires a separate API key from the Anthropic platform.)

### Newer Approach: `init_chat_model`
```python
from langchain.chat_models import init_chat_model

llm_openai = init_chat_model(model="gpt-5-mini")
response = llm_openai.invoke("Hello, how are you?")
```
- Recently added utility — no need to import provider-specific classes.
- Just specify the model name and it handles the rest.

## Key Takeaway
LangChain simplifies working with multiple LLM providers by offering **one consistent interface**, instead of requiring a separate SDK (and separate syntax) for each provider (OpenAI, Anthropic/Claude, Google/Gemini, etc.).
