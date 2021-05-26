# [Real-world data medical knowledge graph: construction and applications](https://www.sciencedirect.com/science/article/pii/S0933365719309546)

## Introduction

Knowledge bases are very important and have made great strides in recent years. The ones created by hand take a huge amount of skilled work; 15 person-years were reported. Automatic approaches are less labour intensive, but are generally less precise and suffer from the problem of reading natural languages.

This paper describes the different tasks to do and applies it to a big EHR data set.

## Method

To create a knowledge graph from EHR data the following steps have to be done:

1. data preparation
   - Bundle all visits of the same patient together
1. entity recognition
   - Use NER technologies, such as a CRF or LSTMs, on the descriptions of the codes or if there exist, on the clinical notes.
1. entity normalisation
   - Synonymous entities are mapped to a clear standardised identifier. Such as ICD-10 or binned age.
1. relation extraction
   - The disease is seen as the centre of the relation, therefore all other 8 types are based around a disease and form 9 relations including with diseases themselves. They only take the main diagnosis as the driving factor and ignore side diseases. But all ancestors of the ICD code also get that relation.
1. property calculation
   - A quadruplet is constructed from the <disease, relation, object> triplet, by adding properties like occurrence, specificity, tf-idf and reliability.
1. graph cleaning
   - To remove noise delete entities if the occurrence is below a threshold, or the edge probability is too low.
1. related-entity ranking
   - To favour some medication or exams, the other nodes should be ranked. The paper proposes to use the product of **probability**\* **specificity**\***reliability**. 
1. graph embedding
   - PrTransH, a TransE derivative is used to find the graph embeddings of the nodes.

## Experiments

There are several experiments done to test the various tasks, i.e. for entity recognition, entity ranking and graph embedding. Also a clustering was done on the graph embeddings.

### Applications

**Clinical decision support** takes as input the information about the patient and predicts diagnoses or treatment, which can easily be done using the properties form the quadruplet and a naive Bayes classifier.

**Information retrieval** even inside the hospital intranet and the medical domain, which even doctors don't know by heart.

**Knowledge transfer** to MIMIC-III was also possible.