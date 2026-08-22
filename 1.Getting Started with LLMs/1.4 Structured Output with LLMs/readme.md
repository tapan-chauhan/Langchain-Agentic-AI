# Structured Output with LLMs (Pydantic, TypedDict)

## Why Structured Output?
- LLM output is often used by **downstream functions/workflows** that expect a **fixed schema** (specific keys, specific data types).
- LLMs don't always follow a format consistently unless guided.
- Three approaches (from weakest to strongest guarantee): **prompt guiding** → **TypedDict** → **Pydantic**.

## 1. Guiding via Prompt (Simplest, Least Reliable)
- Just instruct the LLM in plain text to return output in a certain format.
```python
result = llm_openai.invoke(
    "Tell me a joke. Generate the output in key-value pair format with keys 'setup' and 'punchline'."
)
print(result.content)
```
- Works for simple prompts, but unreliable for complex/dynamic workflows — LLM may not always follow it.

## 2. Pydantic — Data Validation + Parsing Library
- **Pydantic** = a Python library for **data validation** and **data parsing**.
- Enforces a **strict schema** — like a strict school **principal**: if you break the rule (wrong key or wrong data type), it fails immediately (raises `ValidationError`).

### Define a Schema
```python
from pydantic import BaseModel, Field

class LLMSchema(BaseModel):
    setup: str = Field(description="setup for the joke")
    punchline: str = Field(description="punchline for the joke")
```
- `BaseModel` is the class you inherit to define your schema (keys + data types).
- `Field(description=...)` gives the LLM extra **context** on what each field should contain — important since LLMs rely on context to generate good output.

### Validation Behavior
- Missing a required key → `ValidationError`.
- Wrong data type (e.g., passing an int where a string is expected) → `ValidationError`.
- Only valid, matching data creates a real object.

### Use with an LLM
```python
llm_structured = llm_openai.with_structured_output(LLMSchema)
result = llm_structured.invoke("Tell me a joke")

print(result.punchline)   # direct attribute access, e.g. result.punchline
print(type(result))       # <class '__main__.LLMSchema'>
```
- No need to manually instruct the LLM about format in the prompt — `with_structured_output()` enforces it.
- Result is a **Pydantic object**, not a plain string/dict — access fields via dot notation (`result.setup`).

## 3. TypedDict — Lighter-Weight Alternative
- Same idea as Pydantic (define keys + types) but **does not enforce validation** — like a lenient **class teacher** who lets mistakes slide.

```python
from typing import TypedDict

class LLMSchemaTD(TypedDict):
    setup: str
    punchline: str
```

- Creating an object with a wrong key (e.g. `"ketchup"` instead of `"setup"`) does **not** raise a runtime error — it's just a **type-hinting** issue (caught by static type checkers/IDEs, not at runtime).
- Result is a plain **dictionary**, not an object — access via `result["setup"]` or `result.get("setup")`.

```python
llm_typed = llm_openai.with_structured_output(LLMSchemaTD)
result = llm_typed.invoke("Tell me a joke")
```

## Pydantic vs TypedDict — When to Use Which

| Aspect | Pydantic | TypedDict |
|---|---|---|
| Validation | Strict — raises errors on mismatch | None — errors are type-hint only, not runtime |
| Output type | Pydantic object (`result.key`) | Plain dictionary (`result["key"]`) |
| Analogy | Strict principal | Lenient class teacher |
| Best for | Strict, error-sensitive workflows where schema must be exact | Providing context/structure when strict enforcement isn't critical |

**Rule of thumb:** Use **Pydantic** when you need a guaranteed, exact schema for downstream dependencies. Use **TypedDict** when you just want to hint structure/context without hard enforcement.





