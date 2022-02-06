# [GraphLIME:Local Interpretable Model Explanations for Graph Neural Networks](https://arxiv.org/abs/2001.06216)
## Overview
GNNs (Graph Neural Networks) are an important neural network that with applications in physics, biology, chemistry and social networks. This paper now introduces *GraphLIME*, which introduces a *LIME* model using the *Hilbert-Schmidt Independence Criterion* (HSIC) Lasso.

This paper is mainly based on two other papers:
- [GNNexplainer](https://proceedings.neurips.cc/paper/2019/hash/d80b7040b773199015de6d3b4293c8ff-Abstract.html) &rightarrow; find subgraphs and select features of the node in question
- [LIME](https://arxiv.org/abs/1606.05386) &rightarrow; finds features as explanations, but is mainly used for Euclidean data
## GraphLIME
GraphLIME stands for *Local Interpretable Model Explanation for Graph Neural Networks*.

GraphLIME assumes that each feature is understandable by a human.

GraphLIME uses an interpretable model class *G* as a surrogate model for our real model *f*. In this paper *G* is a HSIC Lasso model. *X_n* are the node features of *n*-hop neighbours around the main node *v*.

$$\xi(v) = \argmin_{g\in G} g(f,X_n) $$
## HSIC model
The HSIC model is a supervised nonlinear features selection method that uses *y_i* as label for every node to compute the feature importance &beta;:
$$ y_i = f(x_i)$$

The HSIC optimisation lasso model is given
$$\min_{\beta\in R^d}\frac{1}{2}||\bar{L}-\sum_{k=1}^d\beta_k\bar{K}^{(k)}||_F^2+\rho||\beta||_1, \beta \geq 0 $$
With
$$\bar{L}=HLH/||HLH||_F $$
$$ L_{ij}=L(y_i,y_j)=\exp(-\frac{||y_i-y_j||_2^2}{2\sigma^2_y})$$
$$ H = I_n-\frac{1}{n}1_n1_n^T$$
$$ \bar{K} =HK^{(k)}H/||HK^{(k)}H||_F$$
$$[K^{(k)}]_{ij}=K(x_i^{(k)},x_j^{(k)})=\exp(-\frac{(x_i^{(k)}-x_j^{(k)})^2}{2\sigma^2_x})$$
- *bar* &rightarrow; normalised versions to the Frobenius norm
- *L* &rightarrow; kernel for output (Gaussian in this example)
- *K* &rightarrow; kernel generally (Gaussian in this example)
- &rho; &rightarrow; regularisation parameter
- **&beta;** &rightarrow; the weights of the features, what we needed
The *K* part of the first part says that if two features are dependant on each, this value is bigger and therefore worse, with a lower importance. The *L* part indicates the relation to the target *y*, which the higher, the better the importance.

For the explanation we take the top-k most important features.
## Experimentation
### Setup
The complex models are a GraphSAGE and a GAT model and the explanation systems are *LIME*, *GNNexplainer*, *greedy* (remove the most important features until the prediction changes), *random* (select *K* features at random) and *GraphLIME*.

The datasets were *Cora* and *Pubmed*, which are publication datasets with TF-IDF vectors as node features.

### Experiments
The first experiment added 10 noise features and compute 200 explanations for each model. The more bogus features were chosen, the worse the score. It was shown that the Graph-centric models can do this task well. LIME and the lower baselines are comparable with each other

The second experiment randomly determined 30% of features as untrustworthy. An oracle was constructed that returned the trustworthiness of a prediction, whether or not it changes with untrustworthy features, which is also the target. The models respond untrustworthy if at least one feature is seen in the explanation. *LIME* performs better than *GNNexplaner*, but worse than *GraphLIME*. The lower baselines perform significantly worse. 

The third experiment tests if GraphLIME can help with model selection. It does so by introducing bogus features again and training models with and without those bogus features. The one with the bogus (incognito) features has worse accuracy. The task then is to choose the correct model by looking at the amount of bogus features. In this task, most explanation models perform similarly, but *GraphLIME* performed measurably better.