# [SemaTyP](https://doi.org/10.1186/s12859-018-2167-5)
## General
Literature-Based Discovery (LBD) tries to read texts and find properties of diseases and drugs that solve these properties. One way to do so is with open information extraction, but it (hardly ever) gives logical explanations.

## SemKG
SemKG: Semantic knowledge graph

SemRep is used for relation extraction on PubMed. SemRep uses [MetaMap](https://metamap.nlm.nih.gov/) to map nouns to precise [UMLS](https://www.nlm.nih.gov/research/umls/index.html) concepts and then uses open information extraction to get triplets.
The triplets then get filtered and triplets that were only seen once per document get thrown out for better precision.

Relationship and entity type are UMLS concepts, edges are weighted in SemKG, PubMedID is also attached.
## SemaTyP
### Model
SemaTyP tries to find paths of length *l* from *disease* to *drug* via a *target*.

Each edge or node is described by a vector of its UMLS entries (there could be many). The length of the entity vector is *K* and the length of the relation vector is *M*.

Randomly replace one of drug, target, disease with another one and check if it does not exist to create negative training data.

x_i=concat(UMLS entries(path)), where path means both edge and node. Padding has to be applied if the sink has been found earlier for that the last edge and node are repeated.
### Training
Binary cross-entropy loss with with p(y_i=True|&eta;,x_i)= sigmoid(&eta;x_i) and regularization on the length of &eta; 

score(candidate)=1/n SUM(p(y_i=True|X(&pi;_i);&theta;)) over all possible paths of length 2 to *l*.
## Baselines
Both baselines base on random walks, where the next edge to take is chosen with chance 1/(deg(*node*)).
## Test results
*l* is set to 4, *K* is set to 133 and *M* is 52.

10-fold CV and drug rediscovery was tried. Ridge regression works better than Lasso, but both work best in CV if regression is off.
More data is better. Bigger *l* means more training data, *l* over 4 it would take much longer and (probably) get more false positives, but unfortunately not tested. 

SemKG lists all known medicines for diseases in at most four steps. SemaTyP uses SemKG and gives a better ranking than previous solutions on the same graph.
## Problems
Open information extraction leaves to be desired. Further, all nodes and relations have to be in UMLS, which is not complete compared to the task.