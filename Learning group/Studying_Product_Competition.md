# Studying Product Competition Using Representation Learning

## Introduction

Instead of competing with other brands it also often happens that products of the same brand compete with each other. E.g. the light version vs. the normal version.

This is why this paper also introduces the notion of complements e.g. screws and screwdrivers and exchangeable e.g. screwdrivers of different brands.

## Model

The model consists of  a two step process:

1. Representing the products in a smaller space
2. Make a prediction/ computation with the help of these representations

### Representing products

The paper introduces **product2vec**, which is identical to word2vec, with instead of sentences, baskets are used and instead of words, products are seen.

There also is a refined version of this representation *v* that lays a specific eye on price. It is assumed that price is one of the most important features and could distort the more context based parts of the embedding. This is why the paper uses a special dimension on price. This dimension is seen when using product2vec, but not optimised.

### Computation

Before showing the exact utility function, the concepts of complementarity and exchangeability need to be introduced.

**Complementarity** → $C_{AB}= \frac{1}{2}[P(A|B)+P(B|A)] \propto v_A^Tv'_B+v_B^Tv'_A$

**Exchangeability→** $E_{AB}=-\frac{1}{2}\sum_{k\neq A,B} [p(k|A) log(\frac{p(k|A)}{p(k|B)})+p(k|B)log(\frac{p(k|B)}{p(k|A)})]$

Where the exchangeability uses the KL divergence between products *A* and *B* and between *B* and *A*. This measure shows how the products interact with the other products in the shop in general. If they react similarly, they are likely exchangeable (complements also react similarly, but are a stronger property than exchangeability).

To predict a product an utility function *U* is implemented for user *i* and product *j* at time *t*. 

$U_{ijt}=\vec{\alpha} \vec{v_j}+\beta_i P_{jt}+\vec{X}_{jt}\vec{\gamma}+\lambda C_{jb}+\mu E_{jb}+\sigma\hat \eta_{jt}+ \epsilon_{ijt}$

Where 

- $\vec{X_{jt} }$→is the use of marketing initiatives
- $\alpha, \beta, \gamma, \sigma$ → are empirical sensitivities (the price dimensions of $\alpha$ are always 0)
- $\lambda, \mu$ → respectively are non-zero if one wants to use complementarity *C* or exchangeability *E*
- $\epsilon$→ is used as the error term
- P is the price elasticity

$P_{jt} = \vec{\tau_v}\vec{v_j} + \tau_Z Z_{jt} + \vec{X_jt'}\vec{\tau_X}+\tau_C C_{jb}+\tau_EE_{ib}+\eta_{jt}$

Where $\tau$ are parameter to optimise; the only important thing is that the price dimensions for the *v* variant are always 0. Also, if one should skip complementarity *C* and exchangeability *E* then these parameters will also be 0.

*Z* as the instrument variable for item *j* at time *t* and $\eta$ as difference to the actual paid price.

### Comparison to SHOPPER

Shopper only has a one-step approach not the two steps like here, also SHOPPER uses more variables and does not take marketing into account and further, price is not specially looked at.

## Experiments

### Dataset

The used dataset is IRI scanner panel dataset with:

- 30 categories
- 13’000 products
- 5’000 households
- over a year
- amounting to 280’000 baskets

### Results

Complementarity and exchangeability work as expected on their own. The addition of those terms give a slight increase in performance.

Product2vec outperfoms the baselines, feature engineered and one-hot encoding, and mostly also SHOPPER. Next to normal measures AIC an BIC were also looked at.