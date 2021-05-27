# [Pre-training of Graph Augmented Transformers for Medication Recommendation](https://arxiv.org/abs/1906.00346)
## Introduction
EHR data is powerful, but often has problems with edge cases of rare diseases or can not consider patients with only one visit. To better facilitate those cases this paper proposes G-BERT, that uses Graphs as well as the BERT architecture to recommend medications.
## Input
As input, there is EHR data of many patients with a different amount of visits each. A visit consists of ICD-9 codes for diagnoses and medications.

The other input is the ICD-9 and ATC ontology.
## Method
There are three main parts of G-BERT that feed the next part
1. GAT that creates code embeddings
	- First ancestors are updated from children
	- Then children get updated from all their ancestors
1. BERT with removed sentence position
	- Masked language model
	- Predicting medication from diagnoses and vice versa instead of next sentence prediction
1. Deep neural net
## Training
For pre-training single the EHR visits are split and the whole patient was not considered as an entity.

The loss is multi-label cross-entropy.
## Experiment
MIMIC-III was used and patients with more than one visit were considered.

Both parts of G-BERT seem to be important and together they make the model better than GAMENet.