# Big Data Week 09 questions
## General
<details><summary>What is the different between *top*, *take* and *takeSample*? </summary>

- *top* takes the last samples, *take* takes the first samples and *takeSample* takes randomly distributed samples.

</details>
<details><summary>What is the unit of work for parallel execution in Spark? </summary>

- Each partition (by default exactly one HDFS block) creates a task to run in parallel.

</details>
<details><summary>Who executes tasks? </summary>

- Containers/slots (cores and memory of a node/machine)

</details>
	
## Planning

<details><summary>How can data locality be used by a framework? </summary>

- The same node that computed the intermediate results also progresses with downstream tasks, like that, the data does not have to be transferred around.

</details>
<details><summary>How is a *narrow dependency* defined? </summary>

- A sequence of *transformations* that does not depend on other partitions.

</details>
<details><summary>How is a *wide dependency* defined? </summary>

- It is a shuffle, the next partition is dependent on multiple intermediate partitions.

</details>
<details><summary>How can a *wide dependency* be avoided? </summary>

- For each job, one might partition the data beforehand, such that one job is only related to one partition.

</details>
<details><summary>When does a *stage* end? </summary>

- At the input, output or a *wide dependency*.

</details>
<details><summary>Can one task of stage 2 run, when another of stage 1 is still running? </summary>

- No.

</details>
<details><summary>How can intermediate results be re-used? </summary>

- Using the *persist* keyword in Spark one can persist some RDDs in memory.

</details>

<details><summary>What are the steps inside of Catalyst? </summary>

1. Type the columns and give each column a clearly identifiable name. (**analysis**)
1. Restructure that AST with predefined logical rules on abstract syntax trees(AST).(**logical optimization**) Those rules are rather simple pattern matching rules, that get executed on trees, where input sub-trees get repeatedly substituted for better sub-trees until there is no change left. After those changes the AST can be tested again.
1. The logical plan from the previous phase then gets mapped to one or more physical plans, where depending on the size of the underlying DataFrames, the plan with the cheapest joins get chosen (cost-based optimization).(**physical planning**)
1. The final phase is compiling the selected physical plan to Java bytecode(**code generation**). To do that, type-checked quasiquotes are used, which get parsed by the Scala compiler (similar to *eval*).


</details>
<details><summary>What is the benefit of Catalyst being extensible? </summary>

- New data types can easily be added.
- The default optimizations can also be changed rather easily.

</details>

## DataFrames

<details><summary>What are DataFrames? </summary>

- DataFrames are, typed fixed length, stored columnarly, "0NF-tables", created from a SQL query and then transformed into RDDs. 

</details>
<details><summary>What are the benefits of DataFrames? </summary>

- Uses a lot less memory, can be created with SQL.

</details>
<details><summary>When the choice is open, should we use RDD transformations or DataFrames transformations? </summary>

- DataFrame transformations are preferred, because Spark can optimize them better, as they are not a black-box, but SQL, which is often also easier to write.

</details>

## Data heterogeneity
<details><summary>What is the difference in creation of a schema between a RDBMS and Spark? </summary>

- RDBMS have to define a schema and import data for the table, Spark reads directly from the data.

</details>
<details><summary>What is the functionality of *explode(array)* in SparkSQL? </summary>

- For each entry in the array, a new row with only that entry is added instead of the array. This adds a prefix to *product*: *fi.product*, yet the non-array field have no prefix.

</details>	
<details><summary>How does SparkSQL deal with objects/dicts? </summary>

- One can address objects/dicts with ".".

</details> 
<details><summary>How does SparkSQL deal with data heterogeneity? </summary>

- If there is no single type that SparkSQL can infer, it resorts to string and leaves the handling of the string representations to the user.

</details>
	
## Bottlenecks

<details><summary>What are the (usual) sources of bottlenecks? </summary>

- Memory, CPU, disk I/O, network I/O

</details>
<details><summary>What is the bottleneck MapReduce solves? </summary>

- Disk I/O, with more disks.

</details>

## Performance and measures
<details><summary>What is latency? </summary>

- The time until the first result.

</details>
<details><summary>What is throughput? </summary>

- The amount of operations/mega bytes per second.

</details>
<details><summary>What is response time? </summary>

- It is the time until the full results come in; the last operation= latency+transfer\**operations*

</details>
<details><summary>How is speed-up defined? </summary>

- old latency/new latency

</details>
<details><summary>What is the formula of Amdahl's law? </summary>

- Speed-up=1/(1-p+p/s), where *p* is the parallelisable part and *s* is the amount of parallel workers.

</details>
<details><summary>What is Gustafson's law? </summary>

- Speed-up=1-p+sp

</details>
<details><summary>When is Amdahl's law used and when Gustafson's law? </summary>

- Amdahl's law holds for constant problem size.
- Gustafson's law holds for constant computing power.

</details>
<details><summary>Why is scalability not the be-all end-all metric? </summary>

- Any system can scale arbitrarily well with a sufficient lack of care in its implementation.

</details>