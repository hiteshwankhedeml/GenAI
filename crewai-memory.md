# CrewAI - Memory

| Component             | Description                                                                                                                                                                                                      |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Short-Term Memory** | Temporarily stores recent interactions and outcomes using `RAG`, enabling agents to recall and utilize information relevant to their current context during the current executions.                              |
| **Long-Term Memory**  | Preserves valuable insights and learnings from past executions, allowing agents to build and refine their knowledge over time.                                                                                   |
| **Entity Memory**     | Captures and organizes information about entities (people, places, concepts) encountered during tasks, facilitating deeper understanding and relationship mapping. Uses `RAG` for storing entity information.    |
| **Contextual Memory** | Maintains the context of interactions by combining `ShortTermMemory`, `LongTermMemory`, and `EntityMemory`, aiding in the coherence and relevance of agent responses over a sequence of tasks or a conversation. |
