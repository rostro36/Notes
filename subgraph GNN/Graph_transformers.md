# [Graph Transformer Networks](https://arxiv.org/pdf/1911.06455.pdf)
## Introduction
Generally, GNNs assume that the graph structure is fixed and homogeneous. One can not use convolution on a graph, where there are not all edges known or they may change.

## Meta Paths
In a heterogeneous graph, a meta path is a defined path structure such as *Author-Paper-Conference*, that starts from a node in the *author* class, goes to a *paper* node and ends in the *conference* node.
These paths can be computed by multiplicating the adjacency matrices for *author* &rightarrow; *paper* and *paper* &rightarrow; *conference*.

## Meta path generation
This paper suggests using one weight *W* per new meta-path variant during the multiplication of the current length adjacency matrix and the ground adjacency matrix with paths of length 1. Like this, after *l* layers, there are only paths of length *l+1*. To circumvent that, an identity matrix is also used additionally to the adjacency matrices, to also use paths of lower length.

To consider many, but not all paths, *C* of those adjacency matrices get concatenated together and put into a softmax model to perform node classification on them.
## Experiments
The used **datasets** are the two citation network sets DBLP and ACM and the movie dataset IMDB., each having four edge types and hundreds of node features.

The **conventional network embedding baselines** are DeepWalk, that is optimised for homogeneous graphs and metapath2vec. The **GNN baselines** are the homogeneous GCN and GAT, as well as HAN, that can use meta-paths as well.

The GNN based approaches performed much better than the conventional approaches. The GAN was better than the HAN trailed by the GCN. This shows that attention works well for these tasks and that the pre-defined meta-paths for the HAN had an adversary effect.

The highly weighted meta-paths that GTN found also align with the hand crafted meta-paths of domain experts.