# Big Data Week 11 readings
## [Rumble Data Independence for Large Messy Data Sets](https://arxiv.org/pdf/1910.11582.pdf)
### General
Rumble is a query execution engine for large, heterogeneous nested collections of JSON (and similar) objects built on top of Spark.

Rumble uses JSONiq under the hood and tries to translate those queries into Spark primitives.

Data independence for heterogeneous, nested JSON data sets is doable and enables to use latest physical improvements.

### Downfall of classical RDBMs
Classical RDBMs work great on structured, normalized data, but have problems with heterogeneous, nested data:
- a field that gets only used by one object creates a row for all objects
- need for typing, this means that types get very general even though the actual query (due to filtering) only sees one specific type at one field, it still is considered general 
- no support for complicated data structures, they are stored as strings
- non-existing and the explicit *null* become the same *NULL* in SQL
### Background
Objects can be either:
- atomic JSON types as well as binaries and dates
- structured items
- function items
Sequences are always flat, unnested automatically and maybe empty.

If there is no match to the queried structure (e.g. the object has no field, where a filter should be applied) then there is nothing returned from that object, but the query can be applied to other objects.

FLOWR stands for:
- For
- Let
- Order by
- Where
- Return

If-else, switch, try cath also exist

instance-of castable, cast and typeswith also exist to deal with types.

Rumble parses the query to an AST, this then to a physical execution plan, where the execution plan consists of runtime iterators, which each more or less correspond to one JSONiq expression or FLOWR clause.

Runtime iterators can dynamically switch between:
- Dataframe-based execution, which uses the dataframe interface
- RDD-based execution, on the back of Sparks RDDs
- local exucution, like a single-core Java implementation

Typically the outer queries are RDDs or Dataframes, as they are faster and usually can be applied for detemined statically  RDDs, if we do not know the structure.

The execution modes can be written in the bottom ones, but hardly the other way round. This also means that higher levels get used as often as possible, but often have to be translated to local execution, from where the rest either has to be local execution as well or (hardly ever worth it) translated back.

Expressions can
- transform sequences
	- lift a RDD to a DataFrame with *annotate*
- produce sequences
	- reading from files (much like in Spark)
	- *parallelize* which acts the same as in Spark and makes RDDs
- combine sequences
	- concat which is RDD.union
	- if .. then .. else .. where the if-clause gets eagerly evaluated
### FLOWR
Roughly correspond to SQL's *SELECT*, where first there has to be a *for* or a *let* and one *return* that marks the end.

A tuple is a binding of items to variable names and each FLOWR-clause passes a stream of these tuples to the next.

**Return** converts its tuple stream to a sequence of items.

**FOR**
Iteration through a sequence, where each item is bound to a new variable.

The first *for* clause can be converted to a DataFrame.

Two *for* clauses give produce the cartesian product, as the first DataFrame has to be exploded in a Spark-UDF.

**LET**
Bind one variable to a sequence.

Also a UDF that executes the iterator tree.

**WHERE**
Use a UDF.

**GROUP-BY**
Concatenate the sequences of the group, no aggregation is needed. The groups don't need to have the same type, for this the group key is cast and the type annotated in a separate column. Under the hood use the Spark SQL function *order by* and aggregate by *first*.

**ORDER-BY**
Explore the column for types and throw an error if some keys are not comparable, afterwards use the casting from *GROUP-BY* and perform normal Spark SQL.

**COUNT**
Can run in DataFrames and on the back of *MONOTONICALLY_INCREASING_ID()*

**RETURN**
*Count* can end a FLOWR expression, but under the hood *return* gets called which, with the help of a UDF convert the DataFrame to a RDD and then unnest with flatMap().

### Experiments

VXQuery needs the files to be copied.
AsterixDB, CloudDB and SparkSQL make it very hard to load the data to start. Spark takes a long time for the metatables.

The performance of Rumble orients itself at that from SparkSQL, which is the best in the comparison, but adds a layer on top of it (for the polymorphic operators and data representations).

Rumble can handle large datasets better than any other tested and also have a rather small overhead that make the heterogenity simple. JSONiq systems are a lot faster on small input sizes and simple queries, but Rumble catches up with growing input size, as those systems are rather wasteful with their memory.