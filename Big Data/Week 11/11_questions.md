# Big Data Week 11 questions
## General
<details><summary>What is the benefit of data independence? </summary>

- The logical language (SQL) works on many different physical implementations (Postgres, Spark, Hive).

</details>
<details><summary>What is a limitation of data independence? </summary>

- It needs data homogeneity or Spark enforces data homogeneity in a non-optimal way (convert an array to a string etc.).

</details>
<details><summary>Why was JSONiq invented? </summary>

- It can query heterogeneous, denormalized (even more than once, what still is okay for Spark) data nicely compared to SQL.

</details>
<details><summary>What makes a good query language? </summary>

- Declarative: What? (and not how) also enables data independence
- Functional: The language expressions act like mathematical functions that operate on an instance of a model.
- Set-based: Act on one or a set of instances and output a set of instances.

</details>
<details><summary>What is the language ecosystem for trees? </summary>

- There is none, but it is again XML vs JSON and different languages for different operations:
	- Navigation: XPath vs JSONPath
	- Transform. XSLT vs JSONT
	- XQuery: XQuery vs JSONiq (and more)
	- Update scripts: XQuery Update Facility vs JSONiq

</details>
	
## JSONiq basics
<details><summary>What is the JSONiq data model? </summary>

- Ordered sequences of items

</details>
<details><summary>What are the properties of JSONiq sequences? </summary>

- The sequence is ordered and flat, without a hierarchy.

</details>
<details><summary>What are the properties of the items that a sequence holds? </summary>

- Items can be denormalised and heterogeneous.

</details>	
<details><summary>What are the items of JSOniq? </summary>

- atomic
- array (enable hierarchy)
- objects (enable hierarchy)
- functions

</details>
<details><summary>What are the literals/atomics of JSON? </summary>

- Strings
- Numbers
- Booleans
- null

</details>
<details><summary>What do we do for other datatypes e.g. dates? </summary>

- There are implemented functions that take strings as input and return some other datatype.

</details>
	
## JSONiq syntax

<details><summary>How does JSONiq open a json file? </summary>

- json-doc("myfile.json")

</details>
<details><summary>What is the navigation for JSONiq? </summary>

- "." for objects
- "[q]" for arrays (if q is none, then it is the same as explode and q=[number] for an index)

</details>
<details><summary>How does JSONiq open a json file? </summary>

- json-doc("myfile.json")

</details>
<details><summary>How can filtering be done in JSONiq? </summary>

- [$$.code="CH"] responds with the indices where the code is "CH"

</details>
<details><summary>What does $ mean in JSONiq? </summary>

- $ is the context item, that indicates local variables
- $$ means each object, that would match the condition

</details>
<details><summary>How can you escape in JSONiq? </summary>

- With \

</details>
<details><summary>How can you build a sequence? </summary>

- With "," e.g. *(1,2,3,4,5,6,7,8,9,10)* 
- With "to" e.g. *1 to 10*

</details>

## JSONiq evaluation

<details><summary>What are some basic JSONiq operations? </summary>

- integer arithmetic
	- Empty sequence, where "()+1"=()
	- typed, not allowed for nested objects
- String operations:
	- concat
	- string-join
	- take sub strings
	- length of string
- Evaluation (eq, le)
	- General comparisons (=,< etc.)
		- if one item of the sequence matches, the whole sequence returns one true, this holds on both sides
- Logics (and, or, not) including non boolean like "if"

</details>
<details><summary>What is the difference between a sequence and an array? </summary>

- A sequence, which is on top, can scale into the millions, an array, that sits lower, does not scale as well.

</details>
<details><summary>What is the difference between a sequence of (1) and the array [1] in JSONiq? </summary>

- The sequence is equal to 1, while the array encodes a level of hierarchy.

</details>	
<details><summary>What is the default behaviour of operations with the empty sequence? </summary>

- an operation on an empty sequence returns the empty sequence

</details>
<details><summary>When using "if", what is the default behaviour of non boolean values? </summary>

- "0" and empty means *False*, complex datatypes give errors, else *True*.

</details>
<details><summary>Can I add an integer and a string? </summary>

- No, JSONiq is typed, the only "semi-typed" object is the empty set.

</details>
<details><summary>How is JSONiq evaluated? </summary>

- JSONiq is a functional language and as such, there is no evaluation flow, as this is done in a level below the language, but the default is lazy.

</details>	
<details><summary>What is meant by composability? </summary>

- An expression (e.g. 1+2) can be part of any other expression (e.g. "from 1 to 1+2".

</details> 
	
## FLOWR

<details><summary>What does FLOWR stand for? </summary>

- For
- Let
- Order by
- Where
- Return
- (Group by)

</details>
<details><summary>What is the translation of FLOWR to SQL? </summary>

- SELECT.. FROM.. WHERE ..

</details>
<details><summary>What is the structure of a FLOWR expression? </summary>

- Start with *let* or *for*.
- Use *order*, *group*, *where*.
- End with *return*.

</details>
<details><summary>What is a difference when grouping between FLOWR and SQL? </summary>

- In SQL, after grouping, the rows are not available any more, only the groups, whereas with FLOWR, each row of the group still is available.

</details>
<details><summary>Does FLOWR check types? </summary>

- Checking for type is optional, but can be done with *is instance of*, *castable*, *as*

</details>
<details><summary>How does one make an outer join with FLOWR expressions? </summary>

- *allowing empty* enables outer joins

</details>

## JSONiq implementation
<details><summary>What is the flow from a JSONiq query to it's execution? </summary>

- query -parsing &rightarrow; abstract syntax tree -translation &rightarrow; expression Tree -optimization &rightarrow; (another) expression tree -code generation &rightarrow; iterator tree -iterate&materialize &rightarrow; execution

</details>	
<details><summary>What is the materialized execution? </summary>

- The iteration of the iterator (execution) tree and demands that the data is always taken with the instruction to memory (this often can be streamed, if the expression allows it).

</details>
<details><summary>What are the three execution modes in RUMBLE? </summary>

- Dataframe-based execution, which uses the dataframe interface (fastest, most constrained)
- RDD-based execution, on the back of Sparks RDDs (medium, medium)
- local exucution, like a single-core Java implementation (slowest, most expressive)

</details>