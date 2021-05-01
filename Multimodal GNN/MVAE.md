# [Multimodal Generative Models for Scalable Weakly-Supervised Learning](https://arxiv.org/pdf/1802.05335.pdf)

## Introduction
Multimodal data is expensive and sparse, which leads to a weakly supervised setting of having only a small set of examples with all observations present, but having access to a larger dataset with one (or a subset of) modalities.

With multiple modalities and/or missing data, we would normally need every possible combination of inference networks (encoders), which is exponential. Assuming conditional independence among the modalities (![p_&theta;(x_1,x_2,x_3,...,x_n,z)=p(z)p_&theta;(x_1|z)p_&theta;(x_2|z)p_&theta;(x_3|z)p_&theta;(x_4|z)...p_&theta;(x_n|z)](https://latex.codecogs.com/gif.latex?p_%5Ctheta%28x_1%2Cx_2%2Cx_3%2C%5Cdots%2Cx_n%2Cz%29%3Dp%28z%29p_%5Ctheta%28x_1%7Cz%29p_%5Ctheta%28x_2%7Cz%29p_%5Ctheta%28x_3%7Cz%29p_%5Ctheta%28x_4%7Cz%29%5Cdots%20p_%5Ctheta%28x_n%7Cz%29)) one can use the product-of-experts, which reduces the complexity back to linear.

The inference networks can be trained separately, but the generative model requires joint observations.

## VAEs

A VAE jointly trains a generative model, from latent variables to observations (decoder), with an inference network from observations to latents.

A VAE is of the form p_&theta;(x,z)=p(z)p_&theta;(x|z), with p(z) being a (mostly Gaussian) prior and p_&theta;(x|z) being a neural net composed of a simple likelihood. We optimise the tractable [ELBO](https://en.wikipedia.org/wiki/Evidence_lower_bound) instead of the intractable marginal likelihood:

![ELBO(&theta;,&phi;,*X*^*i*)=E_q_&phi;(*z*|*X*^*i*)\[log(p_&theta;(*X*^*i*|*z*))\]-KL(q_&phi;(*z*|*X*^*i*)||p_&theta;(*z*))](https://latex.codecogs.com/gif.latex?ELBO%28%5Ctheta%2C%5Cphi%2CX%5Ei%29%3DE_%7Bq_%5Cphi%28z%7CX%5Ei%29%7D%5C%5Blog%28p_%5Ctheta%28X%5Ei%7Cz%29%29%5D-KL%28q_%5Cphi%28z%7CX%5Ei%29%7C%7Cp_%5Ctheta%28z%29%29)

## Deriving the joint posterior
We want to approximate the true posterior:

![p(z|x_1, ..., x_n)=p(x_1, ..., x_n|z)p(z)/p(x_1, ...,x_n)](https://latex.codecogs.com/gif.latex?p%28z%7Cx_1%2C%5Cdots%2C%20x_n%29%3D%5Cfrac%7Bp%28x_1%2C%5Cdots%2Cx_n%7Cz%29p%28z%29%7D%7Bp%28x_1%2C%5Cdots%2Cx_n%29%7D)

Here we need conditional independence:

![p(z)/p(x_1, ...,x_n)&Pi;_{i=1}^N p(x_i|z)=p(z)/p(x_1,...,x_n)&Pi;_{i=1}^N p(z|x_i)p(x_i)/p(z)=&Pi; p(z|x_i)p(x_i)/p(z)=&Pi;_{i=1}^N p(z|x_i)/&Pi;_{i=1}^{N-1}p(z) &Pi;_{i=1}^N p(x_i)/p(x_1, ...,x_n)&approx;&Pi;\_{i=1}^N p(z|x_i)/&Pi;\_{i=1}^{N-1} p(z)](https://latex.codecogs.com/gif.latex?%5Cfrac%7Bp%28z%29%7D%7Bp%28x_1%2C%20...%2Cx_n%29%7D%5Cpi_%7Bi%3D1%7D%5EN%20p%28x_i%7Cz%29%20%3D%5Cfrac%7Bp%28z%29%7D%7Bp%28x_1%2C...%2Cx_n%29%7D%5Cpi_%7Bi%3D1%7D%5EN%5Cfrac%7Bp%28z%7Cx_i%29p%28x_i%29%7D%7Bp%28z%29%7D%20%5C%5C%20%3D%5Cfrac%7B%5Cpi_%7Bi%3D1%7D%5EN%20p%28z%7Cx_i%29%7D%7B%5Cpi_%7Bi%3D1%7D%5E%7BN-1%7Dp%28z%29%20%7D%20%5Cfrac%7B%5Cpi_%7Bi%3D1%7D%5EN%20p%28x_i%29%7D%7Bp%28x_1%2C%5Cdots%2Cx_n%29%7D%20%5Capprox%5Cfrac%7B%5Cpi_%7Bi%3D1%7D%5EN%20p%28z%7Cx_i%29%7D%7B%5Cpi_%7Bi%3D1%7D%5E%7BN-1%7D%20p%28z%29%7D)

If we assume p(z|x_i) is properly contained in q(z|x_i), then one can construct **MVAE-Q**:

![q(z|x_1, ..., x_n)=&Pi;\_{i=1}^N q(z|x_i)/&Pi;\_{i=1}^{N-1}p(z)](https://latex.codecogs.com/gif.latex?q%28z%7Cx_1%2C%5Cdots%2C%20x_n%29%3D%5Cfrac%7B%5Cpi_%7Bi%3D1%7D%5EN%20q%28z%7Cx_i%29%7D%7B%5Cpi_%7Bi%3D1%7D%5E%7BN-1%7Dp%28z%29%7D)

If we approximate p(z|x_i) with q(z|x_i)=ç(z|x_i)p(z) to get the more computationally stable **MVAE**, where ç(z|x_i) is the underlying inference network:

![p(z|x_1, ..., x_n)~&Pi;\_{i=1}^N p(z|x_i)/&Pi;\_{i=1}^{N-1} p(z)~&Pi;\_{i=1}^N ç(z|x_i)p(z)/&Pi;\_{i=1}^{N-1} p(z)=p(z) &Pi;\_{i=1}^N ç(z|x_i)](https://latex.codecogs.com/gif.latex?p%28z%7Cx_1%2C%5Cdots%2C%20x_n%29%5Capprox%5Cfrac%7B%5Cpi_%7Bi%3D1%7D%5EN%20p%28z%7Cx_i%29%7D%7B%5Cpi_%7Bi%3D1%7D%5E%7BN-1%7D%20p%28z%29%7D%5Capprox%5Cfrac%7B%5Cpi_%7Bi%3D1%7D%5EN%20%5C%5B%5Ctilde%7Bq%7D%28z%7Cx_i%29p%28z%29%5C%5D%7D%7B%5Cpi_%7Bi%3D1%7D%5E%7BN-1%7Dp%28z%29%7D%3Dp%28z%29%5Cpi_%7Bi%3D1%7D%5EN%20%5Ctilde%7Bq%7D%28z%7Cx_i%29)

Where &Pi;\_{i=1}^N ç(z|x_i) is a **product of experts**(PoE) and p(z) is a **prior expert**.

In general, these distributions have no closed form solution, only in the (quite usual) case of Gaussian experts and priors, the resulting function is again a Gaussian.

If there is a modality (*j*) missing in the data, then one can simply put in the value of 1 instead of p(z|x_j). 

## Training the MVAE
To train the N inference networks, we give:
1. The ELBO using the product of the N Gaussians (to get *all* the cross modalities right)
1. Each of the N individual ELBOs (to train each of them by themselves, to max out that part)
1. *k* ELBO terms, which use a random subset of the N Gaussians (similar to dropout)
	There is hardly an effect on data log-likelihood in increasing *k*; the variance diminishes, but the computational effort increases. It is suggested to use a small *k* in practice.
  
## Experiments
The uni-modal datasets are transformed to multi-modal problems by treating labels as a second modality. MVAE has about the same number of parameters as [BiVCCA](https://arxiv.org/pdf/1705.10762.pdf) (in the case of only two modalities).

MVAE performs state-of-the-art (which is mostly the more resource-intensive [JMVAE](https://arxiv.org/pdf/1611.01891.pdf)) in most tasks, except in p(x_2|x_1) (classification), where [CVAE](https://papers.nips.cc/paper/2015/file/8d55a249e6baa5c06772297520da2051-Paper.pdf) performs better, but it does not learn the joint distribution, therefore can not generate.

In a weakly supervised setting (where there are many examples of only a subset of modalities, but fewer of all modalities) MVAE performs better than [JMVAE](https://arxiv.org/pdf/1611.01891.pdf), both need some examples (~1'000) to catch up to classical neural nets. MVAE especially shines in the *middle area* (100'000-10'000'000 samples), where the individual experts could be trained, but the more classical approaches could not perfect every parameter.