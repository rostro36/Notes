# [Exploiting hierarchy in medical concept embedding](https://academic.oup.com/jamiaopen/article/4/1/ooab022/6172949)
## Introduction
Embeddings like Word2Vec are very popular also for ICD codes, but they suffer from having significantly less data and have a temporal dimension.

Poincaré embeddings are hard to map back to Euclidean space.

This paper tries to add clinical groups by the [*Revised* software](https://www.hcup-us.ahrq.gov/toolssoftware/ccsr/ccs_refined.jsp#download) to get hierarchical information on the embeddings.
## Model
There are two models trained:
- Word2Vec &rightarrow; maximise continuous bag-of-words \(CBOW\)
- Skip-Gram &rightarrow; distinguish between real skipped word and negative sample

## Metrics
The first training set was to find category clusters, which were evaluated with [Normalised Mutual Information \(NMI\)](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.normalized_mutual_info_score.html), [Adjusted Mutual Information \(AMI\)](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.adjusted_mutual_info_score.html) and [silhouette scores](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html). The clusters were found with K-means on the representations.

The second set of training was logistic regression to predict patient mortality and unplanned hospital admission.
## Results
Generally Skip-gram and CBOW performed substantially better than Med2Vec. Skip-gram then was slightly better than CBOW and the software codes also helped additionally.