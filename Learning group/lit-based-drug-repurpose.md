# [A Literature-Based Knowledge Graph Embedding Method for Identifying Drug Repurposing Opportunities in Rare Diseases](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6937428/)
## General
Rare diseases are rare and often not worth the new development. Repurposing other, already FDA-approved drugs might help.

This model will be the first, that also includes uncertainty in predictions.
## Global Network of Biomedical Relationships ([GBNBR](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6061699/))
Large, heterogeneous knowledge graph comprising of drugs, diseases and gene/protein entities and their support score.

Extract [MeSH](https://www.ncbi.nlm.nih.gov/mesh/), [OMIM](https://www.omim.org/) and [UMLS](https://www.nlm.nih.gov/research/umls/index.html) IDs for each rare disease in [Orphanet](https://www.orpha.net/consor/cgi-bin/index.php). [DrugCentral](https://drugcentral.org/) was used to find FDA-approval.

GNBR was filtered afterwards and all nodes were removed which were not a gene node, a high-priority rare disease or a drug/disease node, associated with a known indication.

## Model
All the nodes and edges were embedded in a graph embedding.

g(triple)=relationship_confidence\*(start_confidences^T end_confidences) &rightarrow; closer start and end have higher probability

confidence(triple)=min(max(weights\*g,0),1), where the weights are learnt.

Changing the end is used to generate negative triples and as a loss for the training of the weights the squared error of predictions to support(confidence in GBNR) was used.
## Experiments
Removing non-treatment drug-disease themes decreases performance markedly, as expected because the other drug-disease themes are semantically related and often correlated in support scores. Therefore, many training examples that would positively contribute to the final embeddings are lost.

Highest accuracy if only disease and treatment are kept in graph, but makes it impossible to find repurposable drugs.

Embedding gets tested with UMAP and positive and negative get coloured and compared. The embeddings are also tested on cosine similarity.