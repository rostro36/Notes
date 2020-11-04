# Big Data Week 08
## [Dominant Resource Fairness](https://cs.stanford.edu/~matei/papers/2011/nsdi_drf.pdf)
### General
DRF can be implemented such that each scheduling decision takes time O(log(users)).

### Max-min fairness and slots
Before DRF, *max-min fairness* was used on a meta-source(*slot*) that was a fixed percentage of a machine.

Allocating them would leave lots of performance idle, that was allocated to get all the percentages out but not used, because the load was not balanced.

Also choosing the amount of slots can have big effects:
- too few slots (*undersubscribed*) can be clogged by resource-light jobs, that do not need the whole share of resource.
- too many slots (*oversubscribed*) that makes the jobs to be preempted, because the resource is needed by another job. 
### Allocation properties
Good sharing stragies should tick as many allocation properties as possible:
- Sharing incentive/ balancing
	- each user should benefit from sharing the cluster compared to only use the set of his allocated slot mix
- Strategy-proof/ honesty
	- users should not benefit from lying
- Envy-free/fairness
	- a user should not prefer the allocation of another user
- Pareto efficient/ full power
	- The allocation of one user can not be increased without decreasing another's share.
Nice to have:
- Single resource fairness
	- For a single resource, the solution should resolve to *max-min* fairness.
- Bottleneck fairness
	- If there is one bottleneck resource, the whole strategy should reduce to *max-min* fairness on that resource.
- Population monoticity
	- When a user leaves, no remaining user should suffer from that.
- Resource monoticity
	- If more resources are added, a user should not suffer from that.
No allocation policy that satisfies the sharing incentive and Pareto efficiency properties can satisfy resource monotonicity, because if two users need two resources, while they are more dependent on different resources, but still have to split them accordingly, they both gain something from sharing, as each user gets more than half from the bottleneck source. If only one source gets extended heavily and is not a bottleneck anymore, the other source gets split fairly, which is worse for the user that was bottlenecked by that resource before.
### Dominant Resource Fairness
DRF computes the share of each resource allocated to that user; the maximum among those resource share per user is called the *dominant share* and the resource that corresponds to that share is called *dominant resource*.

**DRF performs max-min on those dominant shares.**

![DRF algorithm](08_DRF_algo.PNG)
### Weighted DRF
The definition of a weighted dominant share(*s*) changes to max{u_ij/w_ij}, where *u_ij* is the user *i*s share of the resource *j* and *w_ij* the weight of user *i* on resource *j*.
### Asset Fairness
Each user takes the combined **sum** of all the sub-resources (instead of max), so *c%* for CPU and *m*% for memory and takes *c+m*% now.

This violates the sharing incentive property, if one user needs a lot more (non-crucial) resources it gets a lot less capacity then just take their normal share.
### Competitive Equilibrium from Equal Incomes(CEEI)
Each user initially gets the *Steady Fair Share* and then start trading with each other.

CEEI is not strategy proof. Because if a user is heavily reliant on the bottleneck resource, he can lie about needing more of the less needed resource (and not get less shares of the bottleneck)  and trade it for the bottlenecked resource with one less reliant user.

CEEI is also not population monotone, as with a user leaving, a trading partner with other needs leaves.

### Experiments
The sum of dominant shares may exceed 100%, because they might be on shared resources and they are dynamically given out, so fluxes happen. Since you can not cut a CPU, there might be a discrepancy of one max *needed* compared to something continuous.


DRF outperforms the other strategies in terms of response time, job completion and utilization, especially for long jobs.

## [Resilient Distributed Datasets](https://www.usenix.org/system/files/conference/nsdi12/nsdi12-final138.pdf)
### General
MapReduce only shares computation, not memory, which can help a lot in iterative tasks, as data can be reused. Also MapReduce does not save it's intermediary results somewhere (out of the box) which makes interactive data mining much harder.

Spark's RDDs tries to perform in-memory computations on large clusters in a fault-tolerant manner. Being in-memory means not having to load it to disk and also not converting to something that can go to disk. Further RDDs allow for a data placement and partitioning strategy.

Unlike other operations RDD only allows coarse-grained transformations, which apply to the whole dataset, not just certain entries.

Some programming models that RDDs express well:
- MapReduce
- Iterative MapReduce
- DryandLINQ
- SQL
- Pregel
- Bathced System Processing
### Resilient Distributed Datasets (RDDs)
RDDs are read-only, immutable, partitioned collections of records, created(*transformed*) from other RDDs or data in stable storage.

RDDs can be typed, but do not have to. RDDs themselves are a type.

RDDs are a set of deterministic functions(*actions*, bundled into a *lineage*), which are applied to base data.

Users can indicate the *persistence* (where) and the *partitioning* (in which partition) to enable faster computation.

### Recomputing RDDs
If one RDD partition fails (or is in the hand of a straggler), it can be recomputed by finding an *ancestor* of that partition (not the whole dataset) and run through the rest of the *lineage*. All of this while not dependant tasks can still work on ther nodes. 

This notion of recomputability also makes it easier to schedule for data locality and easier to debug, as a partition can be recomputed and captured swiftly.

### Storing RDDs
An RDD is represented by:
- a set of *partitions* (atomic pieces of the dataset)
- *dependencies*, which enable to recompute the RDD
- a *function* that describes how to compute the RDD from the *dependencies*
- metadata about *partitioning*
- metadata about *data placement*
All of these properties can be queried by the devs.

RDDs can be persisted with the *persist* keyword, this acts as checkpointing:
- in-memory storage as deserialized Java objects (fastest)
- in-memory as serialized data (memory efficient)
- on-disk storage (good if RDD only fits on disk, but are too useful to recompute)
### Spark
Spark is written in Scala on top of a JavaVM.

The Scala interpreter is changed only slightly, as classes are served over http and instead of referencing another line of the input (written by the user) reference the value generated by that line.

Devs write a *driver program* that connects to a cluster of *workers*. The driver then also tracks the RDDs lineage.

There is a LRU policy on RDDs, not partitions. If a partition can not be stored on RAM, it throws out a partition from less recently used RDD or go himself to disk, if the other partition is from the same RDD.
### Scheduling
*Narrow* dependencies map one parent partition to at most one child partition, where as *wide* dependencies map one parent partition map to many child partitions. *Narrow* dependencies are preferred, as they are much easier to recompute, easier to track and parents do influence less children on failure.

The scheduler first forms the query into *stages*, which are levels in the DAG. A stage is ended either by root data, already computed data or a *wide* dependecy. Each stage should contain as many *narrow* dependencies as possible. After this planning, the output RDD gets computed, if that is not possible, the parent has to be computed (also lazily).

The tasks get mapped to workers based on data locality (bring the task to the data). For *wide* dependencies, the intermediate records get materialized to simplify fault recovery. If tasks take too long or fail, they can be re-run.

### Performance
Spark can be 20-40x faster than Hadoop, which is fast enough to be interactive for a 1TB dataset. Most of the speed-up comes from avoiding I/O and data transformation inside the JavaVM.

The more the data gets reused, the higher is the speedup, without reuse, the speedup compared to Hadoop is only very small and comes from having a lighter setup and dealing less with HDFS and not having to do conversion from/to HDFS to use them in the JVM. Spark scales best with more machines compared to Hadoop and can deal with little memory.

Also node recovery is much faster.