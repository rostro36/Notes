# [Auto-Encoding Variational Bayes](https://arxiv.org/pdf/1312.6114.pdf)
## Introduction
We assume the data is generated using some hidden latent variable *z* (e.g. class), that adheres to some **prior** p_{&theta;\*}(*z*) and then a process, which adheres to some **conditional distribution** p_{&theta;\*}(*x*|*z*).

Since we do not know *z*, we have to check for all the possible *z*s and since we do not know &theta;\*, we have to learn some parameters via MAP or ML.

We allow the integral of the **marginal likelihood** p\_\{&theta;\}\(*x*\)=integral\(p\_&theta;\(*z*\)p\_&theta;\(*x*|*z*\)d*z*\) to be intractable, like neural nets often are, which means that we can't use an EM algorithm. Also the algorithm should work on a large dataset.

That is quite hard to do on the exact distributions, that is why approximates for &theta;, p\_&theta;\(*x*|*z*\)(**decoder**) and q\_&phi;\(*z*|*x*\)(**encoder**) are used, which are more efficient.
## Reparametrization
We assume that *z* \(&approx; q\_&phi;\(*z*|*x*\)\) = g\_&phi;\(&epsilon;,*x*\), where the **auxiliary variable** &epsilon; brings in the randomness.

We also know that q\_&phi;\(*z*|*x*\)=&Pi;\_i d*z*\_i = p\(&epsilon;\)&Pi;\_i d&epsilon;\_i. Which means ![integral(q\_&phi;(z|x)f(z)dz)=integral(p(&epsilon;)f(g\_&phi;(&epsilon;,x))d&epsilon;)&approx;1/L &Sigma;\_{l=1}^L f(g\_&phi;(x,&epsilon^(l)))](https://latex.codecogs.com/gif.latex?%5Cint%20q_%5Cphi%28z%7Cx%29f%28z%29dz%3D%5Cint%20p%28%5Cepsilon%29f%28g_%5Cphi%28%5Cepsilon%2Cx%29%29d%5Cepsilon%20%5Capprox%20%5Cfrac%7B1%7D%7BL%7D%5Csum_%7Bl%3D1%7D%5EL%20f%28g_%5Cphi%28x%2C%5Cepsilon%5E%7B%28l%29%7D%29%29).

Where the second approximation comes from the Monte Carlo estimate.

This reparametrization works for all distributions that I know, but not all and makes it easier/possible to derive the expectation.
## Stochastic Gradient Variational Bayes estimator (SGVB)
Using the reparametrization we assume again that z = g_&phi;(&epsilon;,x) and we have the Monte Carlo estimate ![E_{q_&phi;(z|x^{(i)})\[f(z)\]}&approx;1/L &Sigma;\_{l=1}^*L* f(g\_&phi;(&epsilon;^{(*l*)},x^{(i)}))](https://latex.codecogs.com/gif.latex?E_%7Bq_%5Cphi%28z%7Cx%5E%7B%28i%29%7D%29%7D%5C%5Bf%28z%29%5C%5D%5Capprox%5Cfrac%7B1%7D%7BL%7D%5Csum_%7Bl%3D1%7D%5EL%20f%28g_%5Cphi%28%5Cepsilon%5E%7B%28l%29%7D%2Cx%5E%7B%28i%29%7D%29%29).

This can used to define the **Stochastic Gradient Variational Bayes** estimator, which approximates the *variational lower bound*/ELBO:

![L^A(&theta;,&phi;,x^{(i)})=1/L &Sigma;\_{l=1}^L log(p\_&theta;(x^{(i)},z^{(i,l)}))-log(q\_&phi;(z^{(i,l)}|x^{(i)}))](https://latex.codecogs.com/gif.latex?L%5EA%28%5Ctheta%2C%5Cphi%2Cx%5E%7B%28i%29%7D%29%3D%5Cfrac%7B1%7D%7BL%7D%20%5Csum_%7Bl%3D1%7D%5EL%20log%28p_%5Ctheta%28x%5E%7B%28i%29%7D%2Cz%5E%7B%28i%2Cl%29%7D%29%29-log%28q_%5Cphi%28z%5E%7B%28i%2Cl%29%7D%7Cx%5E%7B%28i%29%7D%29%29)

Utilizing the [KL-divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence), which often can be integrated analytically:

![L^B(&theta;,&phi;,x^{(i)})=-KL(q\_&phi;(z|x^{(i)})||p\_&theta;(z))+1/L&Sigma;\_{l=1}^L(log(p\_&theta;(x^{(i)}|z^{(i,l)})))](https://latex.codecogs.com/gif.latex?L%5EB%28%5Ctheta%2C%5Cphi%2Cx%5E%7B%28i%29%7D%29%3D-KL%28q_%5Cphi%28z%7Cx%5E%7B%28i%29%7D%29%7C%7Cp_%5Ctheta%28z%29%29&plus;%5Cfrac%7B1%7D%7BL%7D%5Csum_%7Bl%3D1%7D%5EL%28log%28p_%5Ctheta%28x%5E%7B%28i%29%7D%7Cz%5E%7B%28i%2Cl%29%7D%29%29%29)

The first part (the KL-divergence) acts as a regularization term, while the second term is the expected reconstruction error. The second formulation typically has less variance than the first normal ELBO.
## Auto-Encoding Variational Bayes training (AEVB)
The AEVB is an algorithm to learn the best parameters using a minibatch:
- Init &theta;,&phi;

- until convergence of &theta;,&phi;

	1. Sample minibatch from *X*
	1. Sample &epsilon; from p(&epsilon;)
	1. gradients=&nabla;_{&theta;,&phi;}L^{A or B}(&theta;,&phi;,minibatch,&epsilon;)
	1. update &theta;,&phi; with the gradients using SGD/Adam

- return &theta;,&phi;

## Variational Auto-Encoder
The probabilistic encoder q_&phi;(*z*|*x*) is set as a neural network, which approximates a Gaussian N(*z*;|&mu;&sigma;), where &theta; and &phi;(=(&mu;,&sigma;)) are learnt jointly using the AEVB algorithm.

The prior p_&theta;(*z*) is a Gaussian (without parameters in the example) and p_&theta;(*x*|*z*) is a multivariate Gaussian or a Bernoulli distribution, whose parameters are trained using a MLP.

The resulting loss can be estimated and updated easily with *z*^{(*i*,*l*)}=&mu;^{(*i*)}+&sigma;^{(*i*)} &epsilon;^{(*i*)}, where &epsilon; is sampled from a standard Gaussian:

![L(&theta;,&phi;,x^{(i)})&approx;1/2&Sigma;\_{j=1}^J (1+log((&sigma;\_j^{(i))^2)-(&mu;\_j^{(i)})^2-(&sigma;\_j^{(i))^2)+1/L &Sigma;\_{l=1}^L log(p\_&theta;(x^{(i)}|z^{(i,l)}))](https://latex.codecogs.com/gif.latex?L%28%5Ctheta%2C%5Cphi%2Cx%5E%7B%28i%29%7D%29%20%5Capprox%20%5Cfrac%7B1%7D%7B2%7D%5Csum_%7Bj%3D1%7D%5EJ%20%281&plus;log%28%28%5Csigma_j%5E%7B%28i%29%7D%29%5E2%29-%28%5Cmu_j%5E%7B%28i%29%7D%29%5E2-%28%5Csigma_j%5E%7B%28i%29%7D%29%5E2%29&plus;%5Cfrac%7B1%7D%7BL%7D%20%5Csum_%7Bl%3D1%7D%5EL%20log%28p_%5Ctheta%28x%5E%7B%28i%29%7D%7Cz%5E%7B%28i%2Cl%29%7D%29%29)

## Experiments
MNIST and [Frey Face](http://www.cs.nyu.edu/~roweis/data.html) was used to test, where the VAE was used as described as above.

The generative models (**decoders**) have 500 hidden units and the recognition models (**encoders**) have 200 hidden units. Although the model seemed not to be too sensitive to parameters, also overfitting is not a very sensitive problem, as the the variational bound has a regularizing component.

The model has problems with higher dimensional latent states.