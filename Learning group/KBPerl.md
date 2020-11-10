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
