# [HyperCore: Hyperbolic and Co-graph Representation for Automatic ICD Coding](https://www.aclweb.org/anthology/2020.acl-main.282/)
## Introduction
ICD code annotation is a hard task, that, if it has to be shown to be done correctly, has to be done by a trained person.

The ICD codes have two important phenomena:
- they are hierarchical &rightarrow; different levels encode different level of precision
- there are some co-occurrences &rightarrow; sister codes hardly occur together, while there are some that occur together often

This paper tries to exploit the hierarchy with hyperbolic representations and co-occurrences with GCNs.
## Method
First the codes are embedded into the hyperbolic space and then a GCN is used to aggregate them.
### CNN encoder
Each word is put into a an embedding like Word2Vec and then the embedding is computed with a CNN that takes a window the words around and including the centre.
### Code-wise attention
Each ICD-code may focus on a different word, so for each code, attention is generated as a softmax on the mean of it's descriptor per document multiplied with the CNN embedding of the words. The code-aware document representation then is found by multiplying the attention with the document representation.  
### Hyperbolic code embedder
All codes are embedded in the Poincaré using the hierarchy relationship. Where directly related codes are attract each other and negative samples repel each other.
### Hyperbolic document projector
The document is embedded using two MLPs, one for the direction and the second for the magnitude. They start with the code-aware document representation.

To find the similarity between codes and documents, the distance is taken \(as the scalar product is ill defined in the hyperbolic space\).
### Code Co-graph
Each code is a node and the strength of the edge between the nodes is the amount of times they have been seen together. A normal GCN then is used to transfer information, the start is the average word embedding.
### Aggregation layer
In the aggregation layer, the hyperbolic similarity score and the GCN code embedding then get transformed with a matrix each and added together.
### Training
The code prediction is done with a softmax on the aggregated code vector. And the whole model is trained on multi-label binary cross-entropy.
## Evaluation
MIMIC-II and MIMIC-III were used. HyperCore beats the state of the art CAML rather convincingly.

The ablation study shows that the hyperbolic representation is more useful than the co-graph, but both advance the model.

The hierarchy can correctly be represented with the hyperbolic representation.