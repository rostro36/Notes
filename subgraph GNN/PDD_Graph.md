# [PDD Graph: Bridging Electronic Medical Records and Biomedical Knowledge Graphs via Entity Linking](https://arxiv.org/abs/1707.05340)

## Introduction

This paper tries to link MIMIC-III EHR data to knowledge graphs, in this case the ICD-9 ontology and DrugBank. The writers hope that by linking the diagnoses and different drugs a patient takes a fuller picture can be drawn.

The authors draw a patient-drug-disease graph that can also be accessed with SPARQL.

## Graph architecture

The graph follows the RDF model of triplets.

- **Patient IRI** &rightarrow; Attach an IRI prefix to the **patients** de-identified ID from MIMIC-III
- **Prescription triple generation** &rightarrow; Actor is the patient IRI, then the is a *pdd:prescribed* relation used to the name of the drug.
- **Diagnosis triple generation** &rightarrow; The NLP notes are extracted and the NLP model C-TAKES is used for NER of diseases, which then get assigned to ICD-9 codes. These things then form the triplet of the *patient IRI*, the relation *pdd:diagnosed* and the ICD code.
- **Drug entity linking** &rightarrow; An NLP model is used to map the name of the drug in the prescription triplet to the DrugBank name.

## Experiments

Tests made by hand generally have good recall and precision. The linking to the knowledge bases works well in most cases, there are some exception when the medicine names are very short or there are errors or NULLs.  The main bottleneck for performance is the Named Entitiy Recognition.

