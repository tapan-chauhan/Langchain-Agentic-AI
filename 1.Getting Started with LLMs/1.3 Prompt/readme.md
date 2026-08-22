# LangChain Prompt

## Why Prompt Templates?
- Instead of hardcoding a message, we want to build prompts **dynamically** at runtime based on user input.
- Very similar in concept to Python f-strings — inserting a variable into a string template — but built as a dedicated, reusable LangChain class since prompts can be used for many purposes beyond simple string formatting.

## 1. `PromptTemplate` — Basic Dynamic Prompts (No Tone Control)

### Import
```python
from langchain_core.prompts import PromptTemplate
```

### Create a Dynamic Prompt
```python
user_input = input("Enter a topic for fun fact")

dynamic_prompt = PromptTemplate.from_template("Write a fun fact about {topic}")
```
- Printing `dynamic_prompt` alone only shows the **template structure** — it has an `input_variable` called `topic`, but the value isn't injected yet.

### Inject the Value
```python
ready_prompt = dynamic_prompt.invoke({"topic": user_input})
```
- The key (`topic`) in the dictionary **must match** the variable name used in the template.
- `ready_prompt` now contains the fully formed text, e.g. `"Write a fun fact about flowers"`.

### Send to the LLM
```python
response = llm.invoke(ready_prompt).content
```
- Full flow: user input → inject into template → `ready_prompt` created → pass to `llm.invoke()` → get response.

**Limitation:** `PromptTemplate` alone doesn't support setting a "tone" (i.e., no equivalent of a system message).

## 2. `ChatPromptTemplate` — Dynamic Prompts *With* Tone Control

Use this when you also want to set the system tone/persona (like telling "Simon" to *talk nicely* before talking to "Rahul").

### Import
```python
from langchain_core.prompts import ChatPromptTemplate
```

### Build the Message List
```python
prompt_template = ChatPromptTemplate.from_messages([
    ("user", "Write a fun fact about {topic}"),
    ("system", "You are a polite assistant")
])
```
- Instead of manually writing `SystemMessage(...)`, `HumanMessage(...)`, etc., you use simple tuples: `("user", "...")` and `("system", "...")`.
- `ChatPromptTemplate` automatically converts these tuples into the proper message objects behind the scenes.
- You can inspect the generated structure via `.messages` on the invoked result — it produces the same list of message objects as manually creating `SystemMessage`/`HumanMessage`, just with less boilerplate.

### Take Dynamic User Input
```python
user_input = input("Enter a topic")

ready_prompt = prompt_template.invoke({"topic": user_input})
```

### Send to the LLM
```python
response = llm.invoke(ready_prompt).content
```
- You don't need to manually break apart the messages — the LLM can handle the full `ready_prompt` object directly.
- (Optional, for advanced/token-conscious use: you can extract and pass only `.messages` to save tokens.)

## 3. Adding Multiple Dynamic Variables

You aren't limited to one variable — you can define as many as needed in a template:

```python
prompt_template = ChatPromptTemplate.from_messages([
    ("user", "Write a fun fact about {topic}"),
    ("system", "You are a {tone} assistant")
])

topic = input("Enter a topic")
tone = input("Enter a tone")

ready_prompt = prompt_template.invoke({"topic": topic, "tone": tone})
response = llm.invoke(ready_prompt).content
```

**Example:** topic = `"Microsoft"`, tone = `"funny"` → generates a fun fact about Microsoft written in a humorous tone.


- Both classes support injecting **any number of variables** into the template using `{variable_name}` placeholders and a matching dictionary passed to `.invoke()`.
- `ChatPromptTemplate.from_messages()` uses simple `("role", "text")` tuples instead of manually instantiating `SystemMessage`/`HumanMessage`/`AIMessage` objects — cleaner and less boilerplate.
- The final `ready_prompt` (from either class) can be passed directly into `llm.invoke()`.
