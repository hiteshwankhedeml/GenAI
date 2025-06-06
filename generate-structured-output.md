# Generate Structured Output

* To have control over the generated output json structure

```python
from langchain.output_parsers import StructuredOutputParser, ResponseSchema

schema=[
    ResponseSchema(name="first_fact", description="first fact about text"),
    ResponseSchema(name="second_fact", description="second fact about text"),
    ResponseSchema(name="third_fact", description="third fact about text"),
]

parser=StructuredOutputParser.from_response_schemas(schema)

template3 = PromptTemplate(
    template='Give 3 fact about {topic} \n {format_instruction}',
    input_variables=['topic'],
    partial_variables={'format_instruction':parser.get_format_instructions()}
)

chain = template3 | openai_model | parser

result=chain.invoke({"topic":topic})

print(result)
# {'first_fact': '....', 'second_fact': '.......', 'third_fact': '....'}

```
