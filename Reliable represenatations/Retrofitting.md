# [Retrofitting Word Vectors to Semantic Lexicons](https://aclanthology.org/N15-1184.pdf)
## Overview
Distributional word representations like Word2Vec are good, but disregard semantic/ontological information. Semantic information could be information like synonym, hyponym (sub-category), hypernym (super-category) or paraphrase (similar).
## Retrofitting
Retrofitting is a post-processing step that optimises the objective:
$$\Phi(Q)=\sum_{i=1}^n[\alpha_i||q_i-\hat{q}_i||^2+\sum_{(i,j)\in E} \beta_{ij}||q_i-q_j||^2]$$
- *Q* & *q* &rightarrow; post-embeddings
- *^q* &rightarrow; pre-embeddings
- *n* &rightarrow; word
- *E* &rightarrow; relation edges between two words *i* and *j*
- &beta; &rightarrow; parameter for relation

This can be optimized with the iterative method of doing the following iterations about 10 times:
$$q_i=\frac{\sum_{j:(i,j)\in E} \beta_{ij} q_j +\alpha_i \hat{q}_i}{\sum_{j:(i,j)\in E} \beta_{ij}+\alpha_i} $$

In the evaluation later &beta; is set as the reciprocate of the degree of the word *i* and &alpha; is set at 1.
### Previous work
In comparison, previous work used the a prior **during** training instead (called *MAP*):
$$p(Q) \propto exp(-\gamma\sum_{i=1}^n\sum_{j:(i,j)\in E} \beta_{ij}||q_i-q_j||^2) $$
where &gamma; is used to regulate the strength of the prior.

## Experiments
### Lexica
To get the semantic information i.e. the semantic information different lexica are used:
- *PPDB* &rightarrow; paraphrases translated and translated back
- *WordNet* &rightarrow; manual of synonyms and subset
- *FrameNet* &rightarrow; word used in same frame/situation

### Metrics
To show the performance of the method tests in four different areas are used:
- word similarity &rightarrow; ranking words using the cosine similarity and using spearman's to compare to human annotators
    - WS-353
    - RG-65
    - MEN
- syntactic relation &rightarrow; *a* is to *b* as *c* is to *d*
- synonym selection &rightarrow; find closest word in multiple-choice
    - TOEFL
- sentiment analysis
### Results
*SkipGram* performed better than *GloVe* and using (only) *PPDB* to retrofit performed best, also better than using no retrofitting, except for the syntactic relation benchmark, where retrofitting was counter-productive.

Futher, the same relations in different word pairs now also have the same angle. As compared to before, where the relation were a lot messier. 

It was also shown that this method works in other languages and bigger vectors perform better, at least until a dimension of 1000, but it starts to plateau at 600.