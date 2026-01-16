---
hidden: true
---

# ✈️ Format Instruction vs Instructor - Differences

| Feature               | Format Instructions / PydanticOutputParser | Instructor API            |
| --------------------- | ------------------------------------------ | ------------------------- |
| Enforcement           | Relies on prompt, not strict               | Strict schema validation  |
| Invalid JSON Handling | Manual retries needed                      | Auto-retries & correction |
| Output                | Raw → parsed                               | Direct Pydantic object    |
| Best for              | Prototypes, experiments                    | Production apps           |
