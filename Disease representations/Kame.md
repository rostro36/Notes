# [KAME: Knowledge-based Attention Model for Diagnosis Prediction in Healthcare](https://dl.acm.org/doi/10.1145/3269206.3271701)
## Introduction
Previous models like Med2Vec do not use the temporal nature of ICD codes or Dipole does not scale down well with little EHR data. To solve this, the paper introduces KAME, that exploits the hierarchical nature of ICD codes in graphs to build more robust representations. The target of KAME is to predict the next visit.
## Model
### Leaf architecture
The input has three tiers
- codes &rightarrow; embedded using GRAM into the ontology
- visits &rightarrow; one-hot encoding of the seen codes multiplied with the embeddings
- patients &rightarrow; series of visits, combined with a GRU to a hidden state *h*
### Graph embedding
GRAM is used to embed the codes into the ontology. GRAM generates the embeddings of the codes *M* as well as the ancestor embeddings *A*.
### Knowledge-based attention mechanism
After one hidden layer DNN, the ancestor embeddings get attended with the *h* as the query and all other codes as the responses and formed to a knowledge vector *k*.
### Prediction
*h* and *k* get concatenated together and then fed into a softmax layer to predict the next visit codes. The loss is cross-entropy.
## Experiments
### Datasets
The used datasets are:
- the Medicaid dataset &rightarrow; 100k patients and 2 million visits
- Diabates dataset (subset of Medicaid, where the patients have a diabetes code) &rightarrow; 17k patients, 470k visits
- MIMIC-III with at least two visits &rightarrow 7,5k patients, 20k visits
### Baselines
The compared baselines are:
- RNN
- Dipole
- GRAM
And the performance with Precision or Accuracy@k is from top to down, with KAME being the best by some margin.
### Data sufficiency
To test how much data the models need, the results are still computed as normal, but the codes are evaluated by the percentile in how much they have been seen. KAME performs significantly higher in the lower categories compared to the baselines and it evens out the more often the codes appear.
### Interpretability
The attention part in the ancestor/knowledge vector computation allows for importance information, where the case study on one of the MIMIC-III heart patients performs as expected.
t-SNE graphs of KAME and GRAM show that the representations are in accord with the hierarchies.
