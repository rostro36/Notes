# [Neural Subgraph Matching](https://arxiv.org/abs/2007.03092)
## Introduction
In general the problem of subgraph matching is an NP-hard problem and with that does not scale well.

A way to circumvent that is by aggregating/pre-processing some nodes together and query those meta-components. The result then is not as precise as before, but much faster.
## Problem setup
We assume G_{target} consisting of vertices_{target}, edges_{target} and features_{target} to be large and we want to identify our smaller G_{query} in the target.

To be a matching sub-graph, the edge and vertex features have to match exactly.

*Matching query to dataset* &rightarrow; Predict if a sub-graph of *G_{target}* is isomorphic to *G_{query}*.

This problem can be decomposed to 

*Matching neighbourhoods* &rightarrow; Given a target neighbourhood G_u around node *u* and a query neighbourhood G_v around *v* make a binary prediction, whether these two neighbourhoods and anchor nodes match.
### Subgraph matching
The subgraph matching has to adhere to the following relations:
- *transitivity* &rightarrow; if G_1 is a subgraph of G_2 and G_2 is a subgraph of G_3, then G_1 is a subgraph of G_3
- *anti-symmetry* &rightarrow; if G_1 is a subgraph of G_2 and G_2 is a subgraph of G_1, then G_1 is isomorphic to G_2
- *intersection set* &rightarrow; the intersection of the set of subgraphs of two graphs contains all common subgraphs of those two graphs
- *non-trivial intersection* &rightarrow; the intersection of two graphs contains at least the trivial graph
### Node vs edge-induced matching
A **node-induced matching** fixes the nodes and only contains edges, which are also in G_{query}.

In contrast an **edge-induced matching** starts with edges, which have to be in G_{target} and contains all endpoints of these edges.

Edge induced matching is more general and used for NeuroMatch.

## NeuroMatch
The general idea is to decompose the two graphs into many overlapping subgraphs and these subgraphs then get embedded with a GNN and matched with each other.
### Embedding stage
Run G_{target} through a *k*-layer GNN, which embeds the *k*-hop neighbourhood around every anchor node.
### Query stage
Use a subgraph prediction function f(*z_{query}*,*z_u*) that solves the matching neighbourhoods problem.
In the end aggregate the results of the f's until you get a final result.

A good *k* is at around 10.
### Subgraph prediction function
The subgraph prediction function is designed such that it adheres to the easy to check subgraph-property:
![z_{query}\[i\]\leq z_u\[i\]\forall_{i=1}^D](https://latex.codecogs.com/gif.latex?z_%7Bquery%7D%5Bi%5D%5Cleq%20z_u%5Bi%5D%20%5Cforall_%7Bi%3D1%7D%5ED) iff G_q subgraph of G_u
Where *D* are the embedding dimensions.

This results in the max margin loss:
![L(z_q,z_u)=\sum_{(z_q,z_u)\in P} E(z_q,z_u)+\sum_{(z_q,z_u)\in N} max\{0,\alpha-E(z_q,z_u)\}](https://latex.codecogs.com/gif.latex?L%28z_q%2Cz_u%29%3D%5Csum_%7B%28z_q%2Cz_u%29%5Cin%20P%7D%20E%28z_q%2Cz_u%29&plus;%5Csum_%7B%28z_q%2Cz_u%29%5Cin%20N%7D%20max%5C%7B0%2C%5Calpha-E%28z_q%2Cz_u%29%5C%7D),

with E(z_q,z_u)=||max\{0,z_q-z_u}||^2
with *P* being the positive examples, where z_q is a subgraph of z_u and *N* is a set of negative examples.

&alpha; stands for a margin that non-matching subgraphs should at least have.

There is a violation if for any dimension we have z_q\[i]>z_u\[i] and E(z_q,z_u) says how much.

The used subgraph prediction function f(z_q,z_u), then uses the E from before with a boundary, if E(z_q,z_u) is bigger than that boundary, then they are distinct, if E is smaller than that boundary then it is a subgraph.

### Training NeuroMatch
There are five steps:
1. generate G_{target} randomly
1. a node *u* is elected in G_{target}, from there a random BFS that does not take an edge with some probability is run and the result of this BFS then is G_{u}
1. we take *q* to be *u*, run the random BFS again, but this time on G_{u} instead of G_{target}
1. generate negative examples *w*
	- either by taking random anchors *u* and *q* in G_{target} and perform random traversal
	- disturb G_q in some form, to make it not a subgraph any more
1. train NeuroMatch
	1. embed *q*,*u*,*w*
	1. compute the loss
	1. backprop
	
There is a **curriculum training scheme**, that first starts with easy graphs and a small batch size and then once that plateaus, the graphs get harder\(bigger) and the batch size gets bigger.
### Runtime complexity
In the embedding stage the complexity is O(*K*(|E_{target}|+|E_{query}|)), where *K* is the amount of GNN layers and size of neighbourhood. The query stage then takes O(|V_{target}|&times;|V_{query}|), which is massively better than checking all possible subgraphs. This means it grows linearly in the size of the graphs, compared to exact methods, which grow exponentially.