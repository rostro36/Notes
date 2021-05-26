# [SUGAR: Subgraph Neural Network with Reinforcement Pooling and Self-Supervised Mutual Information Mechanism](https://arxiv.org/abs/2101.08170)
## Introduction
Graph representations face the following problems:
- Discrimination &rightarrow; over-smoothing, such that details, both local and global, get lost
- prior knowledge &rightarrow; many methods need some motifs\/graphlets, which are pre-selected
- interpretability &rightarrow; some methods use coarsening of structures, which makes them hard to interpret

This paper introduces SUGAR, which addresses these problems.
## Architecture
Sugar consists of three main parts:
- subgraph sampling
- subgraph selection with the reinforcement pooling module
- subgraph sketching 
- and a loss.
### Subgraph sampling
Choose the *n* nodes with the highest degree. Then use a BFS from them to get *s* nodes in per subgraph. The goal of this is to maximise the coverage of the original graph.

Use normal message passing to get node embeddings. An intra-subgraph attention mechanism then is used to find a subgraph representation. The attention weights are shared among all nodes of the subgraph. The subgraph representation is the sum of the attended node embeddings.
### Subgraph selection
Only the top *k* percent nodes are selected. This selection is done by the length of the projection from the subgraph embedding to a trainable vector *p*. The rank then is used as a weight for the next step.
### Subgraph sketching
Subgraphs are taken as super-nodes and they are connected if they share more than a threshold of normal nodes.

This new supergraph then computes the final subgraph embedding for each supernode\/subgraph with graph attention message passing.

Each subgraph then gets put into an MLP for class estimation. All subgraphs are ensembled together via sum to give the final class estimation.
### Reinforcement pooling module
The task is to find the best *k*, so to not use a hyperparameter. This is done with reinforcement learning on the parameter and uses the accuracy as the reward.
### Loss
The loss consists of three parts which are summed together with hyperparameters each:
- classification loss &rightarrow; cross-entropy
- MI loss &rightarrow; MI from below, similar to KL-divergence
- regularisation &rightarrow; L2-norm on the parameters

The MI loss should ensure that the subgraph representations are mindful of the global structural properties. This is done with the [Jensen-Shannon MI estimator](https://en.wikipedia.org/wiki/Jensen%E2%80%93Shannon_divergence).

A discriminator is introduced that uses a sigmoid on the dot product of the subgraph embedding and the sum of the subgraph embeddings and a weighting functions that tries to differentiate between real samples and negative samples from other graphs. The loss is then a binary cross entropy loss on the prediction of the discriminator.
## Experiments
SUGAR is competitive with other graph classification methods.

The start embedding only plays a marginal role.

Generally, the bigger the neighbourhood of the subgraphs, the better the result, but it starts to plateau at around 5.

Attention works as expected in finding interesting subgraphs.