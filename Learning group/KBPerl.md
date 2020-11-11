# [KBPerl](http://www.vldb.org/pvldb/vol13/p1035-lin.pdf)
## General
Knowledge is usually stored in the form of (*subject*, *predicate*, *object*), which can help question answering.

Most knowledge bases (KB) have limited coverage and usually are hard to keep up-to-date, as they often have to be rebuilt from the ground up.

The paper tries to solve this by augmenting an incomplete KB.
There are two ways to solve this problem, the first one is searching for predifined relations (e.g. is born), the second one (open information extraction) does not have this limitation, but is harder to achieve, also because of two major problems:

- the canonicalization problem, which deals with mapping different spellings to the same entity (e.g. *Pirmin Schwegler* and *P. Schwegler*)
- the linking problem, which deals with linking new entities to in the base graph existing entities, this is mostly done with additional information gathered from the document, outside of the triplet

KBPerl solves these problems by extracting *canonicalized facts* and *side information* i.e. named entities and meta information.

1. KBPearl constructs a semantic graph
1. The system disambiguates the noun and relation phrases in the facts by determining a dense subgraph
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
1. Filter out triplets which only consist of pronouns, sotpwords and interrogative words.
1. ~ exchange "he" with the name.
1. Employ a pattern dictionary to conduct basic canonicalization like in [QKBfly](http://www.vldb.org/pvldb/vol11/p66-nguyen.pdf)

## Semantic graph construction
Nodes in the semantic graph can be onw of noun phrases (which can be typed), entities (typed noun phrases in the target graph), relation phrases(verb of the triplet) or predicates(verbs and synonyms in the target graph).

Edges represent a similarity measure between two nodes, the edges can be one of:
- Noun phrase- entity edge: which tells how likely the noun phrases is correctly mapped to that entity. This measure is based on the target KB's links of the entity with the same content as the noun to the other entity. (Good for disambiguation)
- Relation phrase- predicate edge: which tells how likely the relation is correctly mapped to that predicate. This can come from pattern repositories such as [PATTY](https://www.aclweb.org/anthology/D12-1104.pdf).
# Entity- entity edge
- Entity- entity edge: 