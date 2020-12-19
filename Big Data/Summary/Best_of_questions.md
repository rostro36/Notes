# Big Data Best of questions
## Week 02
<details><summary>CAP theorem </summary>

- No **C**onsistency: Just return your store/garbage.
- No **A**vailabilty: It takes forever.
- No **P**artition-tolerance: There is no partitioning.	

</details>
<details><summary>Symmetry vs. Heterogeneity? </summary>

- both if possible
- Symmetry means every system has the same responsibilities; there is no clear master.
- Heterogeneity allows that servers have different CPUs/hardware, but still in the same ballpark (e.g. 8GB vs 16 GB, not 4 KB). They have more/less load according to their exact hardware.
- Nodes are symmetric in responsibilities but heterogeneous in loads.	

</details>
<details><summary>R+W>N </summary>

- **N** amount of replicates in the system. 
- **R** amount of replicates that we need to get to successfully **read**. If **R** is big, **W** is low and the clients can **write** faster. 
- **W** amount of replicates that we need to get to successfully **write**. If **W** is big, **R** is low and clients can **read** faster.
- If both are high, we have a higher consistency, durability, but slower (increased latency) and worse availability.
- R+W>N: There has to be at least one node that has seen a write up-date that gets read. 	

</details>
<details><summary>Why are virtual nodes introduced?</summary>
	
- Easier to distribute/balance across nodes.
- Counter randomness a bit.		
		
</details>

<details><summary>Why do we need a vector-clock instead of timestamps? </summary>
	
- Ensure consistency.
	- Imagine a scenario with N=2, R=2, W=1. Client1 adds "hi" to node 1 and afterwards Client2 adds "bye" to node 2,
		without a synchronisation between the two nodes in between. If a client now reads it gets the two versions and can not tell if "bye" deleted "hi" first or just the scenario as above.
	- With vector-clocks this can be resolved, because the reading client sees, that they have not updated with each other; they are forks of each other.		
		
</details>
<details><summary>Why is a Merkle hash-tree used to check if two nodes have the same items? </summary>
	
- The Merkle tree allows for easier checks of the whole ring. The leafs are one key-range each and each parent is the hash of its children;
	like that a node only has to check the hashes it got from the sibling up to the top, instead of hashing other leafs.
- Like this it is easier to check if both copies of a key-range have the same items.		
		
</details>

## Week 03
<details><summary>Importance of Throughput, Capacity and Latency? </summary>

- Depends on the application, but Capacity can be much higher compared to Throughput, which can be bigger than Latency.
	
![Scaling behaviour](../images/03_scaling.PNG)

</details>
	
<details><summary>What is the advantage of having small blocks? </summary>

- It is easier to parallelize over different nodes/hard drives, but the search time to find them is larger, which impacts latency.

</details>
<details><summary>What is the advantage of having big blocks? </summary>

- We hardly have to search this block, but we have to send the whole block over the network, even if we only need a small part, we also have a higher chance if the block fails,
	also we use more storage if we can not fill the whole block with data.	

</details>
<details><summary>What is the difference between object storage and file file storage? </summary>

|**Object storage**|**File storage**
|-:|-:|
|Billions of TB files|Millions of PB files	|
|bad latency, better throughput	|better latency, worse throughput	|
|allows random access	|only allows scanning	|
|often only key-value, (get/put)	|file system exists	|
|offered as services by Amazon (or other) use with other people	|create&use cluster yourself	|

</details>
<details><summary>HDFS architecture: </summary>

![HDFS architecture](../images/03_architecture.PNG)

</details>
<details><summary>What should a NameNode do? </summary>

- File namespace +Access control (how the file system looks like)
- File to block mapping
- Block to location (node)	

</details>
<details><summary>How does a data block look like? </summary>

- Each data block consists of the data itself and it's metadata (checksums, generation stamp) in two separate files.	

</details>
<details><summary>What is the benefit of a higher replication factor? </summary>

- Higher replication is higher fault tolerance and increases read bandwidth.

</details>

## Week 04
<details><summary>What are the possible JSON values? </summary>

- Object, array, number, string, boolean, null 

</details>
<details><summary>How would you model an empty element in XML? </summary>

