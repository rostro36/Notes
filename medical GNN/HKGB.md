# [HKGB: An Inclusive, Extensible, Intelligent, Semi-auto-constructed Knowledge Graph Framework for Healthcare with Clinicians’ Expertise Incorporated](https://www.sciencedirect.com/science/article/pii/S0306457320308190) 

## Introduction

There are several problems in incorporating knowledge bases into EHR data:

- many relationships are not expressive enough in the knowledge graph
- in the EHR record some information that was unimportant for the annotator may be useful for the model, this means missing information is not at random
- there are other health data sources
- compared to a more general domain like Wikipedia, there are too many too precise concepts and special relations in the medical domain

This paper proposes a model that tackles these problems and offers to integrate knowledge from domain experts in a *clinician-in-the-loop* mechanism for the cardiovascular domain.

### Knowledge graph types

There are different kinds of knowledge graphs, they are differentiated by the type of nodes they contain:

- only concept nodes &rightarrow; conceptual knowledge graph (CKG)/ontology
- entity nodes and event nodes &rightarrow; instance knowledge graph (IKG)

- conceptual, entity and event nodes &rightarrow; factual knowledge graph (FKG)/this paper

