# Big Data Week 08 questions
## General
<details><summary>What is a slot? </summary>

- A slot are some computing resources (memory+processor) that can execute jobs.	

</details>
<details><summary>What is the drawback of too few slots? </summary>

- Too few slots (*under-subscribed*) can be clogged by resource-light jobs, that do not need the whole share of resource.

</details>
<details><summary>What is the drawback of too many slots? </summary>

- Too many slots (*oversubscribed*) that makes the jobs to be pre-empted, because the resource is needed by another job. 

</details>

## MapReduce vs. YARN
<details><summary>What are the problems of MapReduce? </summary>

- MapReduce only scales to thousands, not dozens of thousands.
- JobTracker is a single point of failure.
- JobTracker has to do two things: schedule and monitoring
- The amount of memory is static.
- Some resources are idle, because another phase gets executed.	

</details>
<details><summary>What does YARN make different? </summary>

- It adds another layer of master(*ResourceManager*)-client(*NodeManager*). A *NodeManager* may open a MapReduce JobTracker *container* and asks for more *containers*/slots.	

</details>
<details><summary>What makes YARN better? </summary>

- Scales to tens of thousands.
- It does not keep resources from previous phases or reserve resources way too early.
- The *ResourceManager* tracks, not the JobTracker
- YARN is fairer, also for SLA-guarantees.
- It can more than MapReduce.
- YARN has an idea of locality.

</details>
<details><summary>What makes Spark different compared to MapReduce? </summary>

- There is no static pipeline of Map-Shuffle-Reduce, but the much more open framework of Spark's DAG.	

</details>
<details><summary>How can Spark be seen as a DAG? </summary>

- The DAG shows the dataflow, the sources are the DataNodes and the sinks are the clients.	

</details>
<details><summary>What is the workflow of a Spark job? </summary>

- The user builds the pipeline of transformations, but requesting the action then triggers the execution of that pipeline.	

</details>

## YARN architecture
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
<details><summary>What does a *ResourceRequest* contain? </summary>

- number of containers
- resources per container
- locality preferences
- priority of requests within the app	

</details>
<details><summary>What is the job of the NodeManager? </summary>

- The NM is the head of the computation node. And reports the state of his node and the job it runs, also does some security check before starting a job. 	

</details>

## Scheduling
<details><summary>What are properties that a good allocation strategy must fulfil? </summary>

- Sharing incentive/ balancing
	- each user should benefit from sharing the cluster compared to only use the set of his allocated slot mix
- Strategy-proof/ honesty
	- users should not benefit from lying
- Envy-free/fairness
	- a user should not prefer the allocation of another user
- Pareto efficient/ full power
	- The allocation of one user can not be increased without decreasing another's share.

</details>
<details><summary>What are some nice to have properties of an allocation strategy? </summary>

- Single resource fairness
	- For a single resource, the solution should resolve to *max-min* fairness.
- Bottleneck fairness
	- If there is one bottleneck resource, the whole strategy should reduce to *max-min* fairness on that resource.
- Population monotonicity
	- When a user leaves, no remaining user should suffer from that.
- Resource monotonicity
	- If more resources are added, a user should not suffer from that.

</details>
<details><summary>How can slots be scheduled? </summary>

- FIFO
- Capacity scheduling, where there are sub-groups that are weighted differently(in proportion to their capacity) and are merged to a bigger queue.
- Fair Scheduling, like capacity scheduling, but there is neither user nor weights.	

</details>
<details><summary>What is queue elasticity? </summary>

- If one user-group does not use it's whole capacity, it can be shared with another user-group.	

</details>
<details><summary>How does the (weighted) fair scheduler work? </summary>

- Steady Fair Share: the capacity/weights each user-group has reserved/bought
- Current Share: Current usage
- Delta: Current usage-Steady Fair share

- The fair scheduler gives the slot to the user with the biggest delta. 

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
<details><summary>How are the slots allocated? </summary>

- Each container has the size of the % of dominant resources needed by the user.
- The sum of the size of the containers are proportional to the weights.	

</details>
<details><summary>What are the assumptions made by the dominant resource fairness on the hardware? </summary>

- It assumes that the machines are balanced, the memory has to be proportional to the CPU for each and every machine inside the cluster. The amount of CPUs may change, but the proportion has to stay.	

</details>
<details><summary>Why is it possible to execute more than 100% of the cluster allocated with dominant resource fairness? </summary>

- Dominant resource fairness first applies the 100% resource tokens and afterwards it sees that not everything is used, because each token might have a different dominant resource. So even more tokens can be packed.


</details>
<details><summary>How does dominant resource fairness allocate slots? </summary>

- Computing one small block and repeating it for the whole cluster. The last one has to be broken up and accounted for with the *Delta*.	

</details>

## RDDs
<details><summary>What is a RDD? </summary>

- It is a Resilient Distributed Dataset, the "data format" of Spark, a big, partitioned collection.
	
</details>
<details><summary>What is the lifecycle of an RDD? </summary>

- **Creation:** Taken from the filesystem (local, S3, HDFS..)
- **Transformation:** Transform RDD to another RDD (e.g. MapReduce on a RDD)
- **Action:** Save the final output (not an RDD), this triggers all the computations before, because the previous computation were lazy.

</details>
<details><summary>Why is it less of a problem if a RDD gets lost? </summary>

- RDDs can easily be recomputed.
	
</details>
<details><summary>What are some of the one-to-one transformations? </summary>

- Filter: Depending on the condition, forward the RDD or throw it away.
- Map: Apply a function to an RDD and output another RDD (one-to-one).
- FlatMap: Like Map, but flatten the output, as one function may have multiple outputs (one-to-many).
- Distinct: Check for equality and only output the distinct ones (Deduplication).
- Sample: Seed and fraction from the whole RDD.	

</details>
<details><summary>What are some many-to-one transformations? </summary>

- Union: Merge two RDDs
- Intersection (Join): Take the intersection of two RDDs
- Subtract: Only keep values which are not in another RDD	

</details>
<details><summary>What is a many-to-many transformation? </summary>

- Cartesian product	

</details>
<details><summary>What are some actions? </summary>

- Collect: Take all the output of the previous RDDs (will be a list, not an RDD)
- Count: Amount of output, not the value in there.
- Count by value: Counts all the duplicates, output will be a dict of distinct values and their count.
- Take: Only collect the first few.
- Top: Only collect the top values.
- TakeSample: Take a **random** sample of outputs. 
- Reduce: Apply a reduce function to the previous RDD.

</details>
<details><summary>What is the difference between narrow and wide dependencies in Spark? </summary>

- *Narrow* dependencies map one parent partition to at most one child partition, where as *wide* dependencies map one parent partition map to many child partitions.
	
</details>
<details><summary>What are the implications of narrow and wide dependencies on the scheduling in Spark? </summary>

- Each parallel stage is ended either by root data, already computed data or a *wide* dependency. So many *narrow* dependencies can be computed in parallel.
	
</details>