- With <*element* />

</details>
<details><summary>What data-structure does a JSON manifest? </summary>

- A dict, without duplicate keys. 

</details>
<details><summary>What does a number in JSON correspond to in Python? </summary>

- JSON is programming language independent and the standard only defines conformance, not how to interpret the text e.g. if an object is a list or an array.

</details>
<details><summary>What are possible JSON value types? </summary>

- A JSON value can be one of:
	- object
	- array 
	- number 
	- string
	- literal name token

</details>
<details><summary>What are the tokens in JSON?</summary>

- JSON text is formed out of strings, numbers and 9 tokens.

- 6 Structural tokens:
	- [
	- {
	- ]
	- }
	- :
	- ,

- 3 literal name tokens:
	- true
	- false
	- null

</details>
<details><summary>How are numbers represented in JSON? </summary>

![JSON numbers](../images/04_JSON_number.PNG)

</details>
<details><summary>What data-structure does a XML manifest? </summary>

- A tree.

</details>
<details><summary>How is the root level different to any other level in XML? </summary>

- There must be exactly one leaf in this level, not more, not less.

</details>
<details><summary>Does order matter in XML? </summary>

- The order of elements matters.
- The order of attributes does not matter. 

</details>
<details><summary>Which characters are legal for XML element names? </summary>

- Alphanumeric, special characters, "-","\_" and "." 

</details>
</details>
<details><summary>Of those legal characters, which can be at the start? </summary>

- Small and capital letter, "\_".

</details>
<details><summary>Which characters must be escaped in text? </summary>

- <, &

</details>	
<details><summary>What is the purpose of "<\!\[CDATA\[\"? </summary>

- You do not have to escape <,&, only the end tag of CDATA. The content in CDATA will be seen as text, no elements in there.

</details>
<details><summary>Is the tag "aTag" the same as "atag" in XML? </summary>

- No, Tags are case-sensitive.

</details>
<details><summary>What is the "forbidden sequence" of XML-comments? </summary>

- "--" can only be used to close with -->, else you have escape. 

</details>
<details><summary>What is a XML declaration there for and how does a sample look like? </summary>

- The XML declaration sets grounds for reading the XML file to follow.
- \<\?xml version="1.0" encoding="UTF-8" standalone="no" \?\>

</details>
<details><summary>How can the default namespace be changed? </summary>

- To change the namespace in the scope of the tag, you have to change the attribute *xmlns*.

</details>
<details><summary>How can the default namespace be changed? </summary>

- To change the namespace in the scope of the tag, you have to change the attribute *xmlns*.

</details>	
<details><summary>How are non-default namespaces defined? </summary>

- They are defined like normal attributes registered in the xmlns namespace, they can be used in the same tag as they are created.

</details>
<details><summary>Is there a difference between elements and attributes regarding namespaces? </summary>

- Yes, attributes do not have the default namespace function, you can only explicitly define namespaces on them. 

</details>

## Week 05
<details><summary>What are CRUD operations? </summary>

- **C**reate
- **R**ead
- **U**pdate (write)
- **D**elete 

</details>
<details><summary>Why can there be a total order in HBase? </summary>

- HBase supports ACID with locks, as there is exactly one RegionServer per row, this is (easier) possible.

</details>	
<details><summary>What is a region in HBase? </summary>

- A list of rows determined by a range of their RowID.

</details>
<details><summary>What is stored on the same physical machine in HBase? </summary>

- A *store*, a column family of the same region.

</details>
<details><summary>What is the hierarchy of entities in HBase? </summary>

- Table &rightarrow; Region &rightarrow; Store &rightarrow; HFile &rightarrow; HBlock &rightarrow; KeyValue
![Architecture](../images/05_layers.PNG)

</details>
<details><summary>Where are stores saved? </summary>

- They are saved in one or multiple HFiles in HDFS.

</details>
<details><summary>What are cells in HBase? </summary>

- Cells are timestamped (milliseconds passed since midnight, January 1, 1970 UTC) values of row x column, due to versioning, there may be many.

</details>
<details><summary>Why do regions exist in HBase? </summary>

- Regions are essentially contiguous ranges of rows stored together and are the partitions in HBase, each region has a region server.

