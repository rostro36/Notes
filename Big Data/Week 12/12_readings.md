# Big Data Week 12 readings
## [Graph Databases Chapter 3](https://hura.hr/wp-content/uploads/2016/10/Graph_Databases_2e_Neo4j-5.pdf)
### Labelled property graphs general
Labelled property graphs consist of:
- nodes
- relationships
- properties
- labels

Nodes (often documents):
- contain properties (key-value pairs)
- can be tagged with labels, that group nodes together
Edges (relationships):
- can also have properties

### Cypher general
Cypher works by pattern matching. Because graphs are multidimensional and the command line is only one dimensional, we can use node names to refer to the same node.
```
(emil:Person {name:'Emil'})
<-[:KNOWS]-(jim:Person {name:'Jim'})
-[:KNOWS]->(ian:Person {name:'Ian'})
-[:KNOWS]->(emil)
```
The first part (e.g. *emil*) is the identifier for the shell, then followed by the type (*Person*) in the brackets then are conditions on properties (*{name:'Emil'}*, also called anchoring). The same also applies to relationships in the squared brackets.

Each matching node is lazily bound to its identifier as the client iterates the results and are only available in the query scope.
### Cypher clauses
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
### Cypher examples
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
### Cypher regex
```
MATCH (user:User)-[*1..5]-(asset:Asset)
```
The operator in the middle (*-[\*1..5]-*) matches one to five edges, regardless of direction and edge name.

```
MATCH 	(theater:Venue {name:'Theatre Royal'}),
		(newcastle:City {name:'Newcastle'}),
		(bard:Author {lastname:'Shakespeare'}),
		(newcastle)<-[:STREET|CITY*1..2]-(theater)<-[:VENUE]-()-[:PERFORMANCE_OF]->()-[:PRODUCTION_OF]->(play)<-[:WROTE_PLAY]-(bard)
RETURN DISTINCT play.title AS play
```
The first three parts act as anchors and are reused in the big query. *[:STREET|CITY\*1..2]* will match streets or cities never, once or twice.*()* is called an anonymous node that matches every node. *Distinct* is used in the end to eliminate duplicates. 

### Put the real world into a RDBMS
A good strategy to digitalize a dataset into a RDBMS is to:
1. abstract the data types
1. abstract the relationships
1. normalise it as far as possible
1. denormalise this view to adapt to the use cases, to make them fast

Demands adapt and so must the database, for that, the data will have to *migrate* from time to time, which can take a lot of time and since databases are usually business-critical are also often business-critical.
### Put the real world into a graph database
Graph databases are much more natural for humans than RDBMS, as can be seen that many RDBMS architectures also get sketched with graphs.
A good strategy to digitalize a dataset into a graph database is to:
1. abstract the entities to nodes
1. decide on important label
1. build the relationships with edges

Since graph databases are not as static as RDBMS migrations can be done much more natural and faster as for new demands, we usually only add new elements and do not have to change the whole model/schema.

Cypher supports indexes of properties of nodes with the same labels.
```
CREATE INDEX ON :Venue(name)
```

## [Graph Databases Chapter 6](https://hura.hr/wp-content/uploads/2016/10/Graph_Databases_2e_Neo4j-5.pdf)
### Graph databases general
A graph database has native processing capabilities if it exhibits index-free adjacency, each node maintains direct references to adjacent nodes, instead of global indexes, which also means not the whole graph has to be scanned, only the neighbourhood of a node.

Being index-free, the joins are precomputed and saved as relationship edges. Neo4j can walk through the edges in O(1) per node.

Neo4j is optimised for idiomatic, graph-local queries, that begin their traversal from one or more start points.
### Neo4j physical layout
Nodes get saved in the node store file, where each node has the length 15 bytes, which enables fast searches by id.
- The 1. byte is the in-use flag, whether the node is in the graph or (to be) deleted.
- The next 4 bytes is a pointer to the first relationship to that node.
- The next 4 bytes is a pointer to the first property of that node.
- The next 5 bytes point to the labels.
- The last 1 extra byte is reserved for flags, which may indicate if the node is densely connected.

Relationships are stored in the relationship store file of size 35 bytes:
- The 1. byte is the in-use flag, whether the edge is in the graph or (to be) deleted.
- The next 4 bytes are the origin node.
- The following 4 bytes are the target node.
- Then there are 4 bytes that point to the relationship type.
- Then there is 4 bytes that point to the next relationship of the origin node.
- Then there is 4 bytes that point to the previous relationship of the target node.
- Then there is 4 bytes that point to the next relationship of the target node.
- Then there is 4 bytes that point to the previous relationship of the origin node.
- In the end there is 1 byte to show whether the relationship is the first in the chain.
The doubly linked list is also great for deleting elements, as they only have to update the *next* pointer of their predecessor and the *previous* of their successor, that they store themselves.

Properties are stored in a singly linked list, where each property record consists of four property blocks, that each store one property and the ID of the next property record. This means each property record can store at most four properties.
A property record stores:
- the property type
- a pointer to the property index file, where the property name is stored
- for each property value, the record contains either a pointer into a dynamic store record (for big values) or an inlined value.
	- Where inlining is preferred, as there is less pointers to be chased, but they need more space and may need another property block.

Neo4j uses LRU caching.
### APIs
![APIs](../images/12_APIs.PNG)

1. Kernel API (transaction even handlers, make sure transactions are atomic)
1. Core API is an imperative Java API that exposes the graph primitives to the user, who then can lazily read it or write in an **ACID** fashion.
	- The core API has to be fine-tuned with knowledge of the graph and can get very verbose, but can be quite performant, if this is done.
1. Traversal Framework is a declarative Java API. Everything can be specified, how exactly we want to handle a newly visited node or we can also code to use the lower level Core API functions.
	- Takes less effort and code, but may be less performant.
1. Cypher is simpler and more general than all of them.

### Guarantees
**Transactions** are **atomic** with locks obtained at every node or relationship needed for the query. If the write fails, nothing gets written, if the write succeeds, the new data gets flushed to disk for durability. To speed this up, again a write ahead log is used.

**Recoverability** makes sure that a server crash does not affect availability or quality of data, only throughput. To gracefully recover Neo4j reads back the transaction log (write ahead log) and then asks it's peers or the master for newer transactions.

**Availability** is important to make sure that clients can always write. To do so Neo4j uses a master-slave architecture, where the master administers writes and sends them out to the slaves, that do read. It is also possible to send the write to a slave, but he first has to catch up and inform the master, before the write can be marked as successful. There is much less contention on the objects, as they are can be concurrently started at different start nodes. 

**Scalability** ensures that we can store huge graphs, with a good latency and good throughput. Neo4j is capped by their fixed ID sizes at still billions of nodes, where the more complex a query the sooner the graph part is beneficial to scalability. Latency is constant to the result, what is (nearly) optimal. Also throughput gets a lot easier, as write is a simple add to a list and read does not have to join a whole table only find the matching node and follow the pointers from there.

One way to shard is to use synthetic keys and application code that then has to join them afterwards. 