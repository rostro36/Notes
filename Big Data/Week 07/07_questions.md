# Big Data Week 07
## General
- What is the native archtitecture inside of sequence files?
	- Keyvalue pairs
- What are "normal" dimensions of MapReduce?
	- Several TBs of data
	- Thousands of nodes, which can read in parallel.
## Input/Output
- What is the input for MapReduce?
	- Keyvalues (*Splits*) of one or many Hfiles or from databases, which are stored in different blocks on different machines. 
- What is the output of MapReduce?
	- Parallel files of Keyvalues in the same directory/database.
- How is a text file changed to a keyvalue pair?
	- The key is the offset (character) of each Nth new line and the strings of those lines is the value.
## MapReduce phases	
- What happens in the Map phase?
	- Filter the necessary fields from the input Keyvalues into a new, intermediate Keyvalue relationship.

- What happens int he Shuffle phase?
	- The intermediate Keyvalues get sorted and then partitioned nicely for the next phase.
	
- What happens in the Reduce phase?
	- The function that we want to use will be applied to the sorted intermediate values.

- What are the big O complexities of MapReduce?
	- The first immeadiate results only depend on one shard (Map). O(1)
	- The second immeadiate results depend on many other first immeadiate results (Shuffle). O(n^2)
	- The final results only depend on one of the second immeadiate results (Reduce). O(1)
## Memory and transmission
- What are the two roles in MapReduce (version 1)?
	- JobTracker (master)
	- TaskTracker (servant)
- Where do intermediary results get saved before they are sent?
	- They are kept in memory for as long as possible, if the memory is full, they will get spilled to memory in a L-Tree as a HFile in HBase.
- How are intermediary results get transmitted?
	- Mappers open an http-server, which then get queried by reducers.
- Why is this transmission method chosen?
	- Like this reducers can control their load and are not swamped by incoming data.
	

