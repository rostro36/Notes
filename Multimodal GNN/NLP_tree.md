# [Neural Machine Translation with Source-Side Latent Graph Parsing](https://arxiv.org/pdf/1702.02265.pdf)

## Introduction
The paper tackles the task of translating between different natural languages. The previous methods treated sentences as a simple sequence of words. This paper instead generates a latent dependency-like tree and does the translation there.
## Latent Graph Parser
The parser parses a sentence into a latent, dependency-like tree, that is a set of dependency tuples \(*w\_i*,p\(*H\_i*\|*w\_i*\),p\(*l\_i*\|*w\_i*\)\) consisting of:
- *w\_i*
	- The word at position *i*
- p(H\_i*\|*w\_i*\)
	- The probability, that another token *H_i* inside the sentence is the parent of the dependency of *w\_i*
- p(l\_i*\|*w\_i*\)
	- The probability, that the dependency\/edge type from *w\_i* to *H\_i* is of type *l\_i*

The End-of-sequence tag works as root of the whole sentence.
### POS tagging layer
To do the POS tagging, a bi-directional LSTM is used, where the hidden state consists of a forward state and a backward state. The combined hidden state then gets evaluated with a softmax classifier to predict the tag probability.
### Dependency parsing layer
For the dependency parsing, the POS tag probability from the previous layer and the word representation are again input in a similar bi-directional LSTM architecture as before. The chance of another word being the parent are computed with a softmax, that uses the hidden states of this layer and a newly learnt weight matrix as a scoring function.

## NMT with Latent Graph Parser
Attention-based NMT is used. To speed up training, BlackOut sampling is used.

The **encoder** is a uni-directional LSTM, that takes as input the previous hidden state, the word embedding and the dependency parsing hidden state. Afterwards, this hidden state together with the according to their probability weighted hidden state of the parents and the label probability is fed into a MLP, that then outputs a dependency composition function.

The **decoder** is a LSTM, that uses attention on the encoded representation and also uses the hidden state of the decoder. The decoder then produces a hidden state, that gets fed into a softmax classifier to produce word probabilities.

Instead of a classical **beam search**, the length of the translation, given the original length is also considered.

## Experiments
The Asian Scientific Paper Excerpt Corpus (AS-PEC) corpus was used for English-to-Japanese. If the word was used less than 2\-3 times, the word was replaced with \<UNK\>. BLEU, RIBES and perplexity was used to score and also as training objective.

There is a Wall Street Journal training data set for POS tags, to pre-train the latent graph parser. In the case of a small dataset, the performance of simpler models is better, but with pre-training the POS-tagging and latent graph parser, the performance is better than simpler models. The positive effect of pre-training persists also in the medium and large training set.

This system works better with long-distance dependencies. The adapted latent graph is generally not a graph that is exactly as the dependency parsing tree, as it was optimised for translation.