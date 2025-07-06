# CrewAI - Knowledge

* Knowledge in CrewAI is a powerful system that allows AI agents to access and utilize external information sources during their tasks
* Enhance agents with domain-specific information
* Support decisions with real-world data
* Maintain context across conversations
* Ground responses in factual information

Knowledge Sources:



```python
#String knowledge source
from crewai.knowledge.source.string_knowledge_source import StringKnowledgeSource

# Define the knowledge
policy_text = """Our return policy allows customers to return any product within 30 days of purchase.
                 Refunds will be issued only if the item is unused and in original packaging.
                 Customers must provide proof of purchase when requesting a return."""

# Create a StringKnowledgeSource object
return_policy_knowledge = StringKnowledgeSource(content=policy_text)
from crewai import LLM

llm = LLM(model="gpt-4o")
from crewai import Agent

returns_agent = Agent(
    role="Product Returns Assistant",
    goal="Answer customer questions about return policy accurately.",
    backstory="You work in customer service and specialize in returns, refunds, and policies.",
    allow_delegation=False,
    verbose=True,
    llm=llm
)
from crewai import Task

returns_task = Task(
    description="Answer the following customer question about returns: {question}",
    expected_output="A concise and accurate answer.",
    agent=returns_agent
)
from crewai import Crew, Process

crew = Crew(
    agents=[returns_agent],
    tasks=[returns_task],
    process=Process.sequential,
    knowledge_sources=[return_policy_knowledge],  # This is key
    verbose=True
)
result = crew.kickoff(inputs={
    "question": "Can I get a refund if I used the item once?"
})

from pprint import pprint
pprint(result.raw)
Text Knowledge Source
from crewai.knowledge.source.text_file_knowledge_source import TextFileKnowledgeSource

text_source = TextFileKnowledgeSource(
    file_paths=["hr_policy.txt"]
)
from crewai import Agent, Task, Crew, Process, LLM

llm = LLM(model="gpt-4o")

hr_agent = Agent(
    role="HR Policy Assistant",
    goal="Answer employee questions about HR policies.",
    backstory="You're a reliable HR knowledge assistant.",
    knowledge_sources=[text_source],
    llm=llm
)

task = Task(
    description="What is the leave policy for new employees?",
    expected_output="A clear summary of the leave policy.",
    agent=hr_agent
)
crew = Crew(
    agents=[hr_agent],
    tasks=[task],
    process=Process.sequential,
    verbose=True
)

result = crew.kickoff()
pprint(result.raw)

#PDF source
from crewai.knowledge.source.pdf_knowledge_source import PDFKnowledgeSource

pdf_source = PDFKnowledgeSource(
    file_paths=["meeting_notes.pdf"]
)
meeting_summarizer = Agent(
    role="Meeting Note Summarizer",
    goal="Provide concise summaries of weekly meetings.",
    backstory="You help the team stay updated on discussions.",
    knowledge_sources=[pdf_source],
    llm=llm
)

task = Task(
    description="Summarize the key action items from last week's meeting.",
    expected_output="A bullet-point list of action items.",
    agent=meeting_summarizer
)
crew = Crew(
    agents=[meeting_summarizer],
    tasks=[task],
    process=Process.sequential,
    verbose=True
)

result = crew.kickoff()
pprint(result.raw)

#CSV source
from crewai.knowledge.source.csv_knowledge_source import CSVKnowledgeSource

csv_source = CSVKnowledgeSource(
    file_paths=["feedback.csv"]
)
feedback_analyst = Agent(
    role="User Feedback Analyst",
    goal="Identify common themes in user feedback.",
    backstory="You specialize in converting raw feedback into insights.",
    knowledge_sources=[csv_source],
    llm=llm
)

task = Task(
    description="What are the three most common complaints users had last month?",
    expected_output="A short list of recurring issues.",
    agent=feedback_analyst
)
crew = Crew(
    agents=[feedback_analyst],
    tasks=[task],
    process=Process.sequential,
    verbose=True
)

result = crew.kickoff()
pprint(result.raw)

#JSON source
from crewai.knowledge.source.json_knowledge_source import JSONKnowledgeSource

json_source = JSONKnowledgeSource(
    file_paths=["company_info.json"]
)
company_expert = Agent(
    role="Company Info Specialist",
    goal="Answer questions about company structure and data.",
    backstory="You are an internal data assistant for org-level queries.",
    # knowledge_sources=[json_source],
    llm=llm
)

task = Task(
    description="How many teams are working on the product and what are their names?",
    expected_output="A list of team names and their sizes.",
    agent=company_expert
)
crew = Crew(
    agents=[company_expert],
    tasks=[task],
    process=Process.sequential,
    verbose=True,
    knowledge_sources=[json_source]
)

result = crew.kickoff()
print(result)

#Custom embedding model
ollama_embedder = {
    "provider": "ollama",
    "config": {
        "model": "nomic-embed-text",  # Must match or be compatible with Ollama's supported embedding models
        "api_url": "http://localhost:11434"
    }
}
from crewai import Agent, LLM
from dotenv import load_dotenv
import os
load_dotenv()  # Load environment variables from .env file
#os.environ["OPENAI_API_KEY"] = os.getenv("GEMINI_API_KEY")


# llm = LLM(model="gemini/gemini-2.0-flash", verbose=True, temperature=0.5,
#           api_key=os.getenv("GEMINI_API_KEY"))
True
openrouter_llm = LLM(
    model='openrouter/qwen/qwq-32b:free',
    api_key=os.environ["OPENROUTER_API_KEY"],
    temperature=0
)
ollama_embedder={
        "provider": "google",
        "config": {
            "api_key": os.getenv("GEMINI_API_KEY"),
            "model" : "models/text-embedding-004"
            #"model": "models/embedding-001"
        }
    }
from crewai.knowledge.source.string_knowledge_source import StringKnowledgeSource

# Internal onboarding FAQ
faq_content = """
- You can access your email via portal.company.com using your employee credentials.
- The standard work hours are from 9am to 5pm, Monday to Friday.
- All reimbursement requests must be submitted by the 5th of the following month.
- For any IT-related issues, contact support@company.com.
"""

# Create a string knowledge source
faq_knowledge = StringKnowledgeSource(content=faq_content, embedder=ollama_embedder)
from crewai import Agent

hr_faq_agent = Agent(
    role="HR Assistant",
    goal="Answer onboarding-related questions for new hires.",
    backstory="You are a helpful assistant who knows everything about internal policies and onboarding processes.",
    allow_delegation=False,
    knowledge_sources=[faq_knowledge],
    verbose=True,
    embedder=ollama_embedder
)
from crewai import Task

task = Task(
    description="Answer this onboarding question: {question}",
    expected_output="A short, accurate answer based on internal HR documentation.",
    agent=hr_faq_agent,
    embedder=ollama_embedder
)
from crewai import Crew, Process

crew = Crew(
    agents=[hr_faq_agent],
    tasks=[task],
    #knowledge_sources=[faq_knowledge],
    embedder=ollama_embedder,
    process=Process.sequential,
    verbose=True
)

result = crew.kickoff(inputs={
    "question": "What are the working hours and how do I get reimbursed?"
})

from pprint import pprint
pprint(result.raw)
╭──────────────────────────────────────────── Crew Execution Started ─────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Started                                                                                         │
│  Name: crew                                                                                                     │
│  ID: 5e6a30f1-7a2a-4ec6-933c-ea64d4aebb2a                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
└── 📋 Task: 7fe8306e-d29e-4626-8b1a-06494e846287
       Status: Executing Task...
🚀 Crew: crew
└── 📋 Task: 7fe8306e-d29e-4626-8b1a-06494e846287
       Status: Executing Task...
    └── 🤖 Agent: HR Assistant
            Status: In Progress
# Agent: HR Assistant
## Task: Answer this onboarding question: What are the working hours and how do I get reimbursed?


# Agent: HR Assistant
## Final Answer: 
The standard work hours are from 9am to 5pm, Monday to Friday. To get reimbursed for any expenses, you must submit your reimbursement requests by the 5th of the following month. If you encounter any IT-related issues while accessing the reimbursement system, please contact support@company.com for assistance. You can access your email and further resources via portal.company.com using your employee credentials.


🚀 Crew: crew
└── 📋 Task: 7fe8306e-d29e-4626-8b1a-06494e846287
       Status: Executing Task...
    └── 🤖 Agent: HR Assistant
            Status: ✅ Completed
🚀 Crew: crew
└── 📋 Task: 7fe8306e-d29e-4626-8b1a-06494e846287
       Assigned to: HR Assistant
       Status: ✅ Completed
    └── 🤖 Agent: HR Assistant
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: 7fe8306e-d29e-4626-8b1a-06494e846287                                                                     │
│  Agent: HR Assistant                                                                                            │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭──────────────────────────────────────────────── Crew Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Completed                                                                                       │
│  Name: crew                                                                                                     │
│  ID: 5e6a30f1-7a2a-4ec6-933c-ea64d4aebb2a                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
('The standard work hours are from 9am to 5pm, Monday to Friday. To get '
 'reimbursed for any expenses, you must submit your reimbursement requests by '
 'the 5th of the following month. If you encounter any IT-related issues while '
 'accessing the reimbursement system, please contact support@company.com for '
 'assistance. You can access your email and further resources via '
 'portal.company.com using your employee credentials.')
```
