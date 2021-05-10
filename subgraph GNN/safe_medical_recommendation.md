# [SMR: Medical Knowledge Graph Embedding for Safe Medicine Recommendation](https://arxiv.org/abs/1710.05980)

## Introduction

SMR bridges the gap between knowledge graphs such as ICD-9 and DrugBank and to EHR records like MIMIC-III. SMR jointly embeds diseases, medicines, patients and their relations into a shared space. SMR then predicts a new medicine as linked prediction based on the diagnosis, the already used medicine and the known adverse drug interactions.

This system has to deal with the following problems:

- computational efficiency
- knowledge base incompleteness
- cold start of new therapies

## Data

There are three kinds of data: 

- **knowledge base** &rightarrow; set of heterogeneous triples that makes a general heterogenous graph
- **patient-medicine bipartite graph** &rightarrow; the relations are weighted proportional to the amount a patient has taken a medicine
- **patient-disease bipartite graph** &rightarrow; the edge is only 1 if the disease is diagnosed, else it is 0

The task now is to predict links between the patient *p* and the medicine *m* with a minimum of adverse drug reactions.

## Model

### Knowledge base embedding

The model uses TransR to embed the knowledge base as it is pretty effective and very efficient. TransR sees a relation \<*h*,*r*,*t*\> as a translation from *h* to *t*. i.e.

$z(h,r,t)=b-||hH_r+r-tH_r||_{L1/L2}$

The  probability of a triple is then defined as follows:

$P(h|r,t)=\frac{exp(z(h,r,t))}{\sum_{\hat{h}\in N}exp(z(\hat{h},r,t))}$ the model then tries to maximise the log-likelihood of the seen triples.

### Bipartite graph embedding

LINE is used for the bipartite graph.

The probability that a patient $p_i$ takes a medicament $m_j$ is $P(m_j|p_i)=\frac{exp(z(p_i,m_j))}{\sum_{\hat{m}\in P_i}exp(z(p_i,\hat{m}_j))}$ with $z(p_i,m_j)=m_j^Tp_i$. The objective to optimise is the KL divergence between the predicted probability and the empirical distribution.

### Optimisation and training

All the graph embeddings are trained together and the sum over all losses is trained together. Additionally a regularisation term is introduced for stability and negative sampling is used for efficiency.

### Safe medicine predictions

A new patient is presented by the sum of the disease embeddings $d$, with lower importance for older diseases: $p=\sum_{t=1}^n exp^{-t}d_t$

The new medicines $m_n$ are ranked according to their score $S(q,m_N)=p^Tm_n-\sum_{o=1}^{n-1}||m_n+r_{interaction}-m_o||_{L1/L2}$ where $m_o$ are the already selected medicines. **no combos possible**

## Experiments

The SMR model performs better than LINE and rule based approaches as it can include medical knowledge. This is also supported by medical professionals.

## Outlook

The linking accuracy should be improved by utilising more patient information such as clinical outcome or demographics. Also no medication combinations are possible and there is no information flow between the three knowledge graphs.