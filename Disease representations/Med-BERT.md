# [Med-BERT: pre-trained contextualized embeddings on large-scale structured electronic health records for disease prediction](https://arxiv.org/abs/2005.12833)
## Introduction
EHR data is often sparse and therefore transfer learning would be very beneficial. EHRs generally are \(ordered\) sets of codes, much like NLP are sentences of words. This parallel leads to the idea of using the in NLP widely used BERT model also in the EHR domain.

This paper employs the BERT model on a comparatively huge multi-visit cohort to give ICD code representations.
## Data
There are two different databases:
- [Cerner Health Facts](https://sc-ctsi.org/resources/cerner-health-facts) &rightarrow; 70 million patients of which 30 million are usable
- [Truven Health MarketScan](https://marketscan.truvenhealth.com/marketscanportal/) &rightarrow; 42 thousand patients
## Med-BERT
### Input
The data is transformed to annotate:
- code embedding &rightarrow; for each *word*
- serialisation embedding &rightarrow; to embed the position in the *sentence*
- visit embedding &rightarrow; the position of the *sentence* in the *document*

There are no *CLS* or *SEP* tokens as there is no next sentence prediction and sentence summarisation makes no sense for long histories.
### Pre-training
There are two pre-training tasks used for this task:
- standard masked token &rightarrow; like BERT 80\% of replacing with *MASK*, 10\% of randomising the code, 10\% of keeping the code
- predict prolonged length of stay &rightarrow; instead of question-answer, the length of stay was predicted, as it is an important feature for quality that is also often reported
## Experiments
As downstream task disease prediction is used, this is remarkable as it is something else than the code prediction, which was also done in pre-training.

The embeddings were used in different architectures:
- GRU
- Bi-GRU
- RETAIN
- Feed-forward-layer

On the three different cohorts, the Med-BERT embeddings always performed better than without embedding or word2vec embeddings. Bi-GRU usually performed the best with the feed-forward on Med-BERT being always at least in the top-3 including the other kinds of embeddings.

Med-BERT, like all other Transformer methods, offer some interpretability with the attention patterns. The first two layers are syntactic, the middle two layers show medical meaning and the last two layers are hard to interpret.