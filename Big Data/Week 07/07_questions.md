# Big Data Week 07 questions
## General
<details><summary>What is the native architecture inside of sequence files? </summary>

- Key-value pairs	

</details>
<details><summary>What is the difference between HBlock and a HDFS block? </summary>

- They are different levels, HBlocks are on the file level, HDFS blocks are on the (quasi-)physical level.
- HBlock is extended, so every key-value is in the same HBlock. A HDFS block has a fixed size and key-values will be cut if it has to be done. 	

</details>
<details><summary>What are "normal" dimensions of MapReduce? </summary>

- Several TBs of data
- Thousands of nodes, which can read in parallel.	

</details>
<details><summary>What are the four meanings of Reducer? </summary>

- Reduce function
- Reduce task (when a reduce function gets applied to data)
- Reduce slot(when a machine does a reduce job)
- Machine that has a reduce slot. 	

</details>	
<details><summary>How long does MapReduce take on a 1000-node cluster? </summary>

- A few hours, maybe even a day. 	

</details>

## Input/Output
<details><summary>What is the input for MapReduce? </summary>

- Key-values (*Splits*) of one or many HFiles or from databases, which are stored in different blocks on different machines.
- Often, one split is one HBlock.

</details>
<details><summary>What is the output of MapReduce? </summary>

- Parallel files of Key-values in the same directory/database.	

</details>
<details><summary>How is a text file changed to a key-value pair? </summary>

- The key is the offset (character) of each Nth new line and the strings of those lines is the value.

</details>
<details><summary>How is parallelization achieved in MapReduce? </summary>

- Each HDFS block can be computed for himself in the mapping phase.

</details>

## MapReduce phases
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
	- As in the worst case n immediate results have to be generated and sent to n reducers.
- The final results only depend on one of the second immediate results (Reduce). O(1) 	

</details>	
<details><summary>Can we start the next phase after the first results of the previous phase are available? </summary>

- Hardly, as the next phase depends on the previous phase.

</details>	


## Memory and transmission
<details><summary>What are the two roles in MapReduce (version 1)? </summary>

- JobTracker (master)
- TaskTracker (servant) 	

</details>
<details><summary>What does the master do in the MapReduce framework? </summary>

- Keep track of the status of each of the *M* map and *R* reduce tasks (idle, in-progress, completed)
- The ID of the worker for the non-idle tasks.
- Store the location and size of the intermediate values (partially) completed by the map tasks.
- Transmit those locations to in-progress reduce tasks.
- Ping each worker to see if he has not failed.
- Balance jobs and re-assign failed(including jobs which have completed, but the data  was not already fetched by the following consumer) jobs.

</details>	
<details><summary>Where do intermediary results get saved before they are sent? </summary>

- They are kept in memory for as long as possible, if the memory is full, they will get spilled to memory in a L-Tree as a HFile in HBase.	

</details>
<details><summary>How are intermediary results get transmitted? </summary>

- Mappers open an http-server, which then get queried by reducers. 	

</details>	
<details><summary>Why is this transmission method chosen? </summary>

- Like this reducers can control their load and are not swamped by incoming data. 	

</details>	