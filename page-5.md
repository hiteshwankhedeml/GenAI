---
hidden: true
---

# ✈️ End to End Project

* Create python 3.12 env
* install neo4j 5.14
* Query ⇒ LLM ⇒ It will create cypher query ⇒ Query from GraphDB

```python
NEO4J_URI="neo4j+s://427c9a4f.daases.neo4j.io"
NEO4J_USERNAME="neo4j"
NEO4J_PASSWORD="rQJMGc6ongtdUh6iWZ27jOg5QeyqBcMq3JWxUJ3_Y"

import os
os.environ["NEO4J_URI"] = NEO4J_URI
os.environ["NEO4J_USERNAME"] = NEO4J_USERNAME
os.environ["NEO4J_PASSWORD"] = NEO4J_PASSWORD

from langchain_community.graphs import Neo4jGraph
graph=Neo4jGraph(url=NEO4J_URI,username=NEO4J_USERNAME,password=NEO4J_PASSWORD)
graph

## Dataset Moview 
moview_query="""
LOAD CSV WITH HEADERS FROM
'https://raw.githubusercontent.com/tomasonjo/blog-datasets/main/movies/movies_small.csv' as row

MERGE(m:Movie{id:row.movieId})
SET m.released = date(row.released),
    m.title = row.title,
    m.imdbRating = toFloat(row.imdbRating)
FOREACH (director in split(row.director, '|') | 
    MERGE (p:Person {name:trim(director)})
    MERGE (p)-[:DIRECTED]->(m))
FOREACH (actor in split(row.actors, '|') | 
    MERGE (p:Person {name:trim(actor)})
    MERGE (p)-[:ACTED_IN]->(m))
FOREACH (genre in split(row.genres, '|') | 
    MERGE (g:Genre {name:trim(genre)})
    MERGE (m)-[:IN_GENRE]->(g))


"""

graph.refresh_schema()
print(graph.schema)

import os
from dotenv import load_dotenv
load_dotenv()

groq_api_key=os.getenv("GROQ_API_KEY")

from langchain_groq import ChatGroq
llm=ChatGroq(groq_api_key=groq_api_key,model_name="Gemma2-9b-It")
llm

from langchain.chains import GraphCypherQAChain
chain=GraphCypherQAChain.from_llm(graph=graph,llm=llm,verbose=True)
chain

response=chain.invoke({"query":"Who was the director of the movie Casino"})
response

#> Entering new GraphCypherQAChain chain...
#Generated Cypher:
#MATCH (m:Movie {title:"Casino"})<-[:DIRECTED]-(p:Person)
#RETURN p.name
#Full Context:
#[{'p.name': 'Martin Scorsese'}]
#
#> Finished chain.
#{'query': 'Who was the director of the movie Casino',
# 'result': 'Martin Scorsese.  \n'}
```
