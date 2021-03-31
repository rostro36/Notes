# [Multimodal Generative Models for Scalable Weakly-Supervised Learning](https://arxiv.org/pdf/1802.05335.pdf)

## Introduction
Multimodal data is expensive and sparse, which leads to a weakly supervised setting of having only a small set of examples with all observations present, but having access to a larger dataset with one (or a subset of) modalities.

With multiple modalities and/or missing data, we would normally need every possible combination of inference networks (encoders), which is exponential. Assuming conditional independence among the modalities (p_&theta;(x_1,x_2,x_3,...,x_n,z)=p(z)p_&theta;(x_1|z)p_&theta;(x_2|z)p_&theta;(x_3|z)p_&theta;(x_4|z)...p_&theta;(x_n|z)) one can use the product-of-experts, which reduces the complexity back to linear.

The inference networks can be trained separately, but the generative model requires joint observations.

## VAEs

A VAE jointly trains a generative model, from latent variables to observations (decoder), with an inference network from observations to latents.

A VAE is of the form p_&theta;(x,z)=p(z)p_&theta;(x|z), with p(z) being a (mostly Gaussian) prior and p_&theta;(x|z) being a neural net composed of a simple likelihood. We optimise the tractable [ELBO](https://en.wikipedia.org/wiki/Evidence_lower_bound) instead of the intractable marginal likelihood:

[ELBO](https://en.wikipedia.org/wiki/Evidence_lower_bound) (&theta;,&phi;,*X*^*i*)=E_q_&phi;(*z*|*X*^*i*)\[log(p_&theta;(*X*^*i*|*z*))]-[KL](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence) (q_&phi;(*z*|*X*^*i*)||p_&theta;(*z*))

## Deriving the joint posterior
We want to approximate the true posterior:

p(z|x_1, ..., x_n)=p(x_1, ..., x_n|z)p(z)/p(x_1, ...,x_n)

Here we need conditional independence:

	p(z)/p(x_1, ...,x_n)&Pi;_{i=1}^N p(x_i|z)

	=p(z)/p(x_1,...,x_n)&Pi;_{i=1}^N p(z|x_i)p(x_i)/p(z)=&Pi; p(z|x_i)p(x_i)/p(z)=&Pi;_{i=1}^N p(z|x_i)/&Pi;_{i=1}^{N-1}p(z) &Pi;_{i=1}^N p(x_i)/p(x_1, ...,x_n)

	&approx;&Pi;\_{i=1}^N p(z|x_i)/&Pi;\_{i=1}^{N-1} p(z)

If we assume p(z|x_i) is properly contained in q(z|x_i), then one can construct **MVAE-Q**:

q(z|x_1, ..., x_n)=&Pi;\_{i=1}^N q(z|x_i)/&Pi;\_{i=1}^{N-1}p(z)

If we approximate p(z|x_i) with q(z|x_i)=ç(z|x_i)p(z) to get the more computationally stable **MVAE**, where ç(z|x_i) is the underlying inference network:

p(z|x_1, ..., x_n)~&Pi;\_{i=1}^N p(z|x_i)/&Pi;\_{i=1}^{N-1} p(z)~&Pi;\_{i=1}^N ç(z|x_i)p(z)/&Pi;\_{i=1}^{N-1} p(z)=p(z) &Pi;\_{i=1}^N ç(z|x_i)

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