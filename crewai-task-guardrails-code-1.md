---
hidden: true
---

# CrewAI - Task GuardRails - Code

* &#x20;

```python
from crewai import Task, Agent

summary_agent = Agent(
    role="Summary Agent",
    goal="Summarize the research paper 'Convolutional Neural Networks' in 200 words.",
    backstory="You are a specialized agent that summarizes research papers.",
    verbose=True,
    llm=llm
)

summary_task = Task(
    description="Summarize a research paper in 200 words.",
    expected_output="A concise research summary 200 words.",
    agent=summary_agent,
    guardrail=validate_summary_length,
    max_retries=3
)
from crewai import Crew

summary_crew = Crew(
    agents=[summary_agent],
    tasks=[summary_task],
    verbose=True
)

result = summary_crew.kickoff()
╭──────────────────────────────────────────── Crew Execution Started ─────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Started                                                                                         │
│  Name: crew                                                                                                     │
│  ID: 9d5105c6-f8bc-49f9-9f5d-a6fbacbcd230                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
└── 📋 Task: fd7339e1-b541-4eba-a77d-0e88bb9f9239
       Status: Executing Task...
🚀 Crew: crew
└── 📋 Task: fd7339e1-b541-4eba-a77d-0e88bb9f9239
       Status: Executing Task...
    └── 🤖 Agent: Summary Agent
            Status: In Progress
# Agent: Summary Agent
## Task: Summarize a research paper in 200 words.


# Agent: Summary Agent
## Final Answer: 
Convolutional Neural Networks (CNNs) are a class of deep neural networks primarily used for image recognition and processing. Their architecture is inspired by the visual cortex and consists of convolutional layers, pooling layers, and fully connected layers. Convolutional layers use filters to extract features from the input image through convolution operations. Pooling layers reduce the spatial dimensions of the feature maps, decreasing computational complexity and providing translation invariance. Common activation functions like ReLU introduce non-linearity.

CNNs excel at automatically learning hierarchical representations of images, eliminating the need for manual feature engineering. They are spatially invariant, meaning they can recognize objects regardless of their position in the image. Beyond image recognition, CNNs are applied in areas like video analysis, natural language processing, and audio recognition. Their ability to learn complex patterns from data has made them a cornerstone of modern deep learning. The use of backpropagation enables the network to learn the optimal filter weights for feature extraction.


🚀 Crew: crew
└── 📋 Task: fd7339e1-b541-4eba-a77d-0e88bb9f9239
       Status: Executing Task...
    └── 🤖 Agent: Summary Agent
            Status: ✅ Completed
Validating summary length
Word count: 157
Summary exceeds 150 words
 Guardrail blocked, retrying, due to: Summary exceeds 150 words. Word count: 157

🚀 Crew: crew
├── 📋 Task: fd7339e1-b541-4eba-a77d-0e88bb9f9239
│      Status: Executing Task...
│   └── 🤖 Agent: Summary Agent
│           Status: ✅ Completed
└── 📋 Task: fd7339e1-b541-4eba-a77d-0e88bb9f9239
       Status: Executing Task...
🚀 Crew: crew
├── 📋 Task: fd7339e1-b541-4eba-a77d-0e88bb9f9239
│      Status: Executing Task...
│   └── 🤖 Agent: Summary Agent
│           Status: ✅ Completed
└── 📋 Task: fd7339e1-b541-4eba-a77d-0e88bb9f9239
       Status: Executing Task...
    └── 🤖 Agent: Summary Agent
            Status: In Progress
# Agent: Summary Agent
## Task: Summarize a research paper in 200 words.


# Agent: Summary Agent
## Final Answer: 
Convolutional Neural Networks (CNNs) are deep learning models primarily used for image recognition, inspired by the visual cortex. Their architecture comprises convolutional, pooling, and fully connected layers. Convolutional layers use filters to extract spatial features via convolution. Pooling layers reduce dimensionality, decreasing computation and increasing translation invariance. Non-linear activation functions, such as ReLU, are applied after convolution.

CNNs automatically learn hierarchical image representations, removing the need for manual feature engineering. They exhibit spatial invariance, recognizing objects regardless of location. Beyond image recognition, CNNs are used in video analysis, natural language processing, and audio recognition. Their ability to learn complex patterns makes them a cornerstone of modern deep learning. Backpropagation optimizes filter weights for feature extraction during training. The combination of these features makes CNNs highly effective for various tasks.


🚀 Crew: crew
├── 📋 Task: fd7339e1-b541-4eba-a77d-0e88bb9f9239
│      Status: Executing Task...
│   └── 🤖 Agent: Summary Agent
│           Status: ✅ Completed
└── 📋 Task: fd7339e1-b541-4eba-a77d-0e88bb9f9239
       Status: Executing Task...
    └── 🤖 Agent: Summary Agent
            Status: ✅ Completed
Validating summary length
Word count: 129
Summary is valid
🚀 Crew: crew
├── 📋 Task: fd7339e1-b541-4eba-a77d-0e88bb9f9239
│      Assigned to: Summary Agent
│      Status: ✅ Completed
│   └── 🤖 Agent: Summary Agent
│           Status: ✅ Completed
└── 📋 Task: fd7339e1-b541-4eba-a77d-0e88bb9f9239
       Status: Executing Task...
    └── 🤖 Agent: Summary Agent
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: fd7339e1-b541-4eba-a77d-0e88bb9f9239                                                                     │
│  Agent: Summary Agent                                                                                           │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭──────────────────────────────────────────────── Crew Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Completed                                                                                       │
│  Name: crew                                                                                                     │
│  ID: 9d5105c6-f8bc-49f9-9f5d-a6fbacbcd230                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```
