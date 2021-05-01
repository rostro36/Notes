# [Multi-Task Graph Autoencoders](https://arxiv.org/pdf/1811.02798.pdf)
## Introduction
The paper studies the two subtasks of link prediction and node classification. The model can learn self-supervised an encoder and a decoder.

Node representations
## Problem formulation
The input is a graph with *N* nodes *V* and edges *E*, which are represented by the adjacency matrix *A* and tries to reconstruct *A* with *Â*. There are only a **1** for an existing edge and **0** for an inexistant edge, some edges are not observed/to be inferred and are denoted as *?*. The nodes have features *X*. The goal of the autoencoder is to come up with a latent node representation *Z*, that allows to reconstruct the edges.
## Architecture
The **encoder** takes all the edges of a node and consists of a two layer neural net with a ReLU activation.

### Link prediction
The **decoder** for link prediction only has one round of ReLU, but shares the learnt weight matrices with the encoder, but the bias is still unique.

The **loss** is the Masked Balanced Cross-Entropy (MBCE) loss, that uses a weight factor *c*=1-\#positive\/#negative to get the Balanced Cross-Entropy first.

L\_\{BCE\}=-a\_i log\(&sigma;\(â\_i\)\) c -\(1-a\_i\)log\(1-&sigma;(â\_i\)\)

And then applies the mask *m_i*, which is 1 if the edge state is known and 0 else. It is applied with the Hadamard product \(&times;\) to get the MBCE.

L\_\{MBCE\}=&Sigma;\_i m\_i &times;L\_\{BCE\}\/&Sigma;\_i m\_i
### Node Classification
The **decoder** for node classification uses the same ReLU setup as link prediction with the same shared first weight matrix, but uses a new, non-shared weight matrix afterwards before putting that representation into a softmax to predict the class probability *^y*.

The **loss** is a masked cross entropy, that masks out nodes that do not have a label and then tests all classes *c*

L\_\{nodes\}=-MASK\_i&Sigma;\_c y\_\{ic\}log\(^y\_\{ic\}\)
### Multi-Task Learning
Both decoders get used in parallel and the loss is the addition of the previous losses.

L\_\{Multi-task\}=L\_\{nodes\}+L\_\{MBCE\}

The overall complexity is linear in the number of nodes. \(**Not in the number of edges as well?**\)

## Evaluation
To evaluate a Pubmed, Citesser and Cora dataset are used with partially labelled nodes was used additionally to Arxiv-GRQC and  BlogCatalog, that have no node labels.

To benchmark, labels or edges were masked, randomly, but balanced on classes.