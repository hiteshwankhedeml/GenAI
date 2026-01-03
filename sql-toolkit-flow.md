# 🟢 SQL Toolkit - Flow

```
User
  ↓
Agent (create_sql_agent)
  ↓
LLM-1 (Agent LLM)
  - Understands question
  - Decides SQL tool is needed
  ↓
SQL Tool
  ↓
LLM-2 (Tool LLM)
  - Sees DB schema
  - Generates SELECT query
  ↓
SQL Tool
  - Executes SQL
  ↓
Database
  - Returns rows
  ↓
LLM-1
  - Interprets result
  - Generates final answer
  ↓
User

```