</details>
<details><summary>What is in a key of a HFile? </summary>

- (RowID,columnID,version/timestamp)
![KeyValue](../images/05_keyvalue.PNG)

</details>
<details><summary>What is in a value of a HFile? </summary>

- One HFile consists of many 64kB big *HBlocks* of data to make it easier to search things.

</details>
<details><summary>Why does HBase not do redundancy? </summary>

- It is built on top of HDFS, which already does redundancy.

</details>
<details><summary>Why does HBase not do redundancy? </summary>

- It is built on top of HDFS, which already does redundancy.

</details>
<details><summary>How is data stored on persistent storage? </summary>

- In Log-structured merge-trees, which double in size for every level, and every level holds at most one node.
![LSM-tree](../images/05_LSM_tree.PNG)

</details>
<details><summary>What is the accuracy property of Bloom filters? </summary>

- Bloom filter give no false negatives, but can give false positives.

</details>

## Week 06
<details><summary>What is the difference between well-formed vs valid documents? </summary>

- Valid documents must adhere some schema and the language, well-formed documents must only be well-formed in the language. Every valid document must be well-formed.

</details>
<details><summary>Is a document without a schema valid? </summary>

- By definition a valid document must have a schema, if it does not have a schema it is neither valid nor invalid.

</details>
<details><summary>JSON vs. XML </summary>

![JSON vs XML](../images/06_JSON_vs_XML.PNG)

</details>
<details><summary>What is the default behaviour if a specified schema element does not exist in the document? </summary>

- If the element does not exist, the document is not valid.

</details>


Read through [06_readings](../week 06/06_readings.md) for the schemas.


<details><summary>In Dremel, what does *r* and *d* stand for? </summary>

- *r* is for repetition, which tells how many hops are shared between the current and previous path.
- *d* is for definition, which tells how big the whole path is.
- **Required fields are not counted.** 

</details>

<details><summary>Can you give a Dremel example? </summary>

![Dremel example](../images/06_dremel_example.PNG)

</details>

## Week 07
<details><summary>What is the difference between HBlock and a HDFS block? </summary>

- They are different levels, HBlocks are on the file level, HDFS blocks are on the (quasi-)physical level.
- HBlock is extended, so every key-value is in the same HBlock. A HDFS block has a fixed size and key-values will be cut if it has to be done. 	

</details>
<details><summary>What happens in the Map phase? </summary>

- Filter the necessary fields from the input Key-values into a new, intermediate Key-value relationship. (E.g. From lines of text to words and their counts)	

</details>
<details><summary>Why is there a Combine phase? </summary>

- Combine is an optional phase, that can be added to make sure less data has to be transmitted.	

</details>
<details><summary>What happens in the Combine phase? </summary>

- The mapper can already combine(reduce) the results for the first time, before sending each key-value pair to the reducers in the shuffle phase.	

</details>
<details><summary>What are the properties the Combine function must have? </summary>

- The function key-value input/output are be the same as in the reduce phase and the function is associative and commutative.	

</details> 
<details><summary>What happens in the Shuffle phase? </summary>

- The intermediate Key-values get sorted and then partitioned nicely for the next phase. (E.g. all counts of the same word go to the same reducer)	

</details>	
<details><summary>What happens in the Reduce phase? </summary>

- The function that we want to use will be applied to the sorted intermediate values. (E.g. add up all counts of the same word.) 	

</details>
<details><summary>What are the big O complexities of MapReduce? </summary>

- The first immediate results only depend on one shard (Map). O(1)
- The second immediate results depend on many other first immediate results (Shuffle). O(n^2)
- The final results only depend on one of the second immediate results (Reduce). O(1) 	

</details>	
<details><summary>Can we start the next phase after the first results of the previous phase are available? </summary>

- Hardly, as the next phase depends on the previous phase.

</details>	
<details><summary>What does the master do in the MapReduce framework? </summary>

- Keep track of the status of each of the *M* map and *R* reduce tasks (idle, in-progress, completed)
- The ID of the worker for the non-idle tasks.
- Store the location and size of the intermediate values (partially) completed by the map tasks.
- Transmit those locations to in-progress reduce tasks.
- Ping each worker to see if he has not failed.
- Balance jobs and re-assign failed(including jobs which have completed, but the data  was not already fetched by the following consumer) jobs.

