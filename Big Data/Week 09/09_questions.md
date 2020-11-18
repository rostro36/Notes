# Big Data Week 09
## General
- What is the different between *top*, *take* and *takeSample*?
	- *top* takes the last samples, *take* takes the first samples and *takeSample* takes randomly distributed samples.
- What is the unit of work for parallel execution in Spark?
	- Each partition (by default exactly one HDFS block) creates a task to run in parallel.
- Who executes tasks?
	- Containers/slots (cores and memory of a node/machine)
## Planning
- How can data locality be used by a framework?
	- The same node that computed the intermediate results also progresses with downstream tasks, like that, the data does not have to be transferred around.
- How is a *narrow dependency* defined?
	- A sequence of *transformations* that does not depend on other partitions.
- How is a *wide dependency* defined?
	- It is a shuffle, the next partition is dependent on multiple intermediate partitions.
- How can a *wide dependency* be avoided?
	- For each job, one might partition the data beforehand, such that one job is only related to one partition.
- When does a *stage* end?
	- At the input, output or a *wide dependency*.
- Can one task of stage 2 run, when another of stage 1 is still running?
	- No.
- How can intermediate be re-used?
	- Using the *persist* keyword in Spark one can persist some RDDs in memory.
## DataFrames
- What are DataFrames?
	- DataFrames are fixed length, stored columnarly, "0NF-tables", created from a SQL query and then transformed into RDDs. 
- What are the benefits of DataFrames?
	- Uses a lot less memory, can be created with SQL.
- When the choice is open, should we use RDD transformations or DataFrames transformations?
	- DataFrame transformations are preferred, because Spark can optimize them better, as they are not a black-box, but SQL, which is often also easier to write.
## Data heterogeneity
- What is the difference in creation of a schema between a RDBMS and Spark?
	- RDBMS have to define a schema and import data for the table, Spark reads directly from the data.
- What is the functionality of *explode(array)* in SparkSQL?
	- For each entry in the array, a new row with only that entry is added instead of the array. This adds a prefix to *product*: *fi.product*, yet the non-array field have no prefix. 
- How does SparkSQL deal with objects/dicts?
	- One can address objects/dicts with ".".
- How does SparkSQL deal with data heterogeneity?
	- If there is no single type that SparkSQL can infer, it resorts to string and leaves the handling of the string representations to the user.
## Bottlenecks
- What are the (usual) sources of bottlenecks?
	- Memory, CPU, disk I/O, network I/O
- What is the bottleneck MapReduce solves?
	- Disk I/O, with more disks.
## Performance and measures
- What is latency?
	- The time until the first result.
- What is throughput?
	- The amount of operations/mega bytes per second.
- What is response time?
	- It is the time until the results; the last operation= latency+transfer\**operations*
- How is speed-up defined?
	- old latency/new latency
- What is the formula of Amdahl's law?
	- Speed-up=1/(1-p+p/s), where *p* is the parallelisable part and *s* is the amount of parallel workers.
- What is Gustafson's law?
	- Speed-up=1-p+sp
- When is Amdahl's law used and when Gustafson's law?
	- Amdahl's law holds for constant problem size.
	- Gustafson's law holds for constant computing power.