# [Hierarchical Graph Representation Learning with Differentiable Pooling](https://arxiv.org/abs/1806.08804)
## Introduction
Current GNN architectures are inherently flat, which means they only consider the given graph also as an computational graph and hardly change the structure of it. Often a graph embedding is computed using an aggregation over all the nodes of the graph, where the final aggregation does not consider the structure of the graph.

DiffPool (soft-)clusters the nodes and uses those clusters as input for the next layer.
## Pooling
The pooling is done with an assignment matrix *S*, that has a pre-defined amount of clusters, e.g. 10\% of the nodes of the previous layer.

The assignment matrix softly assigns a node to a cluster. The entry S\_\{ij\} is the probability of node *i* belonging to cluster *j*.

The features *X* of the cluster are computed as and the adjacency matrix *A* as:

X\^\{l+1\}=S\^l Z\^l, where *Z* are the embeddings of the nodes.

A\^\{l+1\}=S\^l A\^l S\^l
## Learning the assignment matrix and the node embedding
The node embedding *Z* gets computed as usual with a GNN:

Z\^l=GNN\_\{l,embed\}\(A\^l,X\^l\)

The assignment matrix gets computed similarly, but with different weights and a softmax layer at the end:

S\^l=softmax\(GNN\_\{l,pool\}\(A\^l,X\^l\)\)
## Training
To stabilise training, extra losses are also put in place; the link prediction *LP* to make sure that clusters should get pooled together and the entropy loss *LE*, that penalises spreading weights too evenly, so a node has an incentive to join exactly one cluster.

LP=\|\|A\^l,S\^l S\^\{l T\} \|\|, where this is under the Frobenius norm.

LE=1\/n &Lambda;\_i Entropy\(S\_i\), where S\_i is the i-th row of *S*
## Architecture
The DiffPool layer can be put in almost any GNN technique, the model of the experiments is based on GraphSAGE consists of two DiffPool layers after three layers of GraphSAGE each. The clusters decrease to 10\% of the previous layer each time.
## Results
The running time is not that much lower, since the graph gets much smaller each time.

The results are generally a bit better than the vanilla approaches, showing that clustering can lead to better results.

Generally, not all allocated cluster have to be populated, so when in doubt probably use too many clusters, this also alleviates the \(theoretical\) problem of non-connected nodes wrongfully being clustered together, as their representation may be too similar.

In many cases, the clusters also help with interpretability as many clusters "make sense".