</details>

## Week 08
<details><summary>What is a slot? </summary>

- A slot are some computing resources (memory+processor) that can execute jobs.	

</details>
<details><summary>What is the drawback of too few slots? </summary>

- Too few slots (*under-subscribed*) can be clogged by resource-light jobs, that do not need the whole share of resource.

</details>
<details><summary>What is the drawback of too many slots? </summary>

- Too many slots (*oversubscribed*) that makes the jobs to be pre-empted, because the resource is needed by another job. 

</details>
<details><summary>What are the problems of MapReduce? </summary>

- MapReduce only scales to thousands, not dozens of thousands.
- JobTracker is a single point of failure.
- JobTracker has to do two things: schedule and monitoring
- The amount of memory is static.
- Some resources are idle, because another phase gets executed.	

</details>
<details><summary>What does YARN make different? </summary>

- It adds another layer of master(*ResourceManager*)-client(*NodeManager*). A *NodeManager** may open a MapReduce JobTracker *container* and asks for more *containers*/slots.	

</details>
<details><summary>What is the workflow of a Spark job? </summary>

- The user builds the pipeline of transformations, but requesting the action then triggers the execution of that pipeline.	

</details>
<details><summary>What is an *ApplicationMaster*? </summary>

- The head *container* of an application (e.g. MapReduce), that also wants new storage for it's application. It also does the monitoring and fault tolerance of the application.

</details>
<details><summary>What does a *ResourceManager* do? </summary>

- They are the admin entry gate.
- It checks the heartbeats of the *NodeManager*s.
- It checks the heartbeats of the *ApplicationMaster*s.
- They balance and assign new resources demanded by the client or *ApplicationMaster*s.
- Authentication of *ApplicationMaster*s and users, to make sure the resources wasted by somebody else to ensure fairness.
- Is still not failure resistant.	

</details>
<details><summary>How can slots be scheduled? </summary>

- FIFO
- Capacity scheduling, where there are sub-groups that are weighted differently(in proportion to their capacity) and are merged to a bigger queue.
- Fair Scheduling, like capacity scheduling, but there is neither user nor weights.	

</details>
<details><summary>What is queue elasticity? </summary>

- If one user-group does not use it's whole capacity, it can be shared with another user-group.	

</details>
<details><summary>What is the difference between instantaneous fair share vs. steady fair share? </summary>

- Instantaneous fair share does exclude empty queues and shares the remaining resources according to steady fair share.	

</details>
<details><summary>What is pre-emption? </summary>

- Cut off a job, after it has taken too long.	

</details>
<details><summary>How does dominant resource fairness work? </summary>

- If there are multiple resources (e.g. memory and cores) categorize each sub-group by it's dominant resource.
		- The dominant resource is the one with the higher percentage need of the whole cluster.
- The final sharing then has to be normalized by the sum of dominant resources.
- **Another way** to look at it is that with that we entangle CPU percentage with memory percentage and use this resource then.	

</details>
<details><summary>What is the lifecycle of an RDD? </summary>

- **Creation:** Taken from the filesystem (local, S3, HDFS..)
- **Transformation:** Transform RDD to another RDD (e.g. MapReduce on a RDD)
- **Action:** Save the final output (not an RDD), this triggers all the computations before, because the previous computation were lazy.

</details>

## Week 09
<details><summary>In Spark, can one task of stage 2 run, when another of stage 1 is still running? </summary>

- No.

</details>
<details><summary>What are DataFrames? </summary>

- DataFrames are fixed length, stored columnarly, "0NF-tables", created from a SQL query and then transformed into RDDs. 

</details>
<details><summary>What are the benefits of DataFrames? </summary>

- Uses a lot less memory, can be created with SQL.

</details>
<details><summary>What is the difference in creation of a schema between a RDBMS and Spark? </summary>

- RDBMS have to define a schema and import data for the table, Spark reads directly from the data.

</details>
<details><summary>What is the bottleneck MapReduce solves? </summary>

- Disk I/O, with more disks.

