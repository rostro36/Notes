# [Deep EHR: A Survey of Recent Advances in Deep Learning Techniques for Electronic Health Record (EHR) Analysis](https://arxiv.org/abs/1706.03446)
## EHR applications\/tasks
- Information extraction from clinical texts using NLP
	- Single concepts
	- Time frames
	- Relations between concepts
	- Abbreviation expansion
- EHR representation learning
	- Concept representation \(like heart failure\) e.g. using skip-gram or auto-encoders
	- Patient representation, seen as sentences of concepts
- Outcome prediction
	- Static outcome \(classical\)
	- Temporal outcome, assisted with RNNs
- Phenotype discovery
	- Clustering similar patients e.g. using AEs which makes it more robust to missing modalities
- De-identification
- Interpretability
	- Attention
	- Constraints
	- Clustering
	- Mimic deep net with interpretable one
## EHR datasets
- [MIMIC](https://mimic.physionet.org)
- [i2b2](https://www.i2b2.org/NLP/DataSets/)
## Miscellaneous
Doctor AI has shown that pre-training, even on different datasets can help.

Readmission, disease progression, disease progression 