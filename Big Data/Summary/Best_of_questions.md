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

It is easier to parallelize over different nodes/hard drives, but the search time to find them is larger, which impacts latency.

</details>
<details><summary>What is the advantage of having big blocks? </summary>

We hardly have to search this block, but we have to send the whole block over the network, even if we only need a small part, we also have a higher chance if the block fails,
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
<details><summary>- Of those legal characters, which can be at the start? </summary>

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
<details><summary>What is an XML declaration there for and how does a sample look like? </summary>

- The XML declaration sets grounds for reading the XML file to follow.
- <?xml version="1.0" encoding="UTF-8" standalone="no" ?>

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