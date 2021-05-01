# [Graph Matching Networks for Learning the Similarity of Graph Structured Objects](https://arxiv.org/abs/1904.12787)
## Introduction
This paper tries to find a way to model (sub-) graph similarity, such that one can compare a test graph with a database of known graphs.

The similarity test will be done in the latent space, to control the size of the graphs and the computing complexity.

The similarity will be measured through cross-graph attention-based matching.

There are already is a lot of research in graph similarity using hand-crafted metrics, such as distance between two nodes or the edit distance \(add/remove/substitute nodes and edges).

A good similarity metric should take into account:
- the graph structure
- the learnt semantics
## Graph Embedding Models
Classical graph embedding models embed a graph into a vector and check for similarity from there.

Often, this embedding comprises of three parts:
1. encoder \(usually just two MLPs, one for nodes and one for edges)
1. propagation layers, the more layers, the further neighbourhood the node can discover
	1. edge update, that takes as input the encoding of the two nodes and the edge into a neural net
	1. node update, that takes as input the encoding of the node and the \(weighted) sum of the embedding of all incoming edges into a neural net
1. aggregator over the whole graph
	- h_G=MLP_G(&Sigma;_{vertex}(&sigma;(MLP_gate(h_{vertex}^{Layer}))&times;MLP((h_{vertex}^{Layer}))))
	- MLP_gate acts as a gating mechanism, to ignore certain nodes
	
In an additional step, these embeddings get compared with each other using an MLP or another model.
## Graph Matching Networks
Virtually, the two graphs get extended with special edges connecting each node of one graph with each node of the other graph and vice versa. This means, if the two graphs were completely disconnected originally, we would have the maximal bipartite graph afterwards.

The parts are now:
1. encoder \(usually just two MLPs, one for nodes and one for edges)
1. propagation layers, the more layers, the further neighbourhood the node can discover
	1. edge update, that takes as input the encoding of the two nodes and the edge
	1. **match update over the special edges, that takes as input the encoding of the two end nodes**
		- the match update is a rather simple weighted subtraction of the embedding of the other node from the embedding of the centre node
		- the weight is the normalised vector distance of the two embeddings
	1. node update, that takes as input the encoding of the node, the \(weighted) sum of the embedding of all incoming edges **and the \(weighted) sum of all match embeddings**
1. aggregator over the whole graph
	- h_G=MLP_G(&Sigma;_{vertex}(&sigma;(MLP_gate(h_{vertex}^{Layer}))&times;MLP((h_{vertex}^{Layer}))))
	- MLP_gate acts as a gating mechanism, to ignore certain nodes
1. **perform a simple vector space similarity between the two h_G**

### Drawbacks
The **computation cost** gets polynomial to O(|V_1|&times;|V_2|+|E|) from O(|V|+|E|) because of the match update.

Also the **representations are dependent on each other**, as during the layers, the two graphs interacted with each other and the representations got influenced by the other graph.
## Training losses
There are two types of training tasks, checking if two graphs are similar or not and triplets, where the model has to decide which graph is closer.

For those two tasks there are two losses:
- L_pair=max\{0,&gamma;-t\(1-distance\(G_1,G_2))}
	- *t* &rightarrow; 1 if similar, -1 if dissimilar
	- *&gamma;* &rightarrow; the margin
- L_triplet=max\{0, distance(G_1,G_2)-distance(G_1,G_3)+&gamma;}
	- G_2 is closer than G_3
	- *&gamma;* &rightarrow; the margin

To use binary representations for some better nearest neighbour searches one can use:
- L_pair=(t-s(G_1,G_2))^2/4
	- *s(G_1,G_2)* &rightarrow; the element-wise sum of tanh\(h_\{G_1,element})&times;tanh\(h_\{G_2,element})\/\#elements
- L_triplet=\(\(s\(G_1,G_2)-1)\^2+\(s\(G_1,G_2)-1)\^2)\/8
	- G_2 is closer than G_3
	- *s(G_1,G_2)* &rightarrow; the element-wise sum of tanh\(h_\{G_1,element})&times;tanh\(h_\{G_2,element})\/\#elements
## Experiments
Nodes have 32 features and the graph has 128 features. More propagation steps lead to better results, tested until 5 steps.

[Weisfeiler Lehman kernel](https://ethz.ch/content/dam/ethz/special-interest/bsse/borgwardt-lab/documents/slides/CA10_WeisfeilerLehman.pdf) is used as one baseline. Another baseline are **Siamese networks** which are two distinct, but entangled networks that compute a representation each and a smaller network then takes these representation and computes a similarity score.

[Graph Convolutional Networks](https://arxiv.org/abs/1609.02907) generally perform better than the baseline with the kernel, GNN are better than the GCNs and the GMNs are better than the GNNs even if they are in a siamese configuration.