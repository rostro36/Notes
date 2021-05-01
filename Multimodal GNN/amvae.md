# [Multimodal Network Embedding via Attention based Multi-view Variational Autoencoder](https://dl.acm.org/doi/abs/10.1145/3206025.3206035)
## Introduction
Multiple datatype and multimodal graphs, such as Facebook or Twitter are the start of this paper. This paper uses the fact that linked nodes have a higher chance of being similar.

This paper introduces the Attention based Multi-view Variational Autoencoder \(AMVAE\), which incorporates link information and multimodal contents for embedding learning.
## Architecture
Each node consists of two parts\/ views: the visual-textual\/feature part and the network\/graph structure view, which are combined in the multi-view VAE:
### Visual-Textual embedding
The visual textual embedding is based on a RNN with attention based on the image. The final state, after the text, is the embedding used for the node. To train, a siamese learning approach is used, which uses negative sampling on the same image and tries to maximise the margin between the correct text and the negatively sampled text.
### Network view
The network view is generated using DeepWalk, I think
### Multi-View VAE for embedding
The two views, the content view and the network view, can be combined to an autoencoder in three modes:
- *separate mode* &rightarrow; both views are modelled as their separate VAEs
	- this does not use the assumption that links have also meaning for the visual-textual embedding
- *connective mode* &rightarrow; concatenated and encoded together, then sampled on their own and then decoded into a concatenated representation again
	- this model favours the longer vector of the two views
- *mixed mode* &rightarrow; both get separately encoded  first, then encoded in a shared hidden state, from this hidden state they get sampled and decoded separately then

The loss consists of the margin loss from the visual-textual embedding added to the VAE loss of the sum of the two views. Shrinkage on the weights is also another loss term, which each get their scalar weight. This means both parts, the visual-textual embedding as well as the \(separate\) autoencoders get learnt jointly. 
## Experiments
The datasets are Flickr posts, which consist of an image and a title. There are four possible reasons for edges, which each add 1 to the weight of the edge:
- same user
- same tag
- same location
- same group

*AMVAE mixed* performs best on node classification and link prediction, even more so if the network is sparse \(as many other baselines only consider the network view\).

The attention in the visual-textual embedding works as expected.