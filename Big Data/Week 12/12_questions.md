# Big Data Week 12
## Questions
- What are the five basic data shapes?
	- Tables, Trees, Text, Graphs, Cubes
- What are the NoSQL data shapes?
	- Table Entity- Relationship model
	- Columns
	- Triples
	- # one more
- What does a graph solve?
	- Complex relations for traversals of relationships
- What are the ingredients for a mathematical graph?
	- n nodes
	- m (directed) edges
- What do nodes have in common?
	- nothing, except that they are entities
- How is a adjacency list built up?
	- The node in one row and an array of the ends of its outgoing edges in the other column.
- How is a adjacency matrix built up?
	- A nxn big matrix with a 1, where there is an edge and a 0 if there is none.
- How is a incidence matrix built up?
	- A nxm big matrix with a 1, where the edge starts, a -1 where the edge ends and 0 if the edge is not connected to that node.
- What are the ingredients for a stored graph?
	- n nodes
	- m (directed) edges
	- p properties which are attached to nodes or edges (object attributes, except for information which is stored in edges)
	- l labels, which corresponds to the type of an edge or node, multiple labels are possible
- What do edges represent in the graph model?
	- Foreign keys
- What are the building blocks of an RDF triple?
	- subject (node)
	- property (labelled edge)
	- object (node)
- What are resources in W3 RDF?
	- Entities stored in subject, property, object
- How are resources stored/encoded?
	- as URI
# Blank Node, IRI, Literal distinction/restriction

- What is the architecture of Neo4j?
	- Core servers (master)- read replicas (client)
- Synchronous and asynchronous data replication is possible
- The whole graph is stored at every computer
- Do you always have to read from the core servers?
	- No, you can read from the read replicas, but that can be disallowed.

- What makes a core server?
	- Read and write?
- What makes a read replica?
	- Clients can read from it.
	- Data replication
- Writes are synchronous until the majority of core servers respond, they then replicate to the other machines in an asynchronous fashion.


- In Neo4j s Fabric node, where are nodes and edges stored?
	- Nodes are stored everywhere but edges are stored more distributively separated.
- How are labels stored?
	- Fixed list.
- How are properties stored?
	- As a list of key-value pairs.
- What do we do if we have a very big label?
	- Store a pointer to the very big label.
- How are relationships(edges) stored?
	- A double linked list of edges, because edges are not ordered in the data model and we need to discover all of them.
# bad wording
- How can edges be traversed?
	- s-previous, s-next, t-previous, t-next

- What can we do, if we want to limit/type RDF?
	- RDF schemas
- What do we mean with RDF is self-aware?
	- Meta-reflection of RDF schemas
