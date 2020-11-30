# Big Data Week 10
## MongoDB: The Definitive Guide - Chapter 3
MongoDB leaves padding factor which is the amount of extra space it gives the documents to grow, default is 1 and gets bigger if the document gets over the factor and diminishes if the document does not move again.

### Insert
```javascript
db.foo.insert({"bar" : "baz"})
```
Will store a document with the *"bar"* field equal to *"baz"* and the implicit *_id* key to the collection.

This action can be batched to make it faster as long it is only one collection and at most 48MB. If the batch insert fails, the first part gets uploaded, the rest will not.

MongoDB only does minimal checks on the data being inserted;
- it adds an *"_id"* field if one does not already exist.
- it checks for size, as all documents must be smaller than 16 MB, to make sure only sensible data schemes get included.
- it also checks if there is no key with a "."

### Delete
```javascript
db.foo.remove({"opt-out":true})
```
Will remove all documents in the *foo* collection, where *opt-out* is *true*, but keep the collection and it's metadata alive.

It is faster and removes all metadata if we drop the collection with:
```javascript
db.foo.drop()
```
### Update whole documents
Updates are row-atomic and executed one after another.

An update always has two arguments, a query document first, that selects which document should be replaced and the updated value.
```javascript
db.users.update({"field_to_check" : correct_value}, new_document);
```
In this case we also copy it's *_id*, which gives an error if we replace another document (that matches the criteria), as the *_id* will be twice in the database. This is a reason, why filtering and selecting by *_id* is better, also it is faster.

### Update fields
Instead of replacing the whole document, we can also only modify a single/multiple field once with ($ indicates an action on fields, without the dollar it is an operation on whole documents)(we can not modify a field twice in the same query):
```javascript
db.analytics.update({"url" : "www.example.com"},{"$inc" : {"pageviews" : 1}})
```
Here we have used *$inc* (if the field was empty before, we would have created it with the value to increment (1 in this case)), to change the value in general we can use *$set*, which creates a new field if it does not exist yet, to remove one can use *$unset*.

Upsert updates a document if the criteria matches, if it never matches it inserts a document that fits the criteria and sets the value according to the command.
```javascript
db.analytics.update({"url" : "/blog"}, {"$setOnInsert" : {"createdAt" : new Date()}},true)
```
Set the *createdAt* if the URL does not exist.

Updates, by default, update only the first document found that matches the criteria.
```javascript
db.users.update({"birthday" : "10/13/1978"}, {"$set" : {"gift" : "Happy Birthday!"}}, false, true)
```
The *true* makes sure that all matching documents get updated.

```javascript
db.runCommand({"findAndModify" : "processes", "query" : {"status" : "READY"}, "sort" : {"priority" : -1}, "update" : {"$set" : {"status" : "RUNNING"}},true)
```
Instead of *"update"* *"remove"* would also be possible to remove the document. The returned document is the found document, not the modified. The last *true* is also for upsert.

### Update arrays
```javascript
db.stock.ticker.update({"_id" : "GOOG"}, {"$push" : {"hourly" : {"$each" : [562.776, 562.790, 559.123], "$slice" : -10, "$sort" : {"rating" : -1}}}}}
```
*$push* appends/creates an array. Adds the whole array to "hourly". *$slice* makes sure the list only consists of the last 10 entries. *$sort* orders the entries first, before taking the top 10. You have to call *"$each"* for the following functionalities.

*$addToSet* and *$ne* only append if the value is not already in the array.

One can remove elements from either end with *$pop* or by criteria with *$pull*.

Arrays are 0-indexed. And *$* can be used as a wildcard which corresponds to all positions in an array.

### Write Concerns
By default actions (and the applications that execute them) wait until they get a response from the database.
- **Acknowledged** writes wait for a (positive/negative) response from the database.
- **Unacknowledged** writes do not give a response, so you don't know if they went through. (Malformed code, duplicate keys also go unnoticed)


## MongoDB: The Definitive Guide - Chapter 4
### Querying in general
```javascript
db.users.find({"username" : "joe", "age" : 27},{"username" : 1, "email" : 1})
```
Will return all usernames and emails with *joe* and age 27. If no criteria is given, all usernames and emails get returned.

*_id* gets returned by default, except you actively exclude it with *"_id":0*.

### Operators
Operators for conditions are:
- "$lt", "$lte", "$gt", "$gte"
- "$ne"
- "$in", "$nin"
- "$mod"
- "$exists"
- Perl Compatible Regular Expressions as normal values.

And meta-operators:
- "$not"
- "$and“
- "$or“ (slow merge of two sub-queries)
- "$nor“.

They can also be combined:
```javascript
db.raffle.find({"$or" : [{"ticket_no" : {"$in" : [725, 542, 390]}}, {"$not" : {"age":{"$gte":18,"$lte":30}}}})
```
### Null
*null* matches itself and *does not exist*. 
```javascript
db.c.find({"z" : {"$in" : [null], "$exists" : true}})
```
Checks if z is exactly *null* and just does not exist.

### Querying for arrays
By default, if one element of an array matches, this condition counts as fulfilled for the whole object. Use "$elemMatch" to say that all conditions should apply on the same element, but this does not work on non-arrays.

- "$all" makes sure all objects match.
- "$size" checks for length, but can not be combined with e.g. "$gt".
- "$slice" : [23, 10]}, skips the first 23 elements and returns up to the next 10.

If you search for an array, it will only be true, if the array is exactly the same.

