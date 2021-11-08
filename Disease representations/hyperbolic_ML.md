# [Hyperbolic neural networks](https://arxiv.org/abs/1805.09112)
## Introduction
Euclidean spaces are used over most of machine learning due to it's simplicity and easier understanding, as this is how science is taught in high school and understood by everyday people.
Ditching euclidean spaces and using a hyperbolic room therefore has a reason; this being that exponentially growing trees map nicely to this space and give a more expressive embedding in the same amount of dimensions.
As said before, the common machine learning operations are not as straight forward as in the Euclidean space and they are shown in this paper together with some benchmarks on how the precision changes from normal Euclidean operations to hyperbolic operations.
## Hyperbolic space
### Riemannian geometry
A Riemannian metric *g* is a collection of inner-products, varying smoothly with *x*. A Riemannian manifold is a manifold *m* with a *g*. Even though *g* seems to be only locally, it induces global distances *d* by integrating the length of shortest path between two points.
$$ d(x,y) =\inf_\gamma\int_0^1 \sqrt{g_{\gamma(t)}(\dot{\gamma}(t),\dot{\gamma}(t))}dt $$
Where &delta; is a *geodesic*, a shortest path, where at 0 and 1 one of the two endpoints are.
### Poincaré ball
The hyperbolic space has five convenient models, the one chosen is the Poincaré ball model and it is defined by the Riemannian metric:
$$ g_x=\lambda_x^2g^E, \lambda_x=\frac{2}{1-||x||}, g^E=I_n, ||x||<1 $$
With *I* being the Euclidean metric tensor.
The induced distance between two points is then:
$$ d(x,y)=cosh^{-1}(1+2\frac{||x-y||^2}{(1-||x||^2)(1-||y||^2)}) $$
And the angles are:
$$ cos(\angle(u,v))=\frac{g_x(u,v)}{\sqrt{g_x(u,u)}\sqrt{g_x(v,v)}}=\frac{\langle u,v\rangle} {||u|| ||v||} $$
### Gyrovector spaces
To get an elegant non-associative algebraic formalism for hyperbolic operations such as addition or matrix multiplication, we use gyrovector spaces, which are described by an additional hyperparameter *c* for curvature. When *c* is 0, then it is just the Euclidean ball and if *c* is 1, then the Poincaré ball is recovered as the ball is given by
$$ |c| ||x||^2<1 $$
And this allows us to define a non-commutative nor associative (Möbius) addition and a (Möbius) matrix multiplication and a distance *d*:
$$ x\oplus_c y =\frac{(1+2c\langle x, y\rangle +c||y||^2)x+(1-c||x||^2)y}{1+2c\langle x,y\rangle+c^2||x||^2||y||^2}, M\otimes^c(x)=\frac{1}{\sqrt{c}}tanh(\frac{||Mx||}{||x||}tanh^{-1}(c\sqrt{c}||x||))\frac{Mx}{||Mx||},  d_c(x,y)=(\frac{2}{\sqrt{c}} tanh^{-1}(\sqrt{c}||-x\oplus_c y||) $$
The matrix multiplication is a summary of the three operations:
- logarithm to the origin: $$ log_0^c(y)=tanh^{-1}(\sqrt{c}||y||)\frac{y}{\sqrt{c}||y||} $$
- multiplication at the origin: $$ r \otimes_c x = exp_0^c(rlog_0^c(x)) $$
- exponentiation from the origin: $$ exp^c_0(v)=tanh(\sqrt{c}||v||)\frac{v}{\sqrt{c}||v||} $$
## Hyperbolic machine learning operations
Using these operations in the hyperbolic space, the paper shows the counterparts of some machine learning learning operations in hyperbolic space, such as multiclass logistic regression, linear layers and RNNs.
### Multiclass logistic regression
First, classical multiclass logistic regression has to be reformulated from a hyperplane *h* with normal vector *a* and bias *b*=*<a,p>* in the hyperbolic space, the same hyperplane *h* additionally has another point *p* through which the plane goes:
$$ p(y=k|x)\propto exp((\langle a_k,x\rangle-b_k))=exp(sign(\langle-p_k\oplus x,a_k\rangle)||a_k||d(x,H_{a_k,p_k})) $$
Writing everything out, we have:
$$ p(y=k|x) \propto exp(\frac{\lambda^c_{p_k}||a_k||}{\sqrt{c}}sinh^{-1}(\frac{2\sqrt{c}\langle-p_k\oplus_cx,a_k\rangle}{1-c||-p_k\oplus_cx||^2 ||a_k||})) $$
### Feed-forward layers
For a linear map, we can use the **Möbius multiplication** if Mx is not 0, if Mx is 0, the result is 0 as well.
$$ M\otimes_c x=\frac{1}{\sqrt{c}}tanh(\frac{||Mx||}{||x||}tanh^{-1}(\sqrt{c}||x||))\frac{Mx}{||Mx||} $$
Another part of the feed-forward layer is **bias *b* translation**, which is given by:
$$ x \leftarrow x\oplus_cb=exp_x^c(P_{0\rightarrow x}^c(log_0^c(b)))=exp_x^c(\frac{\lambda_0^c}{\lambda_x^c}log_0^c(b)) $$
The last thing shown in this chapter is **concatenation** of two vectors *x1* and x2*, stacked on top as *x*, which are each multiplied with a matrix *M1*, *M2* concatenated together to:
$$ M\otimes_cx=M_1\otimes_cx_1\oplus_cM_2\otimes x_2 $$
With a Euclidean *y*, the concatenation in the hyperbolic space with some learnable hyperbolic *b*:
$$ M\otimes_cx\oplus_cy\otimes_cb $$
### Recurrent neural network cells
A **normal RNN** can be quite simply be translated to hyperbolic space with:
$$ h_{t+1}=\varphi^{\otimes_c}(W\otimes_ch_t\oplus_cU\otimes_cx_t\oplus_cb) $$
For **GRU**, it is a bit more complicated, as it uses a pointwise product, which also impacts the other gates.
$$ r_t \odot h_{t-1} \Rightarrow diag(r_t) \otimes_ch_{t-1}$$
$$ r_t \Rightarrow \sigma log_0^c(W^r\otimes_ch_{t-1}\oplus_cU^r\otimes_cx_t\otimes b^r)$$
$$ \tilde{h_t} \Rightarrow \varphi^{c}((W diag(r_t))\otimes_ch_{t-1}\oplus_cU\otimes_cx_t\oplus b) $$
$$ h_t\Rightarrow h_{t-1}\oplus_cdiag(z_t)\otimes_c(-h_{t-1}\oplus_c\tilde{h_t}) $$
## Experiments
### Data
Two NLP datasets are used
- SNLI gives two sentences and asks if the second is inferred by the first
- PREFIX tests to find noisy prefixes, so given check whether the second sentence is a prefix of the first or a random, unconnected sentence  
### Model
The sentences are first embedded using two distinct hyperbolic RNNs, then fed into (hyperbolic) feed-forward layers until they go to a (hyperbolic) multiclass regression layer.
### Optimisation
Embeddings and biases are hyperbolic, but weights are Euclidean. The Euclidean parameters are optimised with Adam, while the hyperbolic parameters are updated with Riemannian stochastic gradient descent (RSGD).
For numerical stability, clipping around the tanh vectors between plus and minus 15 was used as well as arguments of arctanh are clipped to at most 1-10^(-5).
Further, if during the optimisation the word embeddings go outside the Poincaré ball, they are projected back.
Some **hyperparameters** also have to be chosen, **c** is set to 1, to recover the Poincaré ball. Batch size is 64, hidden state and embedding dimension is 5. The 500K-600K samples were trained in 30 epochs. Initialisation is considered very important.
### Results
GRU performs better than RNN. Although Euclidean implementations are better researched and have with Adam a better algorithm, the two variants perform mostly similar. The more tree shaped the data, the bigger is the difference between the Euclidean and the hyperbolic implementations. This is also due, because the model has problems to learn representative word embeddings. Using a different model to implement WordNet into the hyperbolic space yields a big performance gain.
Mapping the hyperbolic embedding back into Euclidean coordinates performed worse than directly using the hyperbolic embedding for the euclidean MLR.
