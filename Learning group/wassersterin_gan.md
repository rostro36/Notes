# [Wasserstein Generative Adversarial Networks](https://proceedings.mlr.press/v70/arjovsky17a.html)
## Overview
Generative Adversarial Networks (GANs) ususally use the Kullback-Leibler divergence to get a data distribution, but that does not work nicely if the model manifold and the true data distribution have a non-negible intersection, which is most often the case.
To alleviate this issue, often times Gaussian noise is added, but with this, information gets less precise and e.g. images get blurry.

The alternative is to use a parametrized feed-forward neural network that maps sampled inputs from &theta; to $P_\theta$. We assume this neural net is continuous, so taht the distance is continous and losses can be applied.

Downfalls of vanilla GANs include mode collapse, which make training unstable, the forcing of certain model architectures for the discriminator or the generator and the unclear connection from loss to sample quality. All of these problems are mitigated with the Wasserstein distance.

## Probability comparison functions
The paper lists four probability comparison functions
- Total variation (TV)
     - $\delta(P_r,P_g) = \sup_{A\in\Sigma} |P_r(A)-P_g(A)|$
- Kullback-Leibler divergence (KL)
     - $KL(P_r||P_g) =\int log(\frac{P_r(x)}{P_g(x)}P_r(x)d\mu (x) $
- Jensen-Shannon distance (JS)
     - $JS(P_r,P_g) = KL(P_r||P_m) + KL(P_g||P_m)$ 
- Earth-Mover distance (EM) &rightarrow; continuous
     - $W(P_r,P_g)=\inf_{\gamma \in \Pi(P_r,P_g)} E_{(x,y)\sim \gamma}[||x-y||]}$
     - With:
          - &Pi;(P_r,P_g) &rightarrow; set of all all join distibutions &gamma; whose marginals are P_r and P_g
### Optimise EM distance
Since the *inf* of the EM distance is very expensive, a trick is needed to make the computation faster.
Luckily there is the Kantorovich-Rubinstein duality, which states that:
$$EM(P_r,P_\theta) =\sup_{||f||_l\leq 1} E_{x\sim P_r}[f(x)]- E_{x\sim P_r}[f(x)] $$
Where:
- *f* &rightarrow; are all 1-lipschitz functions;

Instead of searching for the best imaginable *x*, we search for the best neural network approximation of *x* called *g*.
$$\nabla_\theta EM(P_r,P_\theta) = - E_{z\sim p(z)}[\nabla_\theta f(g_\theta (z))] $$
## EM algorithm
The Em algorithm consists of one loop nested in another loop:
- As long as *f* has not converged
    - Find the best *g* by sampling from priors and real data
    - Update *f* with the best *g* and prior samples and go on to the next step
## Experiments
Because the EM distance is always continuous, the gradients are also always stable and there is no mode collapse.

It was shown that WGAN is allergic to Adam, as the target is highly non-stationary and the momentum in Adam does not paly along well, RMSProp is adviced instead.

Experiments also show that the new loss correlates nicely with sample quality meaning a low loss implies a good sample.
