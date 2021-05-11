# [An open access medical knowledge base for community driven diagnostic decision support system development](https://bmcmedinformdecismak.biomedcentral.com/articles/10.1186/s12911-019-0804-1)

## Introduction

Clinical decision making are a application with huge potential, but is restricted by the available knowledge. This paper shows a way to generate a knowledge base, which unlike previous attempts tries to be interpretable.

## Data

The data is targeted at medical trainees in Mozambique and Sub-Saharan Africa, therefore has a wider range of tropical and infectious diseases.

The knowledge base contains 2000 [ICD-10](https://icd.who.int/browse10/2010/en) diseases, 450 [RX](https://www.nlm.nih.gov/research/umls/rxnorm/index.html) medications, 8000 [SNOMED](https://www.snomed.org/) or [LOINC](https://loinc.org/) observations.

## Knowledge graph construction

The knowledge graph is constructed using the Bayesian model of reasoning. Meaning the weight of a relation is determined by the Bayesian probability of the co-occurrence probability of a medication given some observations. Negative mentions were used as a -1 occurrence.

The ages were binned.

## Querying

Given a set of symptoms, the algorithm tries to find an as small as possible set of diseases that show these symptoms. A greedy algorithm is used to find to always pick the best fitting disease.

## Experiments

The experiments were performed on 117 cases extracted from medical journals and compared to other symptom websites.

Doknosis from this paper scores comparably to DXplain, where DXplain works better for hard cases in the US, whereas Doknosis excels in the Sub-Saharan territory.

## Limitations

The current system has problems with synonyms. The knowledge base is far from complete. The symptoms are not weighted, e.g. "how high is the fever".