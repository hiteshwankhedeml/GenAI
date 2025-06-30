# Building Stateful Agentic AI graph - nodes

ai\_news\_node.py

```python
from tavily import TavilyClient
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate

class AINewsNode:
    def __init__(self,llm):
        """
        Initialize the AINewsNode with API keys for Tavily and OpenAI.
        """
        self.tavily = TavilyClient()
        self.llm = llm
        # this is used to capture various steps in this file so that later can be use for steps shown
        self.state = {}

    def fetch_news(self, state: dict) -> dict:
        """
        Fetch AI news based on the specified frequency.
        
        Args:
            state (dict): The state dictionary containing 'frequency'.
        
        Returns:
            dict: Updated state with 'news_data' key containing fetched news.
        """
        frequency = state['messages'][0].content.lower()
        self.state['frequency'] = frequency
        time_range_map = {'daily': 'd', 'weekly': 'w', 'monthly': 'm', 'year': 'y'}
        days_map = {'daily': 1, 'weekly': 7, 'monthly': 30, 'year': 366}
        
        response = self.tavily.search(
            query="Top Artificial Intelligence (AI) technology news India and globally",
            topic="news",
            time_range=time_range_map[frequency],
            include_answer="advanced",
            max_results=15,
            days=days_map[frequency],
            # include_domains=["techcrunch.com", "venturebeat.com/ai", ...]  # Uncomment and add domains if needed
        )
        state['news_data'] = response.get('results', [])
        self.state['news_data'] = state['news_data']
        return state

    def summarize_news(self, state: dict) -> dict:
        """
        Summarize the fetched news using an LLM.
        
        Args:
            state (dict): The state dictionary containing 'news_data'.
        
        Returns:
            dict: Updated state with 'summary' key containing the summarized news.
        """
        news_items = self.state['news_data']
        prompt_template = ChatPromptTemplate.from_messages([
            ("system", """Summarize AI news articles into markdown format. For each item include:
- Date in **YYYY-MM-DD** format in IST timezone
- Concise sentences summary from latest news
- Sort news by date wise (latest first)
- Source URL as link
Use format:
### [Date]
- [Summary](URL)"""),
            ("user", "Articles:\n{articles}")
        ])
        
        articles_str = "\n\n".join([
            f"Content: {item.get('content', '')}\nURL: {item.get('url', '')}\nDate: {item.get('published_date', '')}"
            for item in news_items
        ])
        
        response = self.llm.invoke(prompt_template.format(articles=articles_str))
        state['summary'] = response.content
        self.state['summary'] = state['summary']
        return self.state
    
    def save_result(self,state):
        frequency = self.state['frequency']
        summary = self.state['summary']
        filename = f"./AINews/{frequency}_summary.md"
        with open(filename, 'w') as f:
            f.write(f"# {frequency.capitalize()} AI News Summary\n\n")
            f.write(summary)
        self.state['filename'] = filename
        return self.state
```

basic\_chatbot\_node.py

```python
from src.langgraphagenticai.state.state import State

class BasicChatbotNode:
    """
    Basic chatbot logic implementation.
    """
    def __init__(self,model):
        self.llm = model

    def process(self, state: State) -> dict:
        """
        Processes the input state and generates a chatbot response.
        """
        return {"messages":self.llm.invoke(state['messages'])}
```

chatbot\_with\_Tool\_node.py

```python
from src.langgraphagenticai.state.state import State

class ChatbotWithToolNode:
    """
    Chatbot logic enhanced with tool integration.
    """
    def __init__(self,model):
        self.llm = model

    def process(self, state: State) -> dict:
        """
        Processes the input state and generates a response with tool integration.
        """
        user_input = state["messages"][-1] if state["messages"] else ""
        llm_response = self.llm.invoke([{"role": "user", "content": user_input}])

        # Simulate tool-specific logic
        tools_response = f"Tool integration for: '{user_input}'"

        return {"messages": [llm_response, tools_response]}
    
    def create_chatbot(self, tools):
        """
        Returns a chatbot node function.
        """
        llm_with_tools = self.llm.bind_tools(tools)

        def chatbot_node(state: State):
            """
            Chatbot logic for processing the input state and returning a response.
            """
            return {"messages": [llm_with_tools.invoke(state["messages"])]}

        return chatbot_node
 
        

        
    
    
    
    
    
```
