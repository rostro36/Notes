# [Integrating biomedical research and electronic health records to create knowledge-based biologically meaningful machine-readable embeddings](https://www.nature.com/articles/s41467-019-11069-0)
## Introduction
To base electronic health records \(EHR\) with more medical knowledge, the disease and drugs should be incorporated in a knowledge graph. This paper does so by embedding EHR into SPOKE, which consists of the following edges:
- Disease &hArr;Gene
- Disease &hArr;Disease
- Compound &hArr;Gene
- Compound &hArr;Compound
## Method
The main embedding strategy is using a modified PageRank version to create Propagated SPOKE Entry Vectors \(PSEVs\) for each node\/entry in SPOKE.

The information of the EHR only encompass around 7.5\% of all SPOKE nodes, to get the rest of them random walks were used.