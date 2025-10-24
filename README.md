# Hybrid Agent Memory Workflow: SQL Database + Markdown Guides

Boost your AI agent’s reliability and transparency by combining persistent memory (using *any SQL database*) and human-readable Markdown (.md) workflow guides. Whether you’re building with Claude, local PostgreSQL, Cloud SQL, MySQL, or SQLite, this repo helps you design smarter code assistants and troubleshooting bots.

---

## 🚀 Overview

- **Persistent Memory**: Store your agent’s key decisions, bug logs, and lessons learned in a SQL database.
- **Markdown Workflow Guides**: Instruct bots and humans *how* and *when* to capture and retrieve memories, using editable `.md` files.

This hybrid approach works locally, in the cloud, and across teams.

---

## 🧠 Why Hybrid Memory?
- **SQL databases** (Cloud SQL, Postgres, MySQL, SQLite): Durable, queryable, structured memory for agents.
- **Markdown docs**: Live workflow standards, easy to audit and update.

---

## 🛠️ Database Setup (Generic)

Create a table for your agent memories. Example (works for PostgreSQL, MySQL, SQLite):

CREATE TABLE agent_memories (
id INTEGER PRIMARY KEY AUTOINCREMENT, -- SERIAL for Postgres; AUTO_INCREMENT for MySQL
project_id VARCHAR(100),
file_path VARCHAR(255),
user VARCHAR(100),
memory_type VARCHAR(50),
timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
content TEXT
);

---

## 📝 Workflow Guide (.md Example)

Create `agent_workflows/agent_memory_workflow.md`:

Agent Memory Workflow Guide
When to Save Memories
After fixing bugs, refactors, or design decisions

How to Save
Use save_agent_memory() with project_id, file_path, user, memory_type, content

When to Retrieve
Call get_agent_memories() before changes

Safety Guidelines
Never overwrite; append new entries only

Flag ambiguous cases for human review

Example
save_agent_memory("projectA", "src/payments.py", "agent", "bug", "Resolved payment error with ENV config.")

---

## 👩‍💻 Universal Python Snippets

Works for SQLite, easy to adapt for Postgres/MySQL:

import sqlite3

def save_agent_memory(db_path, project_id, file_path, user, memory_type, content):
conn = sqlite3.connect(db_path)
cur = conn.cursor()
cur.execute("""
INSERT INTO agent_memories (project_id, file_path, user, memory_type, content)
VALUES (?, ?, ?, ?, ?)
""", (project_id, file_path, user, memory_type, content))
conn.commit()
cur.close()
conn.close()

def get_agent_memories(db_path, project_id, file_path=None, memory_type=None):
conn = sqlite3.connect(db_path)
cur = conn.cursor()
query = "SELECT content, timestamp FROM agent_memories WHERE project_id = ?"
params = [project_id]
if file_path:
query += " AND file_path = ?"
params.append(file_path)
if memory_type:
query += " AND memory_type = ?"
params.append(memory_type)
cur.execute(query, tuple(params))
rows = cur.fetchall()
cur.close()
conn.close()
return rows
---

## 📦 How to Use

1. **Integrate the Python/SQL code in your agent, bot, or automation tools.**
2. **Consult workflow guides (.md) before or during agent operation (inject them into prompts/guidance for Claude or other bots).**
3. **Update guides as best practices evolve—keep in Git for team access & version control.**

---

## 💡 Benefits

- Works on **local, cloud, or hybrid** stacks.
- Combine durable memory + readable workflow standards.
- Easy for new contributors to understand and extend.

---

## 🌍 Contributing / Sharing

- Fork, clone, or adapt for your stack (Cloud SQL, local Postgres, MySQL, etc.).
- Contribute new guides, safety protocols, or code snippets.
- Share with your team or on Dev.to, Medium, Reddit, or LinkedIn!

---

## 📣 Discussion

- Have questions, want more integrations, or see something to improve?
- Open an Issue or a Discussion in this repo, or [contact @datlaphani](https://github.com/datlaphani).

---
