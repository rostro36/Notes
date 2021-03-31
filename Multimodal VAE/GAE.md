# [Variational Graph Auto-Encoders](https://arxiv.org/pdf/1611.07308.pdf)
## Input
A undirected, unweighted graph *G*, with *N* nodes \(*v*\) and their *D* features \(*X*\) and edges \(*e*\). The edges are defined in the adjacency matrix *A* and it is assumed that each node is connected to itself. And there is a diagonal degree matrix *D*. The latent variables \(*z\_i*\) are in a *N* &times; *F* big matrix *Z*.
## Inference model
q\(*Z*|*X*,*A*\)=&pi;\_\{node *v*} q\(*z_i*|*X*,*A*\), with

q\(*z_i*|*X*,*A*\)=Gauss\(*z\_i*|&mu;\_*i*, diag\(&sigma;\_i^2\)\)

&mu; is computed from a GCN\(*X*,*A*\) and log\(&sigma;\)=GCN\(*X*,*A*\)

Both use a *symmetrically normalised adjacency matrix* *Ã*=*D*^\{-1\/2\}*A* *D*^\{-1\/2\}
## Generative model
p\(*A*|*Z*\)=&Pi;\_\{startnode *i*\}&pi;\_\{endnode *j*\} p\(*A*\_\{*ij*\}|*z\_i*,*z\_j*\)

with p\(*A*\_\{*ij*\}=1|*z\_i*,*z\_j*\)=&sigma;\(*z\_i*^T *z\_j*\)
## Learning
The model optimises the ELBO

![\mathcal{L}=\mathbb{E}_{q\(Z|X,A\)}\[log\(p\(A|Z\)\)\]-KL\[q\(Z|X,A\)||p\(Z\)\]](https://latex.codecogs.com/gif.latex?%5Cmathcal%7BL%7D%3D%5Cmathbb%7BE%7D_%7Bq%5C%28Z%7CX%2CA%5C%29%7D%5C%5Blog%5C%28p%5C%28A%7CZ%5C%29%5C%29%5C%5D-KL%5C%5Bq%5C%28Z%7CX%2CA%5C%29%7C%7Cp%5C%28Z%5C%29%5C%5D)

And use the prior p\(*Z*\)=&pi;\_\{*v*\} p\(*z_i*\)=&pi;_\{*v*\}Gauss\(*z\_i*|0,**I**\), and reweighing terms with A\_\{ij\}=1. For training is a full-batch gradient descent algorithm used, that also uses the *reparametrisation trick*.
## Graph auto-encoder \(GAE\)
For a non-probabilistic variant, we compute the reconstructed adjacency matrix *Â*=&sigma;\(*ZZ*^T\), *Z*=GCN\(*X*,*A*\)
## Experiments
The task is given the node embeddings *Z*, predicting whether there is an edge in a citation graph.

The baselines are from [spectral clustering](https://www.researchgate.net/publication/220451898_Leveraging_social_media_networks_for_classification) and [DeepWalk](https://arxiv.org/abs/1403.6652), which both can not use input features \(*X*\).

The test shows that GAE and VGAE are comparable to the baselines without features, but are substantially better with the features also given. Using a Gaussian prior might be a bad prior combined with the inner product decoder that tries to push embeddings away from the origin. VGAE still performs mostly better than GAE.