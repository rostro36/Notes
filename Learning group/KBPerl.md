# [KBPerl](http://www.vldb.org/pvldb/vol13/p1035-lin.pdf)
## General
Most knowledge bases (KB) have limited coverage and usually are hard to keep up-to-date, as they often have to be rebuilt from the ground up.

The paper tries to solve this by augmenting an incomplete KB.

There are two ways to solve this problem, the first one is searching for predefined relations (e.g. *is born*), the second one (open information extraction) does not have this limitation, but is harder to achieve, also because of two major problems:
- the **canonicalization problem**, which deals with mapping different spellings to the same entity (e.g. *Pirmin Schwegler* and *P. Schwegler*)
- the **linking problem**, which deals with linking new entities to in the base graph existing entities, this is mostly done with additional information gathered from the document, outside of the triplet

The overall cycle looks as follows:
1. KBPearl constructs a semantic graph
1. The system disambiguates the noun and relation phrases in the facts by determining a dense subgraph with the help of *canonicalized facts* and *side information* i.e. named entities and meta information.
1. The new subgraph gets inserted in the existing graph

## Problem definition
A KB is a set of of fact triples (*subject*, *predicate*, *object*).

Open triples are the new tentative facts extracted from the OIE system.

Side information includes all the named entity mentions and their types and the timestamp of the document.

## System framework
The pipeline for each document to get side information looks as follows:
1. sentence tokenization
1. POS-tagging
1. noun-phrase chunking and NER, with entity typing
1. Time Tagger to extract time information from the document.

For triplet canonicalization three major tasks are made.
1. Filter out triplets which only consist of pronouns, stop words and interrogative words.
1. ~ exchange "he" with the name.
1. Employ a pattern dictionary to conduct basic canonicalization like in [QKBfly](http://www.vldb.org/pvldb/vol11/p66-nguyen.pdf)

## Semantic graph construction
Nodes in the semantic graph can be one of:
- noun phrases (which can be typed)
- entities (typed noun phrases in the target graph)
- relation phrases (verb of the triplet)
- predicates(verbs and synonyms in the target graph).

Edges represent a relatedness measure/probability of being correctly mapped between two nodes, the edges can be one of:
- Noun phrase- entity edge: This measure is based on the target KB's links from the nound with the same content to the entity. (Good for disambiguation)
- Relation phrase- predicate edge: This can come from pattern repositories such as [PATTY](https://www.aclweb.org/anthology/D12-1104.pdf).
- Entity- entity edge: These edges only exist between entity nodes connected to different noun phrase nodes to make sure there is no problem with same name of different entities. Relatedness is measured by the amount of shared predicates and objects weighted with IDF. p(entity_i, entity_j)=(sum over all shared objects weight(predicate)\* weight(object))/(sum over all shared objects weight(predicate)
- Predicate- predicate edge: exactly the same as entity- entity edge, but with predicates. The weighting stays exactly the same.
- Entity- predicate edge: IDF weight of the predicate, when there exists a tuple with the entity and the predicate in them, both in the new tuples and the existing KB, if these conditions ar not fulfilled it is 0 instead of the IDF.

## Graph densification & linking
Map each noun (of the new tuple) exactly one entity (in the KB) and each predicate (of the new tuple) exactly one relation (in the KB). A good linking results in a dense graph.

The dense subgraph problem is NP-hard and tries to find a subgraph that:
- is connected
- contains all nouns and predicates
- is a semantic graph as described above
- maximizes the minimal weighted degree for densification. (The minima is used instead of the average, as the average can be lifted by heavy nodes.)

The greedy O(|V|) algorithm starts with the whole graph and tries to throw out the node with the smallest degree and finishes, when the lowest vertex can not be thrown out without breaking the rules, but it needs a post-processing step that creates new entities if the degree at an old entity (except for noun phrase- entity edges) is below a threshold, then the connected noun phrases will be new entities.

To deal with bigger documents the greedy algorithm can be iteratively\* run on smaller subsets of the document (O(|subsets|\*|V|) and gives slightly worse results) or only append edges where there exist a sentence where two vertices occur in the same sentence (O(|nouns|^2)). Both variants perform much better and faster than the normal algorithm on large documents.

## Experiments
There are some already existing baselines and for some other datasets manual annotation was done. Many tools are only optimized on small texts with less than 30 words.

KBPerl shines on longer documents, especially if they have a lot of declarative sentences, as questions lower the precision. High recall is prioritized over precision. Relation linking is good compared to competitors.

MinIE is the best extractor because it provides more compact extractions, which leads to high recall, but the overall performance is not too determined by the Open IE tool, because post-processing was done on them in KBPerl.