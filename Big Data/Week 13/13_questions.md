# Big Data Week 13 Questions
## OLTP vs OLAP
<details><summary>What is slow interactive? </summary>

- Takes minutes, but still somewhat interactive.

</details>
<details><summary>What does OLTP stand for? </summary>

- online transaction processing

</details>
<details><summary>What does OLAP stand for? </summary>

- online analytical processing

</details>
<details><summary>What is the difference of access patterns between OLAP and OLTP? </summary>

- OLTP: lots of small writes, exact/ no aggregates
- OLAP: lots of reads over big amount of data, that takes hours(or even more), aggregates

</details>
<details><summary>What is the most natural data shape for OLAP and OLTP respectively? </summary>

- OLTP: tables
- OLAP: cubes

</details>
<details><summary>Why does OLTP not always normalise as much as possible? </summary>

- It is read intensive and has little updates, so the run time save from not doing joins is bigger than the loss from more writes.

</details>
<details><summary>OLTP vs OLAP </summary>

![OLTP vs OLAP](../images/13_OLAP_vs_OLTP.PNG)

</details>

## Data warehouse

<details><summary>What are the properties of a data warehouse? </summary>

- object oriented (single subject)
- integrated (of other databases, like a CRM, ERP)
- time variant (explicit in the reporting from 5-10 years)
- non-volatile (no updates, maybe increment every week)

</details>
<details><summary>What does ETL stand for? </summary>

- Extract (from other databases)
- Transform (data cleaning)
- Load (sort, partition, indexing, integrity constraints)

</details>
<details><summary>What are some data cube dimensions? </summary>

- where
- what
- what currencies
- when
- who

</details>
<details><summary>What is *slicing*? </summary>

- Selecting/extract all the data with a given feature value.

</details>
<details><summary>What is *dicing*? </summary>

- Often 2 dicers, aggregating over the remaining values given a slice and the feature to aggregate on.

</details>
<details><summary>What is the dimensionality of the data cube? </summary>

- The number of dimensions of the cube is the number of columns with keys.

</details>
	
## ROLAP vs MOLAP
<details><summary>What is ROLAP? </summary>

- query cubes similar to a RDBMS
- transparent with relational parents

</details>
<details><summary>What is MOLAP? </summary>

- proprietary memory format, cube is less transparent

</details>
<details><summary>What is the difference between a raw data cube(ROLAP) and a formal data cube(MOLAP)? </summary>

- the formal data cube is already aggregated and therefore may not have any duplicates.

</details>
<details><summary>What are measures in a fact table? </summary>

- Different values (dimensions) given the keys, e.g. cost,profit

</details>
<details><summary>What are satellite tables? </summary>

- Extra information tables that relate from the central table.

</details>
<details><summary>What is the star schema? </summary>

- Each dimension in the main table has at most one satellite table.

</details>
<details><summary>What is the snowflake schema? </summary>

- Satellite tables can have satellite tables.

</details>
	
## Drill down/roll up

<details><summary>What is a roll-up? </summary>

- Combining all values of a feature to a more general aggregate; removing dicers.

</details>
<details><summary>What is drilling down? </summary>

- Adding details/dicers to the results.

</details>
<details><summary>How can different drillings be used in the same fact sheet? </summary>

- Use *null*

</details>
<details><summary>How can different groupings/rollups be used? </summary>

- *group by grouping sets*/*group by roll-up*

</details>
<details><summary>What is *group by cube*? </summary>

- Take the whole cube, the power set of the dimensions and group by each element of that powerset.

</details>
<details><summary>What is a cross tabulation? </summary>

- The output of group by cube, where each cell is given and aggregates on all dimensions and sets of dimensions.

</details>
<details><summary>What is MDX and for which OLAP flavour is it used? </summary>

- Multidimensional dimensional expressions
- Language/Model for MOLAP queries

</details>
<details><summary>What are the dimensions of MDX? </summary>

- Measures/info
- Dimensions/columns

</details>
<details><summary>What are some benefits of MDX compared to fact sheets? </summary>

- MDX is aware of hierarchies, similarly to trees.
- MDX allows to dice easily with on columns/rows

</details>

## Storing cubes
<details><summary>What is the syntax to store cubes? </summary>

- XBRL

</details>
<details><summary>What are the files of XBRL? </summary>

- XML, trees

</details>
<details><summary>Why was this file type used for XBRL? </summary>

- XML is well established, can be used to check if the data is valid and it works in general

</details>
<details><summary>Why can we forget about the drawbacks of XBRL? </summary>

- It is not highly efficient, but the database, where the file is fed into is more efficient.

</details>	