</details>
<details><summary>What is latency? </summary>

- The time until the first result.

</details>
<details><summary>What is throughput? </summary>

- The amount of operations/mega bytes per second.

</details>
<details><summary>What is response time? </summary>

- It is the time until the results; the last operation= latency+transfer\**operations*

</details>
<details><summary>How is speed-up defined? </summary>

- old latency/new latency

</details>
<details><summary>What is the formula of Amdahl's law? </summary>

- Speed-up=1/(1-p+p/s), where *p* is the parallelisable part and *s* is the amount of parallel workers.

</details>
<details><summary>What is Gustafson's law? </summary>

- Speed-up=1-p+sp, , where *p* is the parallelisable part and *s* is the amount of parallel workers.

</details>

## Week 10
<details><summary>What is UTF-8? </summary>

- It is an encoding of the Unicode character catalogue.

</details>
<details><summary>What is tail latency? </summary>

- Stragglers/ that take significantly more time than other jobs.

</details>
<details><summary>What can be done against stragglers? </summary>

- Execute every job twice, but about costs twice as much.
- Start with the second/back-up job, once the first one takes too long.

</details>
<details><summary>What are the 3 basic integrities of RDBMs?  </summary>

- Atomic integrity (all entries are atomic values, not dicts or the like)
- Tabular/Relational integrity (all relations between tables are valid)
- Domain integrity (all entries in the same column have the same type)

</details>
<details><summary>What is the atomicity of MongoDB? </summary>

- Atomicity on one document, like the row-atomicity of HBase.

</details>
<details><summary>What is the document landscape that works best with document stores? </summary>

- Many small files of megabytes.

</details>
<details><summary>Document stores vs. RDBMs </summary>

- Good at projection and selection
- Passable at aggregation
- Bad at joins

- Also in document stores, you validate a document after you have populated.

</details>
<details><summary>What is the default document format in MongoDB? </summary>

- BSON, "JSON in binary", with a bit more data types.

</details>
<details><summary>What are *write concerns* in MongoDB? </summary>

