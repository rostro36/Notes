# [BioWordVec, improving biomedical word embeddings with subword information and MeSH](https://www.nature.com/articles/s41597-019-0055-0)
## Introduction
Instead of wordvectors it can be beneficial to create subwordvectors and then aggregate the subwords together. This is due to better learnability, as a word like *haemophilia* containing two subwords *heamo* for *blood* and *philia* for *attraction*, which do translate well to other words. This means new words can be deducted easily and there is more information gained from a single word.

## Method
There are two kinds of embeddings used in this paper and then get aggregated:
- subword information
- ontological information

### Subword information
For the subword embedding Facebook's [fastText](https://arxiv.org/abs/1607.04606) 4-grams were employed on biological literature; namely PubMed XML.

### Ontological information
Use node2vec and sample a 100-step long random walk from a central node on MeSH and then transition on an edge with translation probability &alpha; dependant on the distance from the first node to the target node:
$$ P(c_i=x|c_{i-1}=v)=\alpha(t,x)\\\alpha(t,x)= (d_{tx}==0) \rightarrow(\frac{1}{p})\\ \alpha(t,x) =  (d_{tx}==1) \rightarrow(1) \\ \alpha(t,x)= (d_{tx}==2) \rightarrow(\frac{1}{q})$$
The resulting random walks get treated as sentences for fastText again.
### Combining subword information and ontological information
To combine the two sets of information the embedding gets trained simultaneously with both losses added together; this means the embeddings are for both methods the same and optimised together. It was chosen to have a dimensionality of 200.

## Experimnents
There are two kinds of experiments performed: extrinsic tasks, that predict a real-world scenario and intrinsic tasks, that predicts something about the code (hierarchy) themselves.

It can be seen that the window size for extrinsic tasks should be rather low at around 5 and higher for intrinsic tasks at about 20.

Just looking at some examples it can be seen that even for rare words the words with biggest cosine similarity are close in meaning, which is a good result.

### Term pair similarity
For more normed tests *UMNSRS-Sim* and *UMNSRS-Rel* were employed, which consist of hand-crafted term similarities which were ordered by domain experts. BioWordVec performed best with getting the highest Pearson and Spearman scores to the ordering. General methods from English Word2Vec performed significantly worse, domain-specific versions for Word2Vec performed better than the general approach, but still worse than BioWordVec.
### Sentence pair similarity
The BioCreative/OHNLP STS dataset was used consisting of 1'068 pairs of sentences from clinical notes and annotated by domain experts.

BioWordVec again performed best, but the influence of MeSH is not as big as for PubMed articles.
### Biomedical relation extraction
For this tasks drug-drug-interactions (DDI) and protein-protein-interactions (PPI) were searched. The underlying datasets for PPI are AIMed, BioInfer, IEPA, HPRD50 and LLL. For DDI the dataset was the DDI 2013 corpus.

To test the embeddings both a CNN and a RNN were used and it was shown that BioWordVec could get the best scores, especially on the simpler CNN model.