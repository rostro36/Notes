# Causal Inference Seminar - Causality for machine learning
## Introduction
Sometimes models don't distinguish on what they should model due to bad data collection. e.g. tanks shot at night are Russian

In the Kepler dataset signal is rather small compared to the noise
- disentangle noise from signal
## Basics and notation
- DAG with random variables as nodes, direct causations as edges, U=noise, PA=Parent
- DAG causal/topological ordering, not strict
- Entanglement
	- Disentangled (causal) factorization:
		- independent noise: p(X_1,...X_n)=pi^n_i=1(p(X_i|PA_i)
		- independent mechanism: changing one p(X_i|PA_i) does not change the other p(X_j|PA_j); they remain invariant implies intervenability, obviously if X_i is in PA_j, we can not do anything, since PA_j is given.
		
		- With no parents, disentanglement --> statistical independence
		- the causal factors will not be statistically independent and independence-based methods struggle to find them.
	- Entangled (non-causal) factorization
		- p(X_1,...X_n)=pi^n_i=1 p(X_i|X_i+1,...X_n)
			- changing one p(X_i|PA_i) often changes lots of the p(X_i)


- Mechanisms could be inverting of colour
- Sparse Causal mechanism shift hypothesis: a change in a distribution always comes from a sparse change in mechanisms. This can be used to check a model.
	- The shifts can be active (e.g. distribution drift) or active(intervention, action).
- Causal generative process is composed of autonomous modules that do not inform or influence each other.

- Half-sibling entanglement
	- Two stars don't interfere with each other, but share the same noise (This is true if the stars are a certain amount apart as camera can oversaturate)
	- Assume additive random Noise f(N)
	- Assume total information, meaning we can get the f(N) from X.
		- We can find Q up to a certain c, that gets added
	If no total information, hope for negligible influence of R, either it is really low or jointly independent and cancel out.
		- E[(^Q-(Q-E[Q]))^2]=E[Var[f(N|X)]]
	^Q=Q-E[Q]=Y-E[Y|X]


## Problems
### Towards causal representational learning
- Problem: SCMs are typically at the 'symbolic' level, they assume the causal variables are given
- Goal: embed an SCM into a a DL model, whose inputs and outputs are high-dimensional and unstructured
	- given a X construct sparse causal variables S and mechanisms S=f_i(PA_i,U_i), such that we get a disentangled representation on these causal variables
- Observation: reparametrization trick used by SCMs and deep generative models.
- Idea: use SCMs for the problem
### Disentangle noise
- Problem: signal and noise are of the same magnitude
- Goal: find a method to disentangle the noise from the signal to allow better predictions
- Observation: the noise is shared across many datapoints, both in time and spatial
- Idea: Use half-sibling regression on the datapoints
## Methods
### Experts
- Have M different experts, while training, the one that gives the best score for one training sample(that has been tampered with in one mechanism) gets it as training. The others ignore that sample.
- After enough training, each expert is specialized on some (hopefully sparse -> disentangled) mechanism.

### Causal autoencoder
Step 1: anticausal encoder from X to latent representation
Step 2: structural/causal model
Step 3: causal decoder to identity

step 1 and step 2 might be known.


### Hybrid Sampling with auto-encoders
clamp a subset of the U_i and S_i to fixed values and compute all other values according to the network (DROPOUT?)

This lets us generate samples outside of the training set, even for models without a sampling distribution

Interventions on U_i make no sense for models where U_i are independent by design, interventions on S_i make sense.

### Causal training of auto-encoders
Counterfactual training: require that the above sampling still produces valid images after reconstruction, assess using a discriminator.

	- For interventions ons U_i, this encourages statistical independence. (standard disentanglement) "Drop-in"/hybridize borrow features from other samples chosen uniformly random for a hybrid sample, which are then judged by the estimator 

	- For interventions on S_i, this encourages the mechanisms f_i to be independently manipulable (**ICM Principle**) hybridize on S_i

Structural training embed the SCM structure of f into the decoder architecture, feed different levels of the causal modelling on different levels of the decoder, let algorithm choose, which ones it likes most.

structural decoder learns to distinguish and order mechanism from more abstract higher level features to the low level ones.

Sparse causal shift training: require that across domain shifts or actions/interventions of the network, only a sparse set of S_i changes

## Utilizations
### Object centric representations
an image consists of multiple different objects, where each object has an own position, but share the lighting, but influence the image together.

training data is images and now find factorization.
### Transit photometry
Two stars don't interfere with each other, but share the same noise (This is true if the stars are a certain amount apart as camera can oversaturate)

Y can explain a previous or future version of itself e.g. from/to 1 day

Found a habitable planet

### Direct imaging
Get a picture of the sun and filter out the sun
Derotate earth's rotation  (angular differential imaging)

don't show explainable effects/pixels and focus on unexplainable pixels

Half sibling regression performs significantly better than the previously used PCA and is even better with additional domain knowledge about the camera
## Outlook/ open questions
### Towards causal world models
learn an interventional multi-task/environment model
- multi-task in multi environments
- re-usable components, that are robust across tasks
- define objects by transformations
### Half-siblings
- ???
digital revolution might be similar to industrial revolution, but instead of energy, generate/convert/process information with the help of machine learning