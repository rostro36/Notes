# [Representation Learning for Networks in Biology and Medicine: Advancements, Challenges, and Opportunities](https://arxiv.org/abs/2104.04883)
## Graph types
**Edges** can be:
- simple \(bool\)
- weighted \(int\)
- attributed \(vector\)

**Graphs** can:
- be heterogeneous &rightarrow; having different kind of nodes or relations in one graph
- multilayered &rightarrow; having multiple graphs at "different resolutions"
- dynamic &rightarrow; graphs evolve over time
- spatially sound &rightarrow; nodes have coordinates, which can be measured between two nodes
## Graph tasks
**Classification**:
- Node classification &rightarrow; predict node label
- Link prediction &rightarrow; predict if link\/relation is missing
- Edge classification &rightarrow; predict the label of a node
- Subgraph classification &rightarrow; predict a label on a subset of nodes
- Graph classification &rightarrow; predict a label on the whole graph

**Detection**:
- Module detection, detect a subgraph that influences a variable
- Community detection, detect if a set of nodes can be clustered together

**Generation**:
- generate a latent graph using a bigger graph for better classification
- generate a lower dimensional latent graph
- generate a the topology of a complicated graph
- generate a graph with some properties, like being a tree or being a valid protein
## Graph machine learning paradigms
Different **abstraction levels** for theoretic techniques and statistics:
- node-level &rightarrow; bottleneck, position
- link\/relation-level &rightarrow; common neighbour index, L3
- subgraph-level &rightarrow; motifs, like in the Weisfeiler-Lehman kernel

**Network diffusion** &rightarrow; check how much a subgraph influenced another part of the graph.

**Topological data analysis** &rightarrow; summarise a graph to a vector with *persistent homology* or an underlying graph with a *Mapper*

**Manifold learning** &rightarrow; maps a higher order node into a lower order space, both for the features as well as the edges\/relations

**Shallow network embeddings** &rightarrow; embeds nodes so that the similarity in the latent space approximates similarity in the graph space
## Applications
| **Task** | **Goal** | **Data** | **Methods** | **Problems** |
|-|-|-|-|-|
| generating protein structure | generate valid proteins | amino acid sequences | generate edges based on distance generate 3D fingerprints | have to adhere validity |
| characterising protein interactions | fill blanks of PPI network | protein-protein interactions (PPI) | embed both proteins and then predict link construct knowledge graph with NLP on PubMed |   very noisy |
| predicting protein phenotypes | identify protein functions | gene ontology, structural data of proteins, PPI | hierarchical graph on GO terms  Transformers |  |
| cell type aware protein representation learning | predict if a gene is expressed by a sequence (node label prediction) | PPI | protein embeddings | different cells have different embeddings |
| integrating gene expression data | identify disease specific markers to classify diseases | gene expression data often as matrix, PPI | condgen (GCN,VAE,GAN) | gene expression data can be noisy and variational PPI does not have all the information needed |
| leveraging non-coding elements | add non-coding information to methods | interaction networks between coding and non-coding RNA snippets (IncRNA, miRNA and diseases) | node2vec or DeepWalk for node embeddings | non-coding snippets are long experiments are non-complete |
| fusing genome-wide data | improve disease classification models by fusing coding and non-coding elements | coding elements and non-coding elements in as two knowledge graphs | fusing the two knowledge graphs |  |
| disease classification using subgraphs | form a subgraph consisting of the symptoms of a patient to predict his disease | human phenotype ontology (HPO) | subGNN |  |
| generating drug structure | generate valid drug structures | nodes as molecules, edges as bonds | use edge features, JT-VAE | valid drug |
| characterising drug interactions | regression on interaction edges | nodes as compounds, edges indicate similarity chemical structures and side effects as nodes | TDA, node2vec | labour and cost intensive experiments to test |
| quantifying drug efficacy | regression on drug efficacy | gene expression data, gene ontology, drug similarity medical subject headings (MeSH),PPI | TDA, DeepWalk | combinational explosion |
| cell-line specific prediction of drug pairs | edge regression, where edge means interaction | PPI, protein-drug interactions, drug-drug interactions | transfer learning | drug effects on the body are not uniform |
| characterising diseases through medical imaging | construct graphs from images to do classification | cell graphs from MRI | cgexplainer | interpretability is crucial |
| personalising medical knowledge networks | edge regression | medical knowledge graph, EHR | predict on hierarchical knowledge graph via GAT,word2vec or LINE RNN for a sequence of drugs/diseases mixed pooling multi-view self-attention autoencoder |  |
| integrating electronic health records (EHR) for personalised medicine | edge regression on patients to new disease or treatments | bioentities (e.g. treatment), patient as meta-node | add a patient as a meta-node |  |

## Perspectives
- Causality to find out why a disease acts the way it does
- Classifying diseases at single-cell resolution
- Diagnosing and treating patients through precision medicine
- Track down corona spreaders with Graphs
- Fairness and unbiasedness