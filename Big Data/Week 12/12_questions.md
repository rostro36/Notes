# Big Data Week 12 questions
## General
<details><summary>What are the five basic data shapes? </summary>

- Tables, Trees, Text, Graphs, Cubes

</details>

<details><summary>What are the NoSQL data shapes? </summary>

- Table Entity- Relationship model
- Columns
- Triples
- Document stores

</details>

## Mathematical graphs
<details><summary>What does a graph solve? </summary>

- Complex relations for traversals of relationships

</details>
<details><summary>What are the ingredients for a mathematical graph? </summary>

- n nodes
- m (directed) edges

</details>
<details><summary>What do nodes have in common? </summary>

- nothing, except that they are entities

</details>
<details><summary>How is a adjacency list built up? </summary>

- The node in one row and an array of the ends of its outgoing edges in the other column.

</details>
<details><summary>How is a adjacency matrix built up? </summary>

- A nxn big matrix with a 1, where there is an edge and a 0 if there is none.

</details>
<details><summary>How is a incidence matrix built up? </summary>

- A nxm big matrix with a 1, where the edge starts, a -1 where the edge ends and 0 if the edge is not connected to that node.

</details>

## Graphs in a database
<details><summary>What are the ingredients for a stored graph? </summary>

- n nodes
- m (directed) edges
- p properties which are attached to nodes or edges (object attributes, except for information which is stored in edges)
- l labels, which corresponds to the type of an edge or node, multiple labels are possible

</details>
<details><summary>What do edges represent in the graph model? </summary>

- Foreign keys

</details>
<details><summary>What are the building blocks of an RDF triple? </summary>

- subject (node)
- property (labelled edge)
- object (node)

</details>
<details><summary>Why is it easier to migrate with a graph instead of a normal RDBMS? </summary>

- In graphs, new relations can simply be added, while old ones can be deleted. In comparison to RDBMS, where the whole table and it's schema have to be adjusted.

</details>

## Entities in a W3 RDF
<details><summary>What are resources in W3 RDF? </summary>

- Entities stored in subject, property, object

</details>
<details><summary>How are resources stored/encoded? </summary>

- They are stored as URI (IRI) that can appear as subject, property and object.

</details>
<details><summary>What is a literal and where can it appear in a normal graph? </summary>

- It is an instance of direct types, e.g. strings, dates, numbers and XML Schema types, that can appear as objects.

</details>
<details><summary>What is a blank node and where can it appear in a normal graph? </summary>

- It is an empty node that can appear as subject or object.

</details>	
	
## Neo4j architecture
<details><summary>What is the architecture of Neo4j? </summary>

- Core servers (master)- read replicas (client)

</details>
<details><summary>What makes a core server in Neo4j? </summary>

- Read and write

</details>
<details><summary>What makes a read replica in Neo4j? </summary>

- Clients can read from it.
- Data replication

</details>
<details><summary>Do you always have to read from the core servers? </summary>

- No, you can read from the read replicas, but that can be disallowed.

</details>
<details><summary>When is synchronous data replication done? </summary>

- When a user updates a table he will be blocked until the data has been replicated on a majority of read replicas.

</details>
<details><summary>When is asynchronous data replication done? </summary>

- After the synchronous data replication, the data gets spread to the other minority of the read replicas.

</details>
<details><summary>Why is sharding hard in graphs? </summary>

- Most queries traverse a highly-connected graph and due to the polymorphy of edges, the system does not know how best to shard.

</details>
<details><summary>In Neo4j s Fabric node, where are nodes and edges stored? </summary>

- Nodes are stored everywhere but edges are stored more distributively separated.

</details>
<details><summary>What is the relationship between the four APIs of graph databases? </summary>

![APIs](../images/12_APIs.PNG)

The lower the level, the harder/verbose the code, but also faster.

</details>	

## Neo4j physical storage
<details><summary>What is index-free adjacency? </summary>

- Edges are saved on each node and not in a global index.

</details>
<details><summary>How are labels stored? </summary>

- Fixed list.

</details>
<details><summary>How are properties stored? </summary>

- As a list of key-value pairs.

</details>
<details><summary>What do we do if we have a very big label? </summary>

- Store a pointer to the very big label.

</details>
<details><summary>How are relationships(edges) stored? </summary>

- Two double linked list of edges at each end, because edges are not ordered in the data model and we need to discover all of them.
- source-previous, source-next
- target-previous, target-next

</details>

## Neo4j guarantees
<details><summary>Which guarantees does Neo4j provide? </summary>

- Atomicity
- Recoverability
- Availability
- Scalability (since graphs are by design local)

</details>
<details><summary>How is atomicity achieved in Neo4j? </summary>

- Data only gets written if it is wholly done, either by memory flush or written in the write ahead log.

</details>
<details><summary>How is recoverability achieved in Neo4j? </summary>

- After a crash, read the write ahead log and chatter with peers.

</details>
<details><summary>How is availability achieved in Neo4j? </summary>

- Master-slave architecture with core nodes and read replicas.

</details>

## RDF schemas
<details><summary>What can we do, if we want to limit/type RDF? </summary>

- RDF schemas

</details>	
<details><summary>What do we mean with RDF is self-aware? </summary>

- Meta-reflection of RDF schemas

</details>	