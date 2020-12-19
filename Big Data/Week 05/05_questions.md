# Big Data Week 05 questions
## General
<details><summary>What are the disadvantages of RDBMS? </summary>

- They are hard to set up and have high maintenance cost if you scale them up(out).

</details>
<details><summary>What is wide column storage? </summary>

- It stores some columns together after another and not one whole row after another.

</details>
<details><summary>What are the basic operations of HBase? </summary>

- Get(*RowID*)
- Put(*Row-Values*)
- Scan(*Range*)
- Delete(*RowID*)

</details>
<details><summary>Why can there be a total order in HBase? </summary>

- HBase supports ACID with locks, as there is exactly one RegionServer per row, this is (easier) possible.

</details>
<details><summary>What are CRUD operations? </summary>

- **C**reate
- **R**ead
- **U**pdate (write)
- **D**elete 

</details>

## Terminology
<details><summary>What is a region in HBase? </summary>

- A list of rows determined by a range of their RowID.

</details>
<details><summary>What is stored on the same physical machine in HBase? </summary>

- A *store*, a column family of the same region.

</details>
<details><summary>What is the hierarchy of entities in HBase? </summary>

- Table &rightarrow; Region &rightarrow; Store &rightarrow; HFile &rightarrow; HBlock &rightarrow; KeyValue

![Architecture](../images/05_layers.PNG)

</details>
<details><summary>What are cells in HBase? </summary>

- Cells are timestamped (milliseconds passed since midnight, January 1, 1970 UTC) values of row x column, due to versioning, there may be many.

</details>
<details><summary>What is the default amount of cells for a value? </summary>

- Default is 3.

</details>

<details><summary>Why do regions exist in HBase? </summary>

- Regions are essentially contiguous ranges of rows stored together and are the partitions in HBase, each region has a region server.

</details>

## HFiles
<details><summary>Where are stores saved? </summary>

- They are saved in one or multiple HFiles in HDFS.

</details>
<details><summary>How is a HFile structured? </summary>

- It is a sorted key-value list.

</details>
<details><summary>What is in a key of a HFile? </summary>

- (RowID,columnID,version/timestamp)

![KeyValue](../images/05_keyvalue.PNG)

</details>
<details><summary>What is in a value of a HFile? </summary>

- One HFile consists of many 64kB big *HBlocks* of data to make it easier to search things.

</details>
<details><summary>Why is there no length for column qualifier? </summary>

- All others are defined or fixed.

</details>
<details><summary>What is the purpose of the key type in a HFile? </summary>

- It tells whether a row is (to be) deleted in lazy deletion.

</details>	
<details><summary>When is data physically deleted? </summary>

- Data gets deleted when the two leafs merge. If there was a delete operation in one of the nodes, in the merged one, the data is omitted and the previous leafs deleted.

</details>

## Implementation
<details><summary>What is meant by HBase replication? </summary>

- The replication of the whole deployment to another datacenter.

</details>
<details><summary>Why does HBase not do redundancy? </summary>

- It is built on top of HDFS, which already does redundancy.

</details>
<details><summary>What does the HMaster do? </summary>

- It is similar to the NameNode, it keeps track of the tables and the column families. Additionally it balances the ranges to the RegionServers.

</details>
<details><summary>Why is there a Write-Ahead Log (HLog)? </summary>

- MemStore flushes all of it's memory content into a HFile, this means the memory is sorted. Sorting takes time, as a quick measure to safe the operation it is added to the Write-Ahead Log.

</details>	
<details><summary>How is data stored on persistent storage? </summary>

- In Log-structured merge-trees, which double in size for every level, and every level holds at most one node.

![LSM-tree](../images/05_LSM_tree.PNG)

</details>
<details><summary>What guarantees does HBase give? </summary>

- Access to row data is atomic and includes any number of columns being read or written to. There is no further guarantee or transactional feature that spans multiple rows or across tables. 

</details>

## [Bloom filter](https://de.wikipedia.org/wiki/Bloomfilter)
<details><summary>Where are Bloom filters used? </summary>

- In the HFile index, to check whether a key is in the file.

</details>
<details><summary>What is the accuracy property of Bloom filters? </summary>

- Bloom filter give no false negatives, but can give false positives.

</details>