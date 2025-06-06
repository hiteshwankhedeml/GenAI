# Parser

* Chain is used to connect different components like prompt, agent, parser
* Parser means the data or convert the data as per the requirement
* Inside AI message we have content and several other metadata
* stroutput parser, json parser
* To validate we can use pydantic parser

```python
template=PromptTemplate(
    template="generate a prices 3 point summary from given text /n {text}",
    input_variables=["text"]
)

chain = template | openai_model

chain_parser = template | openai_model | parser

chain.invoke({"text":text}) # Here we will get entire AI Message
chain_parser.invoke({"text":text}) # Here we will get only the content
```
