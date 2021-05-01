# [Hierarchical graph embedding in vector space by graph pyramid](https://www.sciencedirect.com/science/article/pii/S003132031630200X?via%3Dihub)
## Introduction
A good embedding method should be fast \(not using slow\/global functions\) and still be informative.

The PyrGE framework, that this paper introduces should reduce the amount of information in a graph by "scaling down" the graph and therefore augmenting the classification accuracy.
## Architecture
The pyramid has many layers of clustering. Each layer reduces the amount of nodes by a factor of &lambda;.

There are three phases in each layer:
1. Generate super-nodes
	- Some \(spectral\) clustering algorithm is used on the given task-dependent affinity matrix and then k-means is used. 
1. Insert super-edges
	- Check if the percent of edges between clusters is above a pre-defined threshold to overpass.
1. Define labelling
## Implementation
To test the representations a simple k-NN and a SVM was used. The higher the level of abstraction, the worse the results \(they claim it is still similar, but I am not sold\).

Generally, more representations from different levels give better downstream results.