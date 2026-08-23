# Langchain-Agentic-AI
LangChain is a software toolkit that connects powerful artificial intelligence models to a company's internal data, databases, and business tools. It turns a smart text model into a practical worker that can remember past tasks, search company files, and execute multi-step workflows instead of just answering basic prompts.

# Table of Contents
Project Structure
Installation
Usage
Chapter Overview
Database Example
Dependencies
Contributing
License

# Project Structure
Langchain-Agentic-AI/
│   main.py
│   pyproject.toml
│   README.md
├── 1. Getting Started with LLMs
│   ├── 1_LLM_Call.ipynb
│   ├── 2_Messages.ipynb
│   └── 3_Structured_Outputs.ipynb
├── Building Chains
│   ├── 1_first_chain.ipynb
│   ├── 2_chain_with_customRunnable.ipynb
│   ├── 3_parallel_chains.ipynb
│   └── 4_conditional_chains.ipynb
├── Agents & Database Integration
│   ├── 1_ReAct_Agent_Intro.ipynb
│   ├── 2_React_DB_Agent.ipynb
│   ├── init_db.py
│   └── SalesDB/


# AI Agent vs Agentic AI

| Aspect | AI Agent | Agentic AI |
|---|---|---|
| Definition | A single autonomous system for a specific task | A system of multiple agents working together |
| Scope | Narrow, task-specific | Broad, multi-step, complex goals |
| Structure | One agent, one loop (perceive→decide→act) | Multiple agents coordinating/delegating |
| Autonomy | Task-level | System-level (planning + orchestration) |
| Example | Chatbot answering FAQs | Multi-agent pipeline managing a full project |

# Installation

1. **Clone the repository:**
```bash
git clone https://github.com/tapan-chauhan/langchain-agentic-ai.git
cd langchain-agentic-ai
```

2. **Create a virtual environment (recommended):**
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. **Install dependencies:**
```bash
pip install -r requirements.txt
# Or use poetry/pip with pyproject.toml
```

# Usage

- **Run the main script:**
```bash
python main.py
```

- **Explore the Jupyter notebooks:** Open any notebook in the folders to follow the project interactively.

# Database Example

```sql
CREATE TABLE IF NOT EXISTS orders (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    customer_name TEXT NOT NULL,
    product_name TEXT NOT NULL,
    quantity INTEGER NOT NULL,
    price REAL NOT NULL,
    total REAL NOT NULL
)
```

# Dependencies

Key dependencies (see `pyproject.toml`):

- `langchain`
- `langchain-community`
- `langchain-core`
- `langchain-openai`
- `load-dotenv`
- `openai`
- `wikipedia`

<img width="1623" height="815" alt="image" src="https://github.com/user-attachments/assets/ec7e468a-c977-4219-9e75-d8dd3a83725b" />
<img width="1792" height="966" alt="image" src="https://github.com/user-attachments/assets/efd9dcc1-51d2-4484-95ce-405ed23a3ce0" />
<img width="1653" height="921" alt="image" src="https://github.com/user-attachments/assets/8baf58e3-ab07-4045-801a-55e8eaa3c96f" />
<img width="1539" height="490" alt="image" src="https://github.com/user-attachments/assets/2ae48283-694d-4b0a-a7b4-5441bbdd9a71" />
<img width="914" height="943" alt="image" src="https://github.com/user-attachments/assets/0158c1d6-86f7-4143-be36-354dd2d4212a" />
