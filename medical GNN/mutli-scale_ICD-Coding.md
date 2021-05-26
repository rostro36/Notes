# [EHR Coding with Multi-scale Feature Attention and Structured Knowledge Graph Propagation](https://dl.acm.org/doi/10.1145/3357384.3357897)
## Introduction
The task of automatically assigning ICD codes to patients or rather their electronic health records is important and quite hard, as there are a lot of ICD codes, which are unbalanced and from the written notes only a small part is really important for coding.

This paper uses NLP attention on variable n-grams layer and then uses the ICD hierarchy with a GNN to allow for some transfer learning.

The EHR coding problem is seen as multi-label document classification. 
## Architecture
The model consists of three main parts:
- multi-scale feature extraction
	- embedding layer
	- densely connected convolutional layer
- attention for document representation
	- multi-scale feature attention
	- label-dependent attention
- knowledge graph propagation
### Embedding layer
Pre-trained word2vec is used to convert the words to 100 dimensional vectors.
### Densely connected convolutional layer
There are *k* stacked convolutional blocks\/layers which are densely connected and take *w* tokens each. Meaning the *l*-th block takes as input all previous *l*-1 outputs, this also means the *l*-th block has sees \(*w*-1\)\**l*+1 tokens.
### Attention for document representation
There are two parts to attention:
- multi-scale feature attention  &rightarrow; each of the *k*\**m* outputs from the feature extraction get an attention weight for each layer and then they get added together to get a representation vector for each word.
- label-dependent attention &rightarrow; using a softmax on the output of the previous attention layer and the representation of the code description a code dependent attention scalar is computed for each word.
### Knowledge graph propagation
The official NLP description of the ICD gets fed into a pre-trained Text-CNN. At the end there is a representation for each ICD-code, which gets used as node representations in the hierarchy graph. There are some GNN steps until the leaf nodes\/ICD-codes get extracted which are later used for the final multi-label classification.
### Output layer
The document representation gets put in a sigmoid together with each label representation, which gives for each label a probability. Instead of simply taking the label if the probability is over a threshold, a meta learner that predicts the amount of labels as *t* from the text representation first is used. The *t* top labels then get chosen.

The loss then is binary cross-entropy for the label probability and mean square error for the amount regression.
## Experiments
MIMIC-III was used for IHR data. There are two modes; one with all labels and one with only EHR data with the top-50 labels.

The proposed model performs best in F1 and AUC in both modes. There is a significant gain from using the graph convolution instead of only using the base representation. Attention also works as expected.