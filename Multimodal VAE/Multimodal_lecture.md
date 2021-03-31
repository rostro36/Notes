# [Carnegie Mellon Course 11-777 Multimodal Learning]
- [Recordings](https://www.youtube.com/channel/UCqlHIJTGYhiwQpNuPU5e2gg)
- [Webpage](https://cmu-multicomp-lab.github.io/mmml-course/fall2020/)
# Lecture 1.1 \(introduction/ challenges)
## Definition
- **Modality** &rightarrow; The way in which something happens or is experienced
- **Medium** &rightarrow; A means or instrumentality for storing or communicating information.
## Advent of neural networks
Thanks to:
- good datasets
- faster GPUs
- better visual features/knowledge
- better word embeddings
- (more interest both from researchers and patrons)
## Core challenges
### Core challenges to prepare data
- **Alignment** of two modalities
	- explicit vs. implicit
- **Representations** to better use the modalities
	- exploit complementarity
	- exploit redundancy
	- joint and coordinated representation spaces possible
### Core challenges to do with the data
- **Translation** from one modality to another
- **Fusion** of multiple modalities to infer something novel e.g. classification
	- early fusion, concat different modalities, good for low level features
	- late fusion, pre-process the modalities first, good for high level features
- **Co-Learning**
	- ~transfer learning
# Lecture 1.2 \(datasets/tasks)
## Research tasks:
- Image captions
- Video captioning & "grounding"
- Visual question answering
- Video QA
- Multimodal dialogue
- Large-scale video event retrieval (YouTube search)
- Language, Vision and Navigation
- Self-driving multimodal navigation
## Affective computing
- Find emotions, moods, personality traits, mental health state and feelings
- Rather small datasets as they need experts to annotate
- Best known is the yearly **Audio-visual Emotion Challenge**, where one has to find the emotion of a speaker based on a video clip
- Has an **Alignment** component as one sometimes smiles after saying something, but usually not as important as the **Fusion** component
## Media captioning
- MS COCO dataset, evaluated by mechanical turkers
- Human labels are gold standard, but automatic benchmarks are built as well like BLEU and ROUGE, but they are not as good as one would wish
- **Grounding** means finding one part of one modalitiy (e.g. a word) and find it in another modality (e.g. a part of an image) 
- mostly a **Translation** task
## Question answering
- VQA dataset
- Decent at Yes/No questions, but problems with counting questions.
- The verbal description only adds about a 14% increase.
- Sometimes the model has learnt some common sense instead of perception. E.g. "What colour is the banana?"
- In many parts a **Translation** task, but also has a **Fusion** component
## Multimodal navigation
- Take vocal instructions and apply them in a video-feed of the navigator
- Room-2-Room, given a written instruction, navigate a screen like an old-puzzle game to get to the goal
- **Representation** is the main challenge
# Lecture 2.X \(recap of \[unimodal] machine learning) 
Nearest neighbour needs a distance metric, what may not be obvious in a multimodal task.

*Score* is the score/probability sample to (any) class, *loss* is the cost function of the scores to the ground truth/objective.

The objective is to minimise the loss.

1. First step is to overfit on the data, so that you can be sure that the model is complex enough (and everything is wired correctly together).
1. Add regularisation, so that it generalises better.

# Lecture 3.X \(image and language representation)
Most visual representation is **object-based**, where an image is a list of objects, which are each represented by a feature vector.

CNNs are translation invariant.

Unimodal models should not be used as black boxes, as that would lose some intuition, that could be useful for the multimodal model.

For video, one can have another dimension in CNNs.
# Lecture 4.1 \(Multimodal representations)
## Graphs
A graph consists of:
- set of vertices
- binary adjacency matrix (edges)
- node features (attributes)
- node labels (types)

Graph neural network tasks:
- look on neighbourhood and predict the centre piece
- from a centre piece predict the neighbourhood and the neighbourhood of the neighbourhood

Neighbourhood aggregation:
- average pooling (different weights for neighbours and centre)
- graph convolution network (same weights for centre and neighbourhood, but different normalisation)
- gated graph neural network (the hidden-state of the centre is the output of the average of the neighbouring hidden states and the open value of the centre)

## Multimodal representations

Goals of a good representation:
- Similarity in latent space means similarity in concept space
- usefulness for discriminative tasks
- miss modalities
- fill modalities

A **joint representation** has all modalities in the same space, compared to a **coordinated representation**, where the modalities have different spaces, but can be compared with each other in some meaningful way.

### Unsupervised representation learning
Goals:
1. Extracting interesting information from data:
	- clusterings
	- trends
	- compression
1. learn better representations

### Autoencoder
Self encoding, feed forward network consisting of a encoder and decoder part.

The usual loss is trying to re-find the input. A robuster optimisation is to use noisy inputs to the network and then try and then score the output according to the denoised input.

Can be used to generate something based on the intermediate plane, even if the information up to that plane came from another modality. All the intermediate steps are intermediate, not just the one in the very middle.

Joining the unimodal representations can be done by:
- simple concatenation
- element-wise multiplication or summation
- as a first\/input layer of a multilayer perceptron
	- bilinear pooling \/multimodal tensor fusion network
### Multimodal encoder-decoder
Encoder and decoder are often CNNs for visual modalities and LSTMs/BERT for text.

Multi-view LSTMs have hyperparameters to define if the memory of the own memory should be used or only the memory of the other modalities.

# Lecture 4.2 \(coordinated representation)
## Canonical Correlation Analysis
- canonical &rightarrow; simplest schema possible
1. Find two \(deep) embeddings with maximal correlation
1. We want those embeddings to be orthogonal\/\("canonical"), force the trace of the 
1. Make the objective variance to be the identity 

Training procedure:
1. Pre-train good embeddings using denoising autoencoders
1. Fine-tune the embeddings on CCA
## Multi-view clustering
Multi-view principles
1. Consensus principle: maximise consistency across multiple distinct views
1. Complementarity: multiple views needed to get more comprehensive and accurate descriptions

Multi-view subspace clustering learns a unified feature representation from all the view subspaces by assuming that all views share this representation

## Autoencoder


# Lecture 5.1 \(Multimodal alignment)
**Alignment** &rightarrow; finding relationships and correspondences between modalities

E.g. finding the right frame of a video with a discription

Two types:
- **Explicit** &rightarrow; alignment is the task itself
- **Implicit**  &rightarrow; alignment is part of the problem, but not the main goal


## Dynamic time warping
Constraints:
- monotonicity &rightarrow; no going back in time
- continuity &rightarrow; no gaps
- sentinels &rightarrow; start and end at the same points
- warping window &rightarrow; not too far from diagonal
- slope constraint &rightarrow; do not insert or skip too much

Solve it with a dynamic programm, which searches the best translation matrices (*W*) for the alignment of the two sequences *X*,*Y* and the loss of:
||XW_x-YW_y||^2_F

For multimodal alignment, the alignment will be done in the latent (CCA) space, this can be done in an expectation maximisation kind of optimisation strategy, find the best alignment matrices *W* and then find the best representation *U*.

## Attention
Allows for implicit alignment.

There are three major categories:
- soft attention &rightarrow; acts like a gate function and is deterministic inference
- transform network &rightarrow; warp the input to align with a canonical view
- hard attention &rightarrow; stochastic process, related to reinforcement learning.

### Soft attention
Instead of only using the final state of the encoded sequence, attention uses all the intermediate steps of the encoded sequence e.g. instead of the whole sentence, ever word of the sentence is used.

We make a neural net to tell our neural net where to look.

For images, we have to take the last convolution representation, before going into the fully connected layers, to get some spatial awareness.

### Spatial transform network
Crop and transform an image from one image warp it to some more interesting representation, e.g. crop out, rotate and resize an object.
### Hard attention
Only a yes and no, start with the most important and when the answer is yes, embed it and check the neighbourhood.

You will only have a part of the whole image instead of the whole, like in soft attention.
# Lecture 5.2 \(Self-attention and embeddings)
Contextualise sequences:
- Bi-directional LSTM &rightarrow; hard to be parallelised
- Convolution &rightarrow; problems with long-range dependencies, convolution kernels are static
- Self-attention &rightarrow; can be parallelised, long range dependencies, dynamic kernels, but lots of learnable parameters
## Self-attention (e.g. BERT)

# Multimodal transformer genauer anschauen!!!!!
New attention for each word in input sequence and output sequence.

Every input word will be embedded as a query, key and value.

The query asks the keys of the input words and the attention for that word is then the dot product of the centre query times the key, which weights the value.

Multi-head attention is the same attention, but multiple times with different seed, which then get aggregated.

To add positional information, one can one-hot encode the position and concat it with the input word.

Learning tasks:
- randomly mask input tokens and predict them afterwards
- predict which of the two given sentences is the next sentence

## Sequence to sequence
Seq2seq also uses a latent representation like in self-attention a triple of query,key and value. A typical unimodal problem is translating from English to French.

Key and value are from a "normal" self-attention network, trained on the English input, but the query is from a masked self-attention network, that was trained on the French output. This means we also need it the other way round, from French to English and forces us to 

The decoder then uses the self-attention formula of the dot product of the key and the query for the weighting of the value.

# Lecture 7.1 \(Alignment and Translation)
## Connectionist Temporal Classification \(CTC)
Almost on par to humans, even better than normal deep neural networks.

1. Use a classifier to get a phoneme from speech
1. Generate the probability of the phoneme path
1. Predict labels, remove blanks
1. Output labels

Can be done with forward-backward algorithm.

CTC is much more peaky, as it focuses on transitions \(of the paths), compared to deep neural networks.
## Temporal alignment using neural representations
Assumption: we have paired video sequences that can be temporally aligned

Cycle consistency: Generally, I should be the closest match of my closest match.
1. Embed every frame
1. Find the nearest match
1. Find the nearest match the other way and penalise, if I am not the closest match

Allows for anomaly detection.
## Visual question answering
Answer the written question, by looking at the image. E.g. What is the colour of the bird?

Best is to use **co-attention**:
- attend the image with the found objects in text
- **and**
- attend the text with the found objects in the image.

**Neural Module Network** uses dependency parsing to get the attention for the image and the way how to combine them. 

**Neural Symbolic VQA** parses one input and puts it into a god-given feature space of table form with entries like colour, size etc. If there is a question on those attributes, this performs well, but struggles heavily, if there is a question outside of these attributes.
# Lecture 7.2 \(Generative models)
**Probabilistic graphical model** (PGM) is a graph formalism for modelling joint probability distributions and dependence structures.

## Bayesian networks
Model directed acyclic graphs and conditional dependencies.

Bayesian networks infer structure (a DAG) given external knowledge on the features. This also makes interpretability easier.

This structure gives an easier way to use Bayes rule, given the parents, we try to estimate the child. We do not need more than just the parents.

A **dynamic bayesian network** can change over time and satisfies the Markov rule. Hidden Markov models are one kind of a dynamic bayesian network.

There exist cAE-GANs which use an encoder-decoder to generate images and then a decider to test the generated image vs the real one.

# Lecture 8.X \(Discrimative graphical models)
## \(Restricted) Boltzmann Machines
Undirected graphical model consisting of a hidden layer and a visible layer. The layers are bipartite.

Important graphical models are hidden Markov models and conditional random fields. The embeddings may also have a temporal structure. There are also soft-labels for CRFs.

The reparametrisation trick needs to be used to derive a VAE, this also means that the variational distribution is limited to some distributions.
# Lecture 9.1 \(Reinforcement learning)
## Reinforcement learning
The reinforcement learning framework consists of two parts:
- the agent
	- have a state and perform actions
- the environment
	- give a reward depending on the action, the previous state and the next state of the agent
	
The score that we want to optimise is the sum of the rewards with some parameter &gamma;
G_t=&Lambda;&gamma;^{timestep}Reward_{timestep}

&gamma; acts as a hyperparameter, which makes rewards more near- or far-sighted.

A **policy** is a distribution over actions given states:
- policies fully define the behaviour of an agent
- policies are time-independent
- policies can\(and should) change during learning

RL is good to learn long \(action) sequences with a goal in the end, where one can not just check every intermediate step.

The **state-value function** is the expected return starting from a state *s* following policy &pi;.
The **action-value function** is the expected return starting from a state *s*, making action *a* and from then on, following policy &pi;

# Bellman optimality
# write out Bellman expectation 

Solve it by iterating over the two steps until convergence:
1. policy evaluation
	- define the new state-value function, based on the latest policy
1. policy improvement
	- update the policy based on the new state-value function

Value iteration only one step each:
1. state-value-function evaluation
	- use the Bellman expectation with equality until convergence
1. policy generation
	- generate policies based on the state-value function from before
	
## Limitations
- Use lots of storage and time for all states and actions
- know all the states, actions and transitions

# Tabular Q-learning
Not gradient based, &alpha; is a learning rate parameter

## Epsilon greedy
With probability 1-&epsilon; take the best action, with probability &epsilon; take a random action. Like this, one can help the best path, but still end up in new states, which helps for robustness and exploration.

Hard to find a good &epsilon;, also it can decrease, the longer you are learning. Still needs a lot resources, but can do for unknown transitions

Deep Q-learning instead of learning the whole Q(s,a), learn a deep neural net, that takes as input any s and output the best possible action. Can learn all the states and is slimmer.

Very hard to train, as the samples are often correlated
- build data set from agents experience and sample a random mini batch from them 
and targets not stationary (time-dependent)

# Lecture 9.2
Policy based, learn policy directly, works well with continuous actions, but not good for off-policies and exploration.
Policy gradients algorithm with 4 steps.
Reward function J

REINFORCE algorithm
- High variance, because credit assignment is really hard.

Reinforce trick to go to log

# Lecture 10.1 \(Multimodal Fusion \& Co-Learning)
## Model-free fusion
Early fusion &rightarrow; concat all modalities and let the classifier do the fusion, which is easier to implement, problematic with different granularities
Late fusion &rightarrow; all modalities first get trained on their own classifier, afterwards there is a fusion mechanism/neural net that combines the output of these classifiers
## Co-Learning
Transfer knowledge between modalities, including their representations and predictive models

strongly paired &rightarrow; for every image there is *this* description

weakly paired &rightarrow; for every image there is *some* description
- First align/pair the data a bit stronger

**Few/zero shot learning** &rightarrow; pre-train a classifier, then give something new \(and only *few*\/no examples) and test if the classifier works on that as well

**Weak supervision** &rightarrow; don't pinpoint two things together, e.g. just give a sentence and a picture instead of pointing out the individual words in the pictures
	- to solve use **contrastive learning**
# Lecture 10.2 \(Research trends\)
## Abstraction and logic
Neural state machine &rightarrow; generate a probabilistic scene graph. Nodes are entities, which have attributes \(with probabilities\), edges are \(directed\) relations.

There are more complex questions, which also need some logic to answer and can be deconstructed.
## Causality
Sometimes, less information gives better results, as more information allows for some bias to the additional bias.

Hidden confounder &rightarrow; even though it might appear that *B* causes *C*, but there is another factor *A* that causes both *B* and *C*, *A* could be only latent and not observed.

To study bias, one can change the image\/text \(like adding\/removing some objects\) and ask the same question again to see if the answer changed.
## Understanding multimodal models
Ask a main question and sub-questions, which should lead to the same answer. E.g. "is the banana ripe?", "what is the colour of the banana?"

This leads the models to not use short-cuts, that humans do\/should not use.

More modalities mean more complexity, which means a higher chance of overfitting, to resolve this compute and control the *overfitting-to-generalisation ratio*. The other issue is that the different modalities may have different difficulties and need different learning rates, which can be unimodally reweighed.