# [HyperText: Endowing FastText with Hyperbolic Geometry](https://arxiv.org/abs/2010.16143)
## Introduction
The well known embedding FastText is generally very fast and still pretty good. But FastText does not take word hierarchies into consideration like the ones given by WordNet.

This paper tries to account this hierarchy to produce HyperText, which has less parameters, but takes longer to compute.
## Method
### Poincaré embedding
All words and n-grams are embedded into the Poincaré ball with Euclidean norm \<1 is chosen and the Riemannian metric tensor:
$$ g_x=\lambda^2_xI_d $$
with $$ \lambda_x=\frac{2}{1-\Vert x\Vert^2} $$
### Einstein midpoint Pooling Layer
Like in [hyperbolic attention networks](https://arxiv.org/abs/1805.09786), to aggregate the inputs, the Einstein midpoint is used:
$$ m_K=\frac{\sum_{i=1}^k\gamma_ix_i}{\sum_{i=1}^k\gamma_i} $$
, with the Lorentz factor $$ \gamma_i=\frac{1}{\sqrt{1-\Vert x_i \Vert^2}} $$
But for that, the Poincaré embedding has to be transitioned to the Klein model.
### Möbius linear layer
Instead of the classical linear layer of a matrix multiplication, a [Möbius linear layer](https://arxiv.org/abs/1805.09112) is used before a standard softmax.
### Training
For optimisation the Riemannian (*R*) gradients are used instead of the Euclidean (*E*):
$$\nabla_R f(x)=\frac{1}{1-\lambda_x^2}\nabla_E f(x) $$
Standard cross-entropy loss is used.
## Experiments
The paper uses the same datasets as FastText before.

FastText performs better, the more labels there are and generally is better than FastText and on par with the resource hungry VDCNN and also slightly better than BERT derivatives.

HyperText has less parameters, but needs more computation time.

Using Euclidean counterparts for the Hyperbolic layers lowers accuracy noticeably, but still gives valid results.
