# [Ontological attention ensembles for capturing semantic concepts in ICD code prediction from clinical text](https://www.aclweb.org/anthology/D19-6220.pdf)
## Introduction
Medical code assessment is an important task, that has to deal with the problem of the high number of codes, class imbalance, ambiguous language and inconsistent/bad coding.
## Method
### Word embedding
The codes are embedded using Word2Vec and a one-dimensional CNN with stride 1 with 4 filters of different windows sizes was applied.
### Standard document embedding
In the first mode, normal attention is used to generate a uniform-length document representation vector, which then gets put into a sigmoid to generate the label probabilities.
### Ontological attention
In the second mode, ontological attention is used. This means there is a normal attention head for every code, a parent head for every family of code and a grandparent head for every grand family of code.

All the heads give a vector representation for their level. To train, there is now a current level class labelling task on the representation concatenated to it's ancestor representations.
### Training tricks
To train well, some tricks have to be applied:
- **Batches ordered by size** &rightarrow; Reduce the amount of padding per batch.
- **Dampened class weighting** &rightarrow; the learning rate is scaled against the class imbalance with
$$ weight_{code,label}=(\frac{#all}{#all_{code,label}})^\alpha $$
- **Multitask** &rightarrow; the different tasks, one for each level, where weighted differently, each higher level with 0.1 the weight of before
## Experiments
The proposed method outperforms CAML on MIMIC-III. The ontological attention noticeably helped the model, but adding another layer of great-grandparents was not beneficial.