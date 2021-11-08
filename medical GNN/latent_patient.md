# [Latent Patient Network Learning for Automatic Diagnosis](https://arxiv.org/abs/2003.13620)
## Introduction
This paper introduces a method to learn the underlying patients-graph given their electronic health record. The general idea is that each patient is a node and they share information based on the proximity of their embeddings. The paper analyses a way to find the adjacency matrix between them.
## Method
The method learns a latent graph, given the nodes, in other words they find the best possible adjacency matrix *A*.

Like Welling's graph autoencoder, each node gets embedded using an MLP. The weight of the edge then is computed using the sigmoid on the Euclidean distance between two embeddings.

The message is being passed around using a normalised spatial GC layer.
## Experiments
TADPOLE neuro-disease prediction and UK Biobank data for age prediction was used.

Linear classifiers were much worse than the graph based ones. The spectral GCN could not work with the big UKBB graph. The proposed model performed best comparable with DGM in accuracy and AUC, while having less parameters.
