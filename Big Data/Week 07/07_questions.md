# Big Data Week 07
## General
- What is the native archtitecture inside of sequence files?
	- Keyvalue pairs
- What is the difference between HBlock and a HDFS block?
	- They are different levels, HBlocks are on the file level, HDFS blocks are on the (quasi-)physicial level.
	- HBlock is extended, so every keyvalue is in the same HBlock. A HDFS block has a fixed size and keyvalues will be cut if it has to be done.
- What are "normal" dimensions of MapReduce?
	- Several TBs of data
	- Thousands of nodes, which can read in parallel.
- What are the four meanings of Reducer?
	- Reduce function
	- Reduce task (when a reduce function gets applied to data)
	- Reduce slot(when a machine does a reduce job)
	- Machine that has a reduce slot.
## Input/Output
- What is the input for MapReduce?
	- Keyvalues (*Splits*) of one or many Hfiles or from databases, which are stored in different blocks on different machines.
	- Often, one split is one HBlock.
- What is the output of MapReduce?
	- Parallel files of Keyvalues in the same directory/database.
- How is a text file changed to a keyvalue pair?
	- The key is the offset (character) of each Nth new line and the strings of those lines is the value.
## MapReduce phases	
- What happens in the Map phase?
	- Filter the necessary fields from the input Keyvalues into a new, intermediate Keyvalue relationship. (E.g. From lines of text to words and their counts)
- Why is there a Combine phase?
	- Combine is an optional phase, that can be added to make sure less data has to be transmitted.
- What happens in the Combine phase?
	- The mapper can already combine(reduce) the results for the first time, before sending each keyvalue pair to the reducers in the shuffle phase. 
- What are the properties the Combine function must have?
	- The function key-value input/output are be the same as in the reduce phase and the function is associative and commutative.
- What happens int he Shuffle phase?
	- The intermediate Keyvalues get sorted and then partitioned nicely for the next phase. (E.g. all counts of the same word go to the same reducer)
	
- What happens in the Reduce phase?
	- The function that we want to use will be applied to the sorted intermediate values. (E.g. add up all counts of the same word.)

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