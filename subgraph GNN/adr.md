# [Drug-disease Graph: Predicting Adverse Drug Reaction Signals via Graph Neural Network with Clinical Data](https://arxiv.org/abs/2004.00407)
## Introduction
Adverse drug reactions are a major problem and are one of the top reasons of death. There are some pre-market tests and post-market reporting, but it is hard to test all possible combinations in a lab, so machine learning and graph neural networks to the rescue.

The dataset is a one million patient big electronic health record \(EHR\) dataset.
## Graph construction
Node embeddings are based on NHIS-NSC features, which are a sequence of hospital visits. A skipgram model is used to find one part of the embeddings of the diseases and treatments prescribed in the hospital visits. The other part is based on the [ICD-10](https://en.wikipedia.org/wiki/ICD-10) code of the medical entities.

Homogeneous edges are constructed and weighted according the distance between the nodes. The heterogeneous edges are weighted according to the amount of prescription *j* being given against disease *i*, divided by the amount of disease *i*.
## Method
After the graph is constructed a message passing GNN with attention or normal GCN on the edges is applied. The first hidden state is computed a non-linear projection \(neural net\). The task is to predict links between diseases and drugs, which is done using a sigmoid on the weighted dot product of the last layer embeddings of the nodes.

## Experiments
NHIS-NSC consists of 607 drugs and 556 diseases,which indicated 28\'746 drug-side effects. A lower threshold for constructing a heterogeneous edge is beneficial for the performance as more information flows for general prediction. To specialise on rare diseases a higher threshold on a heterogeneous \(each edge type had different message passing weights\) GCN yielded best results.

The highest "false positives", which are not in the data base seem to be new adverse drug reactions, or rather underspecified adverse drug reactions.