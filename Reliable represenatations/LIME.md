# [“Why Should I Trust You?”: Explaining the Predictions of Any Classifier](https://arxiv.org/abs/1602.04938)
## Explanations overview
Explanations can be used to spot data leakage, if it is carried out by a realistically bad feature, that is treated very importantly.

Features may not be interpretable e.g. NLP embeddings. An explanation is given by features or tokens which carry the highest weight.

A good explainer should be:
- **interpretable**&rightarrow; as long as it is possible by the problem and the data, the explanation should be easy to understand for the target audience
- **preserving local fidelity**&rightarrow; the explanation should be faithful to the model, at least around the inspected area. Global explanations are still more complicated
- **model-agnostic**&rightarrow; the explainer should work with any model, no matter if it already exists and what it's architecture is based on
- **give a global perspective**&rightarrow; such that a user can get a feeling for the whole model and not just one or two predictions

An explanation model acts over the presence/absence of interpretable components. Therefore there is some inherent bias when using a particular model architecture, e.g. an explainer that is based on a CNN will bias for pixel windows.

## LIME
**LIME** stands for:
- **L** &rightarrow; locally
- **I** &rightarrow; interpretable
- **M** &rightarrow; model-agnostic
- **E** &rightarrow; explanation

The lime loss function is:
$$\xi(x) = \argmin_{g\in G}L(f,g,\pi_x)+\Omega(g) $$
With:
- *x* &rightarrow; a data point 
- *g* &rightarrow; an interpretable model
- *L* &rightarrow; the fidelity (loss) function of the underlying problem
- f &rightarrow; the decision function
- &pi; &rightarrow; the distance function to weigh nearby points
- &Omega; &rightarrow; complexity

E.g. with *L* being the squared loss:
$$L(f,g,\pi_x)=\sum_{z,z'\in Z}\pi_x(z)(f(z)-g(z'))^2 $$
Where *Z* is the set of samples and *z'* is an interpretable version of the sample *z*, such as a smaller dimensional vector or super-pixels instead of pixels.

This algorithm alone gives a model, that is *interpretable*, *preserves local fidelity* and is *model-agnostic*, but for a *global perspective* an additional trick is used.
## Submodular pick (SP) algorithm
To get a global view, we pick many *x*. The different *x* should be dissimilar to each other and represent many different components. The paper suggest the greedy *Submodular Pick* algorithm.
1. compute explanations *W* for all *x*
1. compute expressiveness of *W* e.g. by the amount of explainable components
1. greedily add explanations from *W* to get a pre-determined expressiveness or occurrence of components in the explanation/data point set

## Experiments
There are two kinds of experiments, simulated ones and with human subjects:
- For the simulation, already interpretable models were used. In a NLP sentiment analysis experiment, LIME found the gold standard of the interpretable models quite well. This is a good sign, that the model agnostic LIME will also work well with less interpretable models.
- It is also simulated if the model changes the prediction when adding or excluding untrustworthy features and LIME performs better than similar models.
- Real user experiments with Amazon Mechanical Turk in NLP sentiment analysis show that explanations are important to find a reliable system and SP improves accuracy.
- It is also shown, that with rather little effort a user can select which features are good and which are worse.