You can query for specific elements with *.-notation* (*$* acts as a wildcard and matches all indexes in an array), e.g.
```javascript
db.food.find({"fruit.2" : "peach"})
```
- min({"x" : 10}).max({"x" : 20} exist and can be used for post-processing of a found array

### Querying on Embedded Documents
```javascript
db.people.find({"name" : {"first" : "Joe", "last" : "Schmoe"}})
db.people.find({"name.first" : "Joe", "name.last" : "Schmoe"})
```
Checks if the name is exactly that following dict, whereas  the second one checks the keys, other keys might also appear and it ignores the order.

### $where queries
"$where" allows to use arbitrary JavaScript in the clause and are more powerful, but also much slower, as all BSON have to be converted to a JavaScript object and indexes can not be used.

Server-side scripting is a way for security problems. To prevent this, use a *scope*.

### Cursors
*Find* results are usually returned in a cursor, which allows to limit the number of results, skip or sort the results.

- *cursor.hasNext()* checks that the next result exists
- *cursor.next()* fetches the result
- *forEach()* can be used for a loop

Cursor act like pipelines and are only executed for the next batch once the first result of the batch gets used.
```javascript
db.stock.find({"desc" : "mp3"}).limit(50).skip(50).sort({"price" : -1})
```
(1 is ascending, -1 is descending) 

Default sorting behaviour for limit and skip:
1. Minimum value
1. null
1. Numbers (integers, longs, doubles)
1. Strings
1. Object/document
1. Array
1. Binary data
1. Object ID
1. Boolean
1. Date
1. Timestamp
1. Regular expression
1. Maximum value

Large skips are slow, as all the results in the middle have to be computed.

### How are cursors implemented
*plain* queries are only the query, *wrapped* queries have to be used if another clause is used e.g.
- *"$orderby"*, tells to order
- *"$maxscan"*, specify the max number of documents that should be scanned.
- *"$min" / "$max"*, expects an existing document, whose index acts as lower or upper limit.
- *"$showDiskLoc"*, says where on disk it is located.

When running a long job of finding, changing and updating it might happen that the updated document gets found again. To combat that one can use the slower option to .shapshot() the query, so every "_id" only gets found once.

Cursors should be freed, as they take resources on the server. The cursor dies automatically after 10 minutes of inactivity (which can be disabled) or it gave all the results.
### Database commands
Database commands are metacommands that handle the database for e.g.
- drop tables.
- shut down the server
- clone databases
- count documents
- do aggregations
- ```javascript
db.runCommand({getLastError : 1})
```

These commands act like normal commands, but on the *"cmd"* collection and the most important returned fields are "*ok*" (where 1 is good) and *"errmsg"*, which gets omitted if the command was successful. They are field order-sensitive.


## MongoDB: The Definitive Guide - Chapter 5
### Index general
A search without indexing is called a *table scan*. We can combat this with a limit, where it stops after that limit is reached. Or with indices.

Indices read much faster thanks to their pointers, but writing and updating takes longer, as the entry has to be put in every index.

```javascript
> db.foo.ensureIndex({"a" : 1, "b" : 1, "c" : 1, ..., "z" : 1}, {"name" : "alphabet"})
```
Creates an index and gives it the name "alphabet".

Compound indexes exist, but are only useful if the query matches a prefix of the index.

Sorting direction does not matter for simple queries, but compound ones can profit from the better sorting direction.

*Right-balanced indices* only keep the very right parts(for a dates, the most recent) of the tree in memory, e.g. *"_id"*, which is indexed by default.

### Indices in practice
*"$where", "$exists", "$nin"* can not use indices. *"$not"* often is too complicated to modify the clause it tries to negate.

MongoDB automatically first applies fields, where there is an index, if the order allows it.

Range queries can only use the index if the prefix matches the other query criteria and the suffix does not limit another query criteria.

Usually, MongoDB can only use one index per query, except for each *"$or"* clause.

We can index every entry of one (not two+) array, but not the array as a whole. The index does not store the position of the element in the array.

The *index cardinality* says how many different keys are in the index, the higher the cardinality, the more useful the index is.
### explain()
To get information about a query one can add .explain() after the query and get a document with these fields:
- "cursor" tells you if and which index has been used (*"BasicCursor"* means no index has been used, *"BtreeCursor"* means a index has been used.
- "isMultiKey" tells if multiple keys point to the same object, they are a bit worse than normal indexes.
- "n" is the amount of returned documents.
- "nscannedObjects" tells how many objects were searched.
- "nscanned" tells how many index entries were searched.
- "scanAndOrder" tells whether the results were already ordered or had to be ordered in memory for this query.
- "indexOnly" is true if the index can *cover* a query if all the information needed/queried is already in the index.
- "nYields" tells how many times the query temporarily yielded the lock for writes.
- "millis" reports the speed of the query, including the preprocessing of the query.
- "indexBounds" describes which part of the index was used, starting and ending row.
### hint() and default index behaviour
*"hint()*" forces a query to use a certain index, even if it makes the query slower, because it has to chase pointers instead of reading the sequential table.

By default, MongoDB can see if a index covers the whole query and use that. If there is no such index, the query will be run on many likely indexes and selects the one that first gives 100 results and halts the other searches. This index will then be cached for this type of query, the cache will be updated after some time, 1'000 queries or the data has changed a lot.

Indexes work best for large collections of large documents with selective queries.

Creating the index usually blocks all other operations, unless specified otherwise.
### Types of indices
Most indices have a unique key (and become *unique indices*) and ignore replacements of that key, this also holds for the key *"null"* of not having the key field.
If you want to create a new unique index it will not work if there are duplicates in the database, one then can change the values of the objects or *"dropDups"*.

*Sparse indices* act like normal indices, but just ignore objects where the key field does not exist and also not return them if they don't have that key field.