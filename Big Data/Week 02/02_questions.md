# Big Data Week 02 questions
## Theory questions
<details><summary>Why are abstractions useful? </summary>
	
- They limit the complexity at a node. With less complexity resources can be saved by relying on others to do their job right and more importantly reduces coding/maintenance time.
- This abstraction works bottom-up as well as top-down; A computer language does not know which bytes it manipulates and the CPU core does not know which computer language you use.
		
</details>

<details><summary>Why distributed systems instead of centralized? </summary>
	
- Highly available.
- Easier to scale out, because we do not have a strong master that needs many resources.
		
</details>
	
<details><summary>Who should resolve conflicts? </summary>
	
- The client should resolve conflicts, because he knows what to do with them, as different applications need different solving strategies.	
		
</details>
	
<details><summary>CAP theorem </summary>
		
- No **C**onsistency: Just return your store/garbage.
- No **A**vailabilty: It takes forever.
- No **P**artition-tolerance: There is no partitioning.	
	
	
</details>
	
<details><summary>What is incremental scalability?</summary>
	
- It is the scalability behaviour of adding more machines; you should be able to add/remove one(or more) machines at a time without much penalty.
		
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
	
## Implementation questions
<details><summary>What are stateful services? </summary>
	
- Stateful services have to fetch something in memory that can change, stateless services do not need any data-memory; only code-memory.		
		
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
<details><summary>What are the two layers of replication of Azure? What problems do they solve? </summary>
	
- **Partition level**: Replication across stamps (in different datacenters) e.g. a power outage. Does not understand extents.
- **Stream level**: Replication inside of the stamp to deal with failed hard-drives/bit flip.		
		
</details>	
<details><summary>Why are extents sealed (append only)? </summary>
	
- Much easier to give consistency and deliver snapshots at the price of the size of the data.		
		
</details>
<details><summary>Why is DynamoDB not CAP? </summary>
	
- There is no consistency if a partition occurred.
		
</details>
<details><summary>Why is Azure not CAP? </summary>
	
- Ensures partition-freedom and availability, but no consistency. 
- Don't talk to the smaller partition if it is bad/unreachable and then fix the smaller partition, when it is up-to-date, that works in the real world, but not in the CAP-Theorem.		
		
</details>
	
<details><summary>Delete operation in append only? </summary>
	
- I am not sure, sorry		
		
</details>