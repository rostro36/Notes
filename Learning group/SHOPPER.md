# SHOPPER: A Probabilistic Model of Consumer Choice with Substitutes and Complements

## Overview

SHOPPER is a model that is heavily leaned on LDA with (hierarchical) latent variables and allows for a generative process on consumer behaviour. It lays a special eye on price and allows for counterfactuals on price.

## Explanations

The difference between **compliments** and a **co-purchase** is that when hiking the price of an item, it’s compliment will be much less popular, like hot dog bread and hot dog sausages, other than for co-purchases, where the co-purchase will be unaffected, like formula and diapers.

**Exchangeable** items interact similarly with other items. In fact the definition is the same as in the other [paper](https://www.notion.so/Studying-Product-Competition-Using-Representation-Learning-e3560a38be4e42b2bd9f86ca2d2474ec), as well as for **compliments**. 

## Model

### Naive model

The naive model assumes a shopper *u* that has bought some items *y* of objects in the store *c* in week *t.* The probability for the vanilla shopper to choose object $c_{n+1}$  is:

$Prob(c_{n+1}|[y_0, ..., y_n]) \propto exp(\theta_u^T\alpha_c+\rho_c^T(\frac{1}{n}\sum_{j=1}^{n}\alpha_{y_j}))= exp(\Psi_{nc})$

With:

- $\alpha$ →the product features
- $\rho$ → the interaction coefficient between items
- $\theta$ →the preferences of the user

For the naive model, we assume a user computes the probability and chooses an item; updates the sum and then leaves the store if he chose the special *checkout* item or starts the cycle from the beginning again with updated items.

### Utility function

The probability of a new item is tied to the utility function *U*, which then will be used for softmax, but before the utility function, we need to define the utility function of a item *c* at time *t*.  

$\psi_{tc}=\lambda_c+\theta_{ut}^T\alpha_c-\gamma_{ut}^T\beta_clog(r_{tc})+\delta_{wt}^T\mu_c$

With:

- 1st term→item popularity
- 2nd term →customer preference
- 3rd term →price effects
    - the real price $r_{tc}$ is taken as logarithm to make the scaling easier
    - the parameter before the price gets used for matrix factorization to make it easier for the many unobserved user-item combinations
- 4th term →seasonality effects

This leads to the utility $U_t(\mathit{y_t})=\sum_{c\in \mathit{y_t}} \psi_{tc}+\frac{1}{|\mathit{Y}|-1} \sum_{(c,c')\in/\mathit{Y} x \mathit{Y}, c\neq c' }\nu_{c,c'}=\sum_{c\in \mathit{y_t}}\Psi(c, \mathbb{y}_t)$

With the special y as the possible items and $\nu$ as the interaction term of the previous items. 

### Unordered baskets

The used model assumes that items are taken in a clear order. This does not (necessarily) reflect reality and we have to sum over all permutations $\pi$  of the previously selected items. $P(\mathit{y}_t,\rho,\alpha)=\sum_\pi P(y_{t,\pi}|\rho,\alpha)$

### Thinking ahead

Instead of strictly thinking about one next item, *thinking ahead* allows to also think about a compliment already with the help of the max term in the utility:

$\Psi(c, \mathbb{y}_{t,i-1})=\psi_{tc}+\rho_c^T(\frac{1}{i-1}\sum_{i=1}^{i-1}\alpha_{y_t})+\max_{c'\notin [y_{t,i-1},c]}\{\psi_{tc}+\rho_{c'}^T(\alpha_c+\frac{1}{i}\sum_{j=1}^{i-1}\alpha_{y_j})\}$

## Inference

A gaussian prior was used for $\rho_c, \alpha_c, \theta_u, \lambda_c, \delta_w, \mu_c$ and Gamma priors for $\gamma_u, \beta_c$. Since the inference goes over way too many items, variational inference is used, which was mostly shown in the appendix and looked quite complicated. It was also shown that the best feature dimensionality for the item properties lies at 100 and for price and seasonality at 10. 

## Experiments

### Dataset

A [proprietary panel dataset](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.519.7591&rep=rep1&type=pdf) was used for this paper.

- 370 categories
- 5’600 products
- 3’200 households
- 97 weeks
- amounting to 570’000 baskets

The last two months were used for testing; the seasonal prior of the previous year was assumed for the test period again.

## Results

The baselines are Poisson-, Bernoulli embeddings, as well as a hierarchical Poisson factorisation. In comparison to them, SHOPPER uses much more parameters, but also shows clearly better performance, especially with price interventions (counterfactuals). *Thinking ahead* gives a small boost outside the variance of the models. 

Complements and substitutes work as expected and can be found as well as price sensibility and the embedding TSNE fits the expectation.