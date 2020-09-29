# Big Data Week 03

## Questions
- Importance of Throughput, Capacity and Latency?
	- Depends on the application, but Capacity can be much higher compared to Throughput, which can be bigger than Latency.
![Scaling behaviour](../images/03_scaling.PNG)
- What is the difference between object storage and file file storage?
	- **Object storage:**
		- Billions of TB files
		- bad latency, better throughput
		- allows random access
		- often only key-value
		- offered as services by Amazon (or other) use with other people
	- **File storage:**
		- Millions of PB files
		- better latency, worse throughput
		- only allows scanning
		- file system exists
		- create&use cluster yourself
- What is the advantage of having small blocks?
	- It is easier to parallelize over different nodes/hard drives, but the search time to find them is larger, which impacts latency.
- What is the advantage of having big blocks?
	- We hardly have to search this block, but we have to send the whole block over the network, even if we only need a small part, we also have a higher chance if the block fails,
	also we use more storage if we can not fill the whole block with data.
- HDFS architecture:
![HDFS architecture](../images/03_architecture.PNG)
- Why can HDFS be centralized?
	- Because it can, as there are a lot fewer customers (only one company).
- What should a NameNode do?
	- File namespace +Access control (how the file system looks like)
	- File to block mapping
	- Block to location (node)
- What does the DataNode do?
	- It stores blocks of data.
- How does HDFS relate to the local computer on the DataNode?
	- HDFS blocks are stored as files on the physical computer (DataNode).
- How does a client get data from HDFS?
	- The client asks the NameNode about a file.
		- The NameNode responds with the BlockIDs to fetch.
	- The client then asks the DataNode for the data.
- What does the NameNode and DataNode talk with each other?
	- DataNode always initiates connection to namenode and sends heartbeats
		- NameNode answers with block operations
	- every 6 hours there is a block report, to check if all blocks are stored/no block got corrupted
