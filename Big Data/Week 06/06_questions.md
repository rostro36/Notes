# Big Data Week 06 questions
## General

<details><summary>What is the objective of the lecture? </summary>

- Going from physical representation (XML/JSON) to a logical representation (data model).

</details>
<details><summary>What do XML/JSON encode? </summary>

- They encode trees.

</details>
<details><summary>What is the difference between XML and JSON? </summary>

- XML knows it's name

</details>
<details><summary>What is the difference between well-formed vs valid documents? </summary>

- Valid documents must adhere some schema and the language, well-formed documents must only be well-formed in the language. Every valid document must be well-formed.

</details>
<details><summary>Is a document without a schema valid? </summary>

- By definition a valid document must have a schema, if it does not have a schema it is neither valid nor invalid.

</details>
<details><summary>JSON vs. XML </summary>

![JSON vs XML](../images/06_JSON_vs_XML.PNG)

</details>	
<details><summary>What are the 4 XML (most important) information items? </summary>

- Document &rightarrow; children, version
- Element &rightarrow; local name, children, attributes, parent
- Attribute &rightarrow; local name, normalized value, owner element
- Character(Text) &rightarrow; characters, owner element

</details>

## Schema bases
<details><summary>What are the 4 type system fundamentals? </summary>

- Distinction between atomic types and structured types
- More or less the same categories of atomic types
- Lists and maps(dict) as structured types
- Sequence type cardinalities

</details>
<details><summary>In database normal forms, when is the distinction between atomic types and structured types used? </summary>

- 1.NF only uses atomic types, non-normal allows structured types.

</details>
<details><summary>What are the atomic data types? </summary>

- String
- Numbers (often arbitrary precision in logical)
- Boolean
- Dates and Times
- Time Intervals (Since months are not well defined)
- Binaries
- Null

</details>
<details><summary>What are the 4 cardinality types? </summary>

- One (required)
- \* &rightarrow; zero or more (repeated)
- ? &rightarrow; zero or one (optional)
- \+ &rightarrow; one or more

</details>
<details><summary>In JSON schema, what is the difference between "allOf", "anyOf" and "oneOf"? </summary>

- "allOf": Must be valid against all of the subschemas (can not be used to extend existing schemas)
- "anyOf": Must be valid against any of the subschemas
- "oneOf": Must be valid against **exactly one** of the subschemas

</details>

## Examples
<details><summary> What is the high level structure of a dataset?</summary>

- A dataset is a list of maps.

</details>
<details><summary>What is an example of a homogeneous dataset? </summary>

- A database/CSV.

</details>

## Different data-formats
<details><summary>What is the difference between homogeneous and heterogeneous datasets? </summary>

- Heterogeneous may have missing values. 

</details>
<details><summary>What are the categories of formats? </summary>

- Text vs. binary
- Nested vs. flat
- Schema optional vs. required

</details>

## Default behaviour
<details><summary>What should we do if nothing about attributes is said? </summary>

- If nothing is written about attributes, they are allowed.

</details>
	
<details><summary>What is the default behaviour if a specified schema element does not exist in the document? </summary>

- If the element does not exist, the document is not valid.

</details>

## Dremel
<details><summary>What is the purpose of Dremel? </summary>

- Dremel is a scalable, interactive ad hoc query system for analysis of read-only nested data.

</details>
<details><summary>In Dremel, what does *r* and *d* stand for? </summary>

- *r* is for repetition, which tells how many hops are shared between the current and previous path.
- *d* is for definition, which tells how big the whole path is.
- **Required fields are not counted.** 

</details>
<details><summary>In Dremel, what does is mean if *d* is smaller than the max depth? </summary>

- A *d* smaller than the max repetition level denotes a *NULL*.

</details>

<details><summary>Can you give a Dremel example? </summary>

![Dremel example](../images/06_dremel_example.PNG)

</details>
<details><summary>What is a serving tree? </summary>

- A serving tree is an architecture to control computing. The root acts as entry point for the user and gives work down to the children. Each parent can do some aggregation and load balancing.

</details>