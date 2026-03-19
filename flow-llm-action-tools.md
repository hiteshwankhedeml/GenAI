---
hidden: true
---

# ✈️ Flow - LLM, Action, Tools

```sql
User Query
     ↓
LLM (under Agent)
     ↓
Does it require external action?
     ├── No ──▶ LLM generates direct response
     │             ↓
     │         Final Answer
     │
     └── Yes ─▶ Agent selects tool(s)
                   ↓
           LLM prepares tool input
                   ↓
              Tool executes
                   ↓
           Tool returns result
                   ↓
     LLM uses result to form response
                   ↓
             Final Answer
```

