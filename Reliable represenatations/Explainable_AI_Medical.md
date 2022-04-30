# [What do we need to build explainable AI systems for the medical domain?](https://arxiv.org/abs/1712.09923)
## Overview
Explainability is important not only to test models properly and for adoption in the direct domain, but also to comply with new government rules and standards such as GDPR and ISO.

For decision support it is important to understand the **causality** of learned representations to create good explanations.
### Normal machine learning procedure
1. Learn from prior data (*training*)
1. extract knowledge (*testing*)
1. generalize (*control overfitting*)
1. fight curse of dimensionality (*tackle overfitting and lay base for explainability*)
1. disentangle the underlying explainability factors (*explainability*)
But where is data context?

### Dissecting understanding/interpretation/explanation 
- **Understanding** &rightarrow; functional understanding &#8792; don't open black-box
- **interpretation** &rightarrow; mapping of abstract concepts to human comprehensible concepts
- **explanation** &rightarrow; important features for prediction
## Prototype and Expert
Prototype for class *c*
$$ x^*=\max_x log(P(\omega_c|x)) - \lambda ||x||^2 $$
- *P* &rightarrow; neural network output
- &omega;*_c* &rightarrow; weights for class *c*
- &lambda; &rightarrow; regularisation parameter

Expert for class *c* also includes probability of *x*
$$ x^*=\max_x log(P(\omega_c|x)) +log(P(x)) $$ 
For restricted Boltzmann machines:
$$log(P(x))=\sum_j f_j(x)-\frac{1}{2}x^T \Sigma^{-1}x+c$$
$$f_j(x)=log(1+exp(w_j^Tx+b_j))$$
With
- *w*_j &rightarrow; weights for component *j*
- *b*_j &rightarrow; offset for component *j*
## Examples of explainable model architectures
Deconvolutional network &#8792;convolute from output back to patch

One example of making deep models more transparent is training two models:
1. produce image captions
1. use image captions to produce explanation

Medical images can use AM-FM decomposition as input, which is especially useful for textures
$$AM-FM(x_1,x_2)=\sum_{l+1}^LA_l(x_1,x_2) cos(\gamma_l(x_1,x_2)) $$
- *A_l* &rightarrow; amplitude
- *&gamma;_l* &rightarrow; phase

Another alternative is to use higher tiers of ResNet fo enable more explainability as opposed using the lowest tier.

*-omics data needs heterogenous data from different sources in a unified view for the future.

Human-in-the-loop can be beneficial for hybrid distributional models