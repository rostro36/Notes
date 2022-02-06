# [Towards a Rigorous Theoretical Analysis and Evaluation of GNN Explanations](https://arxiv.org/abs/2106.09078)
## Overview
A meaningful GNN explanation should be *reliable* under different circumstances and *interpretable* enough to be easily understandable.

A *reliable* GNN explanation is
- *faithful*
- *fairness preserving*
- *stable*

## Current GNN explanation models
- Gradient based &rightarrow; gradient heat map along edges
- Perturbation based &rightarrow; subgraphs which are most influential even under different perturbations of the input graph
	- Which edges can be dropped without decreasing the accuracy?
- Surrogate model based &rightarrow; create a simpler graph, that predicts similarly
## GNN explanations
The setting for this paper are undirected, unweighted graphs.

An explanation (*E*) is given by the feature mask on the node features and a subset of edges. 
## Faithfulness
$$\sum_{u'\in K}||f(G_{u'})-f(t(E_u,G_{u'}))||_2 \leq \delta $$
With 
- *u* &rightarrow; the node
- *G* &rightarrow; the local neighbourhood around *u*
- *K* &rightarrow; the perturbed neighbourhood around *u*
- *f* &rightarrow; the model and it's output.
- *t* &rightarrow; the masking process to get better explanations

This means the difference between the explained (perturbed) graph and the real (perturbed) graph is low.
## Stability
$$ distance(E_u,E_{u'} \leq \delta) $$
The distance between the real explanation and the explanation on a perturbed node is small.
## Fairness preservation
### Counterfactual fairness
$$ distance(E_u, E_{u^s}) \propto f(G_u)-f(G_{u^s}) $$
The distance between the explanation on the real node and a counterfactual node should be proportional to the model of the real node and the counterfactual node.
### Group fairness
$$ |SP(\tilde{y}_K) - SP(\tilde{y}_K^{E_u})| \leq \delta $$
$$ SP(\tilde(y)_K) = |P(\tilde{y}_{u'}=1|s=0)-P(\tilde{y}_{u'}=1|s=1)| $$
The statistical parity (*SP*) is unaffected by either using the real node or the explanation *E_u* for the perturbed graph (*K*).

## Theoretical analysis
The bounds are not tied to a specific GNN-architecture, only that they use message-passing.
## Faithfulness
$$\sum_{u'\in K}||f(G_{u'})-f(t(E_u,G_{u'}))||_2 \leq \gamma(1+|K|)||\Delta||_2 $$
With:
- *f* &rightarrow; softmax
- &gamma; &rightarrow; product of the Lipschitz constants in the GNN activation and weight matrices
- &Delta; &rightarrow; architecture specific term
## Stability
Again, there are three explanation methods:
- *gradient based*
- *perturbation based*
- *surrogate model based*
### Gradient based
$$ ||\nabla x_{u'}f-\nabla x_{u}f||_p \leq \gamma_3||x_{u'}-x_u||_p $$
With &gamma;_3 being the Lipschitz constant that is equal to (for a single message pass)
$$\gamma_3 = ||y_u-\tilde{y}_u||_p ||(W_{fc})^T||_p ||W_a^1||_p ||(W_a^1)^T||_p $$ 
### Perturbation based
$$ ||z^l_{u',v}-z^l_{u,v}||_2 \leq \gamma_4^l||q^l_{u',v}-q^l_{u,v}||_2$$
Where *z* is the concatenated edge mask explanation from nodes *u*, *v* on message pass number *l*. *q* is the same thing, but for the real nodes. &gamma;_4 is given by
$$\gamma_4^l =C_{SP}C_{LN}^l||W_2^l||_2||W_1^l||_2 $$
With *C_LN* as layer normalisation constant and *C_SP* as a constant of the softplus function.
### Surrogate model based
$$||\beta'_k-\beta_k||_F\leq \delta_2 trace(\frac{1}{e^TW^{-1}e})^{-1}-I ) $$
## Fairness
### Group fairness
|$$ |SP(\tilde{y}_K) - SP(\tilde{y}_K^{E_u})| \leq \sum_{s\in \{0,1\}}|Err_{D_s}(f(t(E_u,G_{u'}))-f(G_{u'}))| $$
Where *Err_D* is the statistical error under the distribution *D*
### Counterfactual gradient fairness
$$ ||\nabla x_{u^s}f-\nabla x_{u}f||_p \leq \gamma_3 $$
It is the same as from the stability proof, but we know that the difference is only the sensitive feature, which is 1.
### Counterfactual perturbation fairness
$$ ||z^l_{u',v}-z^l_{u,v}||_2 \leq \gamma_4^l||q^l_{u^s,v}-q^l_{u,v}||_2$$
Same as from the stability proof, but we use the counterfactual *q* instead of the perturbed one.
### Counterfactual surrogate model fairness
$$||\beta'_k-\beta_k||_F\leq \delta_2 trace(\frac{1}{e^T \overline{W}^{-1}e})^{-1}-I) $$
Same as from the stability proof, but we use the counterfactual *W* instead of the perturbed *W*.
## Experiments
### Datasets
The used datasets are:
- *German credit graph* with 1'000 nodes to classify credit risks with help of similarity links
- *Recidivism graph* with 19'000 nodes to classify bail status with links based on criminal history and demographic
- *Credit defaulter graph* with 30'000 nodes based on payment behaviour and demographics
- *Ogbn-arxiv* citation graph with 170'000 nodes to classify the 40 categories
### Metrics
The previously introduced metrics are used:
- *faithfulness* $$ \sum_{u'\in K}||f(G_{u'})-f(t(E_u,G_{u'}))||_2 $$
- *instability* with the l1 norm $$ distance(E_u, E_{u'})$$ 
- *counterfactual fairness* with the l1 norm $$ distance(E_u, E_{u^s})$$
- *group fairness* $$|SP(\tilde{y}_K) - SP(\tilde{y}_K^{E_u})|$$

### Results
The theoretical upper bounds hold and are somewhat proportional to the empirical values across all four properties.

Surrogate models seem to produce the most reliable explanations as they are stable and are good in preserving group fairness. Perturbation based methods are more faithful and are good in counterfactual fairness, **but the random baseline was the most faithful explanation on two datasets.**

None of the SOTA explanation methods are simultaneously faithful, stable and fair and most prioritize faithfulness over the other properties.

    State-of-the-art explanation methods are optimized to prioritize faithfulness over stable and fair explanations, suggesting that GNN explanation methods do not generate reliable explanations and that there are ample opportunities for algorithmic innovation.