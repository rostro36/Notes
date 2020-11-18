# [GraphWriter](https://arxiv.org/pdf/1904.02342.pdf)
## General
The goal of GraphWriter is to generate coherent multi-sentence texts from the output of information extraction systems and in this paper mainly for knowledge graphs.

GraphWriter is a graph-to-text trainable system, which adds the encoder-decoder framework to the ideas of [graph attention networks](https://arxiv.org/pdf/1710.10903.pdf).

Writing understandable English is an overall well researched problem, but staying on the same line and factor into domain knowledge is a open problem.

Table-based inputs were tried before, but are too limiting. Information Extraction Systems that output graphs on the other hand are much more expressive and have been shown to be able to give good enough annotations, but also erroneous annotations, structural variety and destruction of basic grammatical structures (such as predicate-argument).

To extract entity, co-reference and relation annotations a SOTA IE system was used that should be able to collapse co-referential entities. (*The used OIE system is specialized in Scientific Writing*, but the same researchers have made a [new project for general graphs](https://github.com/dwadden/dygiepp) or [here](https://github.com/gkiril/oie-resources) are more information extraction resources)

## AGENDA dataset
The problem they solve is:
Given the title of a scientific article and the knowledge graph (constructed by an IE system), construct an abstract that is appropriate and expresses the content of the knowledge graph in natural language text.

As dataset of that problem is AGENDA, which consists of 40'000 knowledge graphs paired with their scientific abstracts taken from the Semantic Scholar Corpus taken from 12 AI conferences.

To generate the knowledge graph, first [SciIE](https://www.aclweb.org/anthology/D18-1360.pdf) provides NER with four named and one general entity, further provides co-reference annotations and seven relations (e.g. "Used-for").

In the second step, these co-reference annotations were formed into knowledge graphs, where the co-referential entities were collapsed to a single node (the one with the longest mention) and relations were treated as labelled edges.

The dataset was split into 38'720 training, 1'000 validation and 1'000 test data-points.

## Model overview
Input to GraphWriter is a title and a knowledge graph, which then get encoded with a bidirectional RNN and a Graph Transformer architecture the output will be a weight vector either copied from an entity in the knowledge graph of from the decoder's vocabulary.

The objective is to minimize the negative likelihood of the mixed copy and vocabulary probability distribution and the human authored texts. 
### Encoder
The input for GraphWriter is a unlabelled, connected graph.

Each labaled input edge is replaced with one vertex for the forward direction and a second vertex for the reverse direction of the direction. These relation vertices are then connected to the entity vertices, which then results in a bipartite graph; one party being relations, the other party being entities. Further, a global entity node is added, which is connected to all entities and serves as starting point for the decoder.

The transformer starts with self-attention of local neighbourhoods of vertices, in contrast to GAT, there is a more global view and parameters possible.
![Graph Transformer architecture](../images/GraphWriter.PNG)

Entities vectors are encoded in BiRNNs. And are read in the Graph Attention phase that outputs a concatenation of the N attention heads, that compute the weighted sum of softmax(main-word-weights*embedding(main-word),context-word-weights*embedding(context-word)). The second phase is a feedforward network with normalization before and after.
These two steps form a block that gets repeated L times (with the same weights).

The decoding to a context looks similar, but instead of the current word, the hiddenstate gets used.

The output probability then is computed with a softmax(concat(hiddenstate,context), word in input)*sigmoid(hiddenstate,context)-(sigmoid(hiddenstate,context)-1)*softmax(concat(hiddenstate,context), word in vocabulary)
## Experiments
GraphWriter was compared to the real abstracts and some generated by the graph attention network or a text generator with the title as input, which then human evaluators had to decide for one in in order of grammar (well-formed, "natural" English?), coherence (formed like an abstract?) and informativeness (on topic, uses appropriate words?). Best-Worst Scaling was used. 

Additionally BLEU and METEOR were also used to check on the grammar.

## Training
SGD with momentum and a cyclic regiment that reduces the learning rate from 0.25 to 0.05 over the course of 5 epochs and resets the following epoch for 15 epochs with early stopping based on validation loss in total. LSTMs were used as recurrent networks and dropout of 0.3 in the self attention layers.

The system has 500 fixed hidden states and embedding dimensions and attention learns 500 dimensional projections. In Block layers the feed-forward network has an intermediate size of 2'000 and PReLU was used for the six layers and four attention heads. Uncommon words (less than five times seen) are replaced with "<unk>".

The decoding was done with a beam search of size four. In post-processing repeated sentences and repeated coordinated clauses were deleted.

Looking at the results, one can see, that more information gives better performance. GraphWriter is more often the best of real, only-title and GraphWriter and less often the worst.

40% of entities do no not appear in the generated text. 18% of generated sentences partially repeat themselves.