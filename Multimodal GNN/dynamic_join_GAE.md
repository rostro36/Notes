# [Dynamic Joint Variational Graph Autoencoders](https://arxiv.org/pdf/1910.01963.pdf)
## Introduction
This paper tackles the problem of finding good graph representations of dynamic networks, that may add or remove edges or nodes at different time steps.

The method in this paper does so by jointly learning latent variables at all timestamps based on the graph autoencoder.

To observe the dynamic evolution of a graph, the alignment has to be tracked, which leads to two issues:
1. the embedding vectors may not be in the same latent space
1. there is another optimisation problem just to find the best alignment

Previously dynamic graphs were often modelled with RNNs.
## Method
Dyn-VGAE tries to find a low dimensional representation of each of the *T* graphs. The first step is to use Welling's variational graph autoencoder on each of the graphs.

Next, the widespread assumption that the changes between the graphs are smooth is used to regularise the encoders, by minimizing the Kullback-Leibler difference between the Gaussian random walk generated on the previous embeddings and the current embedding. A fixed standard deviation is used.

The loss then is computed over all graphs. With one part being the GAE loss added to the smoothness loss from above, which is regulated by a scalar hyperparameter.
## Results
The method was tested with node classification on authorship graphs, where this approach outperforms benchmarks heavily even if only the last one or two previous graphs are used to regularise. The node classification was done as a linear regression on all possible previous graphs, where the node was part of.

For link prediction it looks similar, but only the last graph was considered. The method has problems with very sparse datasets, also in node recommendation, where top k nodes embeddings are tested with cosine similarity between two nodes.

The dynamic approaches take longer than the static ones, with more graph to test the similarity taking longer. In the case of only testing similarity with the previous graph, it takes only marginally longer.