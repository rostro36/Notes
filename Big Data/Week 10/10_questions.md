# Big Data Week 10 questions
## General
<details><summary>How can you avoid network I/O scale-up? </summary>

- Batch process
- Push down computation
	- pre-filter
	- pre-project
	- pre-aggregate

</details>
<details><summary>What is the rule of thumb for executor/core mix?  </summary>

- The amount of executors should be around the root of the cores.

</details>
<details><summary>How many task does every executor carry out? </summary>

- Every executor makes many tasks, depending how fast they are/ how close they are to the data.

</details>
<details><summary>Why should one make more than one job? </summary>

- To balance the jobs better.

</details>
<details><summary>Why should one not make a job for every record? </summary>

- Too much I/O over the network overhead and also update files.

</details>
<details><summary>What is UTF-8? </summary>

- It is an encoding of the Unicode character catalogue.

</details>
<details><summary>What are the most basic functions of a storage solution? </summary>

- **C**reate
- **R**ead
- **U**pdate
- **D**elete

</details>	

## Stragglers
<details><summary>What is tail latency? </summary>

- Stragglers/ that take significantly more time than other jobs.

</details>
<details><summary>What is the reason for stragglers? </summary>

- Queues
- Power limits(hyperthreading)
- Garbage collection
- Energy management

</details>
<details><summary>Why is it important to deal with stragglers, even though they are only a small percentage? </summary>

- The latency contributes everywhere;
	- If the map-phase is not over, the reduce-phase can not start
	- If all the other nodes are done with the reduce-phase, and one still takes time, then you are still waiting.

</details>
<details><summary>What can be done against stragglers? </summary>

- Execute every job twice, but about costs twice as much.
- Start with the second/back-up job, once the first one takes too long.

</details>

## RDBMs vs. Document stores
<details><summary>What are the 3 basic integrities of RDBMs?  </summary>

- Atomic integrity (all entries are atomic values, not dicts or the like)
- Tabular/Relational integrity (all relations between tables are valid)
- Domain integrity (all entries in the same column have the same type)

</details>
<details><summary>What is the atomicity of MongoDB? </summary>

- Atomicity on one document, like the row-atomicity of HBase.

</details>
<details><summary>What is in-situ processing for these kind of data systems? </summary>

- In-situ reads and writes files directly as opposed to RDBMs and similar systems, that use the base data to populate a database, where it then gets read and written to.

</details>
<details><summary>What can go wrong from XML to SQL? </summary>

- "Nestedness", not atomic values
- Type heterogeneity

</details>
<details><summary>What is the problem with heterogeneity? </summary>

- We have to store a lot of NULLs, for all rows, where the field does not exist.

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

## Defaults in MongoDB
<details><summary>What is the default document format in MongoDB? </summary>

- BSON, "JSON in binary", with a bit more data types.

</details>
<details><summary>Which language does MongoDB use? </summary>

- JavaScript

</details>
<details><summary>What is the default MongoDB behaviour if the query is empty? </summary>

- Take all.

</details>
<details><summary>What is the default MongoDB behaviour if a field does not exist in the data? </summary>

- Count it as false/not take it.

</details>
<details><summary>What makes a "0" in a projection? </summary>

- It excludes a field, all the rest is taken.

</details>
<details><summary>How to deal with nestedness in MongoDB: </summary>

- Dicts: use "."
- Arrays: use *$in*

</details>
<details><summary>How does MongoDB deal with mismatching types? </summary>

- It will be *False*, but not an error.

</details>
<details><summary>Which "database queries" exist in MongoDB? </summary>

- count
- sort
- skip
- limit
- distinct
- aggregate

</details>
<details><summary>What is special about *aggregate*? </summary>

- It makes a pipeline, which can be executed in parallel.

</details>
<details><summary>What is a *replica set*? </summary>

- The redundant blocks for the same shard.

</details>
<details><summary>How is a shard accessed? </summary>

- Each shard has a primary node with one replica, which gets accessed first.

</details>
<details><summary>What are *write concerns* in MongoDB? </summary>

- The difference between a synchronous (wait for ACK of all secondary nodes) and asynchronous (don't wait) action.

</details>
<details><summary>Does MongoDB store one document after another? </summary>

- No, MongoDB leaves padding factor which is the amount of extra space it gives the documents to grow, default is 1 and gets bigger if the document gets over the factor and diminishes if the document does not move again.

</details>
<details><summary>What symbolizes the $ in a MongoDB query? </summary>

- $ indicates an action on fields, without the dollar it is an operation on whole documents.

</details>
<details><summary>In MongoDB, if an object has an array, where the condition only holds on certain elements, will the condition hold for the whole object? </summary>

- By default, if one element of an array matches, this condition counts as fulfilled for the whole object.

</details>

## Indices
<details><summary>Why are indices needed? </summary>

- To find elements faster compared to a full scan.

</details>
<details><summary>How do indices work? </summary>

- Looking up the index in a table points to all the matching elements.

</details>
<details><summary>What is the best data structure for indices? </summary>

- A B+-tree, where the inner nodes are criteria and the leafs are the elements.

</details>
<details><summary>In a B+-tree, how many children will a node with n keys have? </summary>

- n+1 children

</details>
<details><summary>Is there a default index? </summary>

- Yes, by default *"_id"* is indexed, but many other indices can be created.

</details>
<details><summary>Why does it not make sense to have a index on *year* and one on *year* , *month*? </summary>

- *year* , *month* already contains an index on *year*, as it is a prefix.

</details>
<details><summary>Can we index the entries of an array in MongoDB? </summary>

- We can index every entry of one (not two+) array, but not the array as a whole. The index does not store the position of the element in the array.

</details>