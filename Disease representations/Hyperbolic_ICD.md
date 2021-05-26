#[Learning Electronic Health Records through Hyperbolic Embedding of Medical Ontologies](https://dl.acm.org/doi/abs/10.1145/3307339.3342148)
## Introduction
Readmisssion to ICU or mortality prediction are important metrics for the quality of healthcare and probabilities for them can help to improve the decisions of health professionals.
This paper introduces hyperbolic embeddings to exploit the hierarchical nature of ICD-codes to augment performance of models.
## Method
The medical concepts are embedded in the hyperbolic space where direct relatives attract each other and negative samples repel each other.
The MIMIC-features get used concatenated to ICD-9 codes from the discharge summaries, which first have to be automatically extracted and are then embedded into the hyperbolic space. 
The normal inputs and the ICD-9 codes, which are concatenated together, then get put through a multi-filter CNN before getting concatenated together and put into a dense layer and ready for readmission prediction. For mortality prediction a RNN is used instead of a CNN.
## Experiments
The MIMIC-III dataset was used, but a automatic ICD-9 annotation system was used to get the codes.
### Intrinsic evaluation, hierarchy similarity correlation
To test the similarity four ontology-based similarity measurements are used:
- Wu\&Palmer similarity
- Leacock&Chodorow similarity
- Resnik similarity
- RADA similarity
20'000 concept pairs are randomly sampled and the Pearson Correlation Coefficients are computed between the similarity scores and the distance in the (hyperbolic) embedding space.
The Poincaré embedding is the best even among the better hyperbolic embeddings compared to the Euclidean embeddings.
### Extrinsic evaluation
The two tasks for extrinsic evaluation are:
- 30-day unplanned ICU readmission
- In-Hospital Mortality Prediction
Where in both cases TransE performed better than Poincaré, but constraining the dimensions to e.g. 100 instead of 300 showed that Poincaré is more resource efficient.
Also incorporating the ICD-codes of the medical notes have shown a positive effect on performance even though it was only done by an automatic system.
