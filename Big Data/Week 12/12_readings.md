# Big Data Week 12
## [Graph Databases Chapter 3](https://hura.hr/wp-content/uploads/2016/10/Graph_Databases_2e_Neo4j-5.pdf)
### General
Labelled property graph consists of:
- nodes
- relationships
- properties
- labels

Nodes (often documents):
- contain properties (key-value pairs)
- can be tagged with labels, that group nodes together
- relationsships always are a directed named edge, can also have properties
### Cypher general
Cypher works by pattern matching. Because graphs are multidimensional and the command line is only one dimensional, we can use node names to refer to the same node.
```
(emil:Person {name:'Emil'})
<-[:KNOWS]-(jim:Person {name:'Jim'})
-[:KNOWS]->(ian:Person {name:'Ian'})
-[:KNOWS]->(emil)
```
The first part (e.g. *emil*) is the identifier for the shell, then followed by the type (*Person*) in the brackets then are conditions on properties (*{name:'Emil'}*, also called anchoring). The same also applies to relationships in the squared brackets.
```
MATCH	(a:Person {name:'Jim'})-[:KNOWS]->(b)-[:KNOWS]->(c),
		(a)-[:KNOWS]->(c)
RETURN b, c
```
Returns the friends of somebody named *Jim*, which also know each other.

Instead of the property condition we can also write:
```
MATCH 	(a:Person)-[:KNOWS]->(b)-[:KNOWS]->(c),
		(a)-[:KNOWS]->(c)
WHERE a.name = 'Jim'
RETURN b, c
```
Which returns the same results.

Each matching node is lazily bound to its identifier as the client iterates the results.

Cypher clauses:
- MATCH &rightarrow; query by pattern
- WHERE &rightarrow; filter matching results
- CREATE (UNIQUE) &rightarrow; create nodes and relationships (in a atomic fashion)
- MERGE &rightarrow; used to merge a new subdomain (graph) with an existing one and re-using as many original nodes as possible
- DELETE &rightarrow; remove nodes, relationships or properties
- SET &rightarrow; set property values
- FOREACH &rightarrow; perform an update for each specified element
- UNION &rightarrow; merge results from two+ distinct queries
- WITH &rightarrow; piping in Unix
- START &rightarrow; deprecated
- RETURN &rightarrow; return a result, also allows simple aggregating functions like sum/distinct/order/limit
```
MATCH (user:User)-[*1..5]-(asset:Asset)
```
The operator in the middle (*-[\*1..5]-*) matches one to five edges, regardless of direction and edge name.

Identifiers are only available in the query scope.

Cypher supports indexes of properties of nodes with the same labels.
```
CREATE INDEX ON :Venue(name)
```
```
MATCH 	(theater:Venue {name:'Theatre Royal'}),
		(newcastle:City {name:'Newcastle'}),
		(bard:Author {lastname:'Shakespeare'}),
		(newcastle)<-[:STREET|CITY*1..2]-(theater)<-[:VENUE]-()-[:PERFORMANCE_OF]->()-[:PRODUCTION_OF]->(play)<-[:WROTE_PLAY]-(bard)
RETURN DISTINCT play.title AS play
```
# Explain


### Put the real world into a RDBMS
A good strategy to digitalize a dataset into a RDBMS is to:
1. abstract the data types
1. abstract the relationships
1. normalise it as far as possible
1. denormalise this view to adapt to the use cases, to make them fast

Demands adapt and so must the database, for that, the data will have to *migrate* from time to time, which can take a lot of time and since databases are usually business-critical are also often business-critical.
### Puth the real world into a graph database
Graph databases are much more natural for humans than RDBMS, as can be seen that many RDBMS architectures also get sketched with graphs.
A good strategy to digitalize a dataset into a graph database is to:
1. abstract the entities to nodes
1. decide on important label
1. build the relationships with edges
Since graph databases are not as static as RDBMS migrations can be done much more natural and faster.
#Common Modeling Pitfalls
## [Graph Databases Chapter 6](https://hura.hr/wp-content/uploads/2016/10/Graph_Databases_2e_Neo4j-5.pdf)