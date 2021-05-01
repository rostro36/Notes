# [Heterogeneous Graph Transformer](https://dl.acm.org/doi/abs/10.1145/3366423.3380027)
## Introduction
Meta-Paths are a powerful instrument for getting the best out of heterogeneous graphs. To use them, they often need domain knowledge, assume that the nodes are in the same feature space, even though they are of different types or scale poorly to net-scale. This paper tries to resolve these issues.

An important tool is attention on **meta relation triplets**, which consist of the type of the starting node, the relation type and the type of the ending node, which enables all nodes to keep their feature space.

## Architecture
The model consists of three parts which come into effect after extracting all linked node pairs. First the attention, then the message passing to the aggregation.

### Heterogeneous mutual attention
Attention between source node *s* and target node *t* is computed using meta-relation triplets, as they are not feature space dependent.

Like the usual attention, *t* is mapped to a query vector and *s* is mapped as key. The dot product is then the attention.

The difference to the usual attention is, that in this case there is a different vector embedding for each meta-relation type. This means:
- the **key** is source node type dependent and computed from the last hidden state of the node
	- the length is *d*, where for each of the *h* only d\/*h* rows are active
- the **query** is target node type dependent and computed from the last hidden state of the node
	- the length is *d*, where for each of the *h* only d\/*h* rows are active
- the **attention matrix** is based on the relation type and layer independent
	- to differentiate between possible multiple relations between source and target
- the **prior tensor** which is a scalar, computed over the whole triplet

### Heterogeneous message passing
Like usual message passing, but again multiple heads, which have their own dimensions in the latent space.

First the head-specific dimensions are elected from the source node, which then get multiplied by a matrix to incorporate edge dependency.
### Target specific aggregation
Like normal attention, the attended messages get added together to get a pre-hidden state, which then gets converted using a target type specific process and added to the hidden state of the previous layer. 
## Sampling
The goal of the sampling is to find a similar amount of nodes and edges for each type while keeping the sub-graph dense.

This is done with a separate node budget for each node type, which takes into account the normalised degree of the nodes. 
## Experiments
Since there are many parameters per meta-relations, it might be beneficial for some less-used triplets to use node sharing.

The Open Academic Graph \(OAG\) was used as basis, as it is the biggest co-author graph.

The tasks are edge classification and node classification. The *papers* are the attended hidden state of the title. The *author* is the average of his published papers. *Field*, *venue* and *institute* are metapath2vec embeddings.

There are 8 attention heads which use the 256 hidden dimensions. The amount of parameters and batch time is about equal to previous heterogeneous approaches, but having better precision. Also the metapaths with the highest attention are compatible with domain knowledge.