- The difference between a synchronous (wait for ACK of all secondary nodes) and asynchronous (don't wait) action.

</details>
<details><summary>In MongoDB, if an object has an array, where the condition only holds on certain elements, will the condition hold for the whole object? </summary>

- By default, if one element of an array matches, this condition counts as fulfilled for the whole object.

</details>
<details><summary>How do indices work? </summary>

- Looking up the index in a table points to all the matching elements.

</details>
<details><summary>What is the best data structure for indices? </summary>

- A B+-tree, where the inner nodes are criteria and the leafs are the elements.

</details>
<details><summary>Why does it not make sense to have a index on *year* and one on *year* , *month*? </summary>

- *year* , *month* already contains an index on *year*, as it is a prefix.

</details>
<details><summary>Can we index the entries of an array in MongoDB? </summary>

- We can index every entry of one (not two+) array, but not the array as a whole. The index does not store the position of the element in the array.

</details>

## Week 11
<details><summary>What makes a good query language? </summary>

- Declarative: What? (and not how) also enables data independence
- Functional: The language expressions act like mathematical functions that operate on an instance of a model.
- Set-based: Act on one or a set of instances and output a set of instances.

</details>
<details><summary>What are the properties of JSONiq sequences? </summary>

- The sequence is ordered and flat, without a hierarchy.

</details>
<details><summary>What is the difference between a sequence of (1) and the array [1] in JSONiq? </summary>

- The sequence is equal to 1, while the array encodes a level of hierarchy.

</details>
<details><summary>Can I add an integer and a string? </summary>

- No, JSONiq is typed, the only "semi-typed" object is the empty set.

</details>
<details><summary>What does FLOWR stand for? </summary>

- For
- Let
- Order by
- Where
- Return
- (Group by)

</details>
<details><summary>What is a difference when grouping between FLOWR and SQL? </summary>

- In SQL, after grouping, the rows are not available any more, only the groups, whereas with FLOWR, each row of the group still is available.

</details>
<details><summary>What are the three execution modes in RUMBLE? </summary>

- Dataframe-based execution, which uses the dataframe interface (fastest, most constrained)
- RDD-based execution, on the back of Sparks RDDs (medium, medium)
- local exucution, like a single-core Java implementation (slowest, most expressive)

</details>

## Week 12
<details><summary>What does a graph solve? </summary>

- Complex relations for traversals of relationships

</details>
<details><summary>What are the ingredients for a stored graph? </summary>

- n nodes
- m (directed) edges
- p properties which are attached to nodes or edges (object attributes, except for information which is stored in edges)
- l labels, which corresponds to the type of an edge or node, multiple labels are possible

</details>
<details><summary>What do edges represent in the graph model? </summary>

- Foreign keys

</details>
<details><summary>What is the architecture of Neo4j? </summary>

- Core servers (master)- read replicas (client)

</details>
<details><summary>What makes a read replica in Neo4j? </summary>

- Clients can read from it.
- Data replication

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
<details><summary>What is index-free adjacency? </summary>

- Edges are saved on each node and not in a global index.

</details>
<details><summary>How are relationships(edges) stored? </summary>

- Two double linked list of edges at each end, because edges are not ordered in the data model and we need to discover all of them.
- source-previous, source-next
- target-previous, target-next

</details>
<details><summary>Which guarantees does Neo4j provide? </summary>

- Atomicity
- Recoverability
- Availability
- Scalability (since graphs are by design local)

</details>
<details><summary>How is atomicity achieved in Neo4j? </summary>

- Data only gets written if is wholly done, either by memory flush or written in the write ahead log.

</details>
<details><summary>How is recoverability achieved in Neo4j? </summary>

- After a crash, read the write ahead log and chatter with peers.

</details>
<details><summary>How is availability achieved in Neo4j? </summary>

- Master-slave architecture with core nodes and read replicas.

</details>

## Week 13
<details><summary>What is slow interactive? </summary>

- Takes minutes, but still somewhat interactive.

</details>
<details><summary>Why does OLTP not normalise? </summary>

- It is read intensive and has little updates, so the run time save from not doing joins is bigger than the loss from more writes.

</details>
<details><summary>OLTP vs OLAP </summary>

![OLTP vs OLAP](../images/13_OLAP_VS_OLTP.PNG)

</details>
<details><summary>What are the properties of a data warehouse? </summary>

- object oriented (single subject)
- integrated (of other databases, like a CRM, ERP)
- time variant (explicit in the reporting from 5-10 years)
- non-volatile (no updates, maybe increment every week)

</details>
<details><summary>What does ETL stand for? </summary>

- Extract (from other databases)
- Transform (data cleaning)
- Load (sort, partition, indexing, integrity constraints)

</details>
<details><summary>What is *slicing*? </summary>

- Selecting/extract all the data with a given feature value.

</details>
<details><summary>What is *dicing*? </summary>

- Often 2 dicers, aggregating over the remaining values given a slice and the feature to aggregate on.

</details>
<details><summary>What is ROLAP? </summary>

- query cubes similar to a RDBMS
- transparent with relational parents

</details>
<details><summary>What is MOLAP? </summary>

- proprietary memory format, cube is less transparent

</details>
<details><summary>What is the difference between a raw data cube(ROLAP) and a formal data cube(MOLAP)? </summary>

- the formal data cube is already aggregated and therefore may not have any duplicates.

</details>
<details><summary>What are measures in a fact table? </summary>

- different values (dimensions) given the keys, e.g. cost,profit

</details>
<details><summary>What are satellite tables? </summary>

- Extra information on the central table.

</details>
<details><summary>What is the star schema? </summary>

- Each dimension in the main table has a satellite table.

</details>
<details><summary>What is the snowflake schema? </summary>

- Satellite tables can have satellite tables.

</details>
<details><summary>What is *group by cube*? </summary>

- Take the whole cube, the power set of the dimensions and group by each element of that powerset.

</details>
<details><summary>What is a cross tabulation? </summary>

- The output of group by cube, where each cell is given and aggregates on all dimensions and sets of dimensions.

</details>
<details><summary>What is the syntax to store cubes? </summary>

- XBRL

</details>
<details><summary>Why can we forget about the drawbacks of XBRL? </summary>

- It is not highly efficient, but the database, where the file is fed into is more efficient.

</details>	