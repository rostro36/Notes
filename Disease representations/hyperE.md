# [Representation Tradeoffs for Hyperbolic Embeddings](https://arxiv.org/abs/1804.03329)
## Introduction
The hyperbolic space is better suited for hierarchies such as trees, since there is more space the further away you are from the center(node).
The strategy is to first form the data into a weighted tree and then project that tree into the (hyperbolic) Poincaré disk. This construction works without a loss function.
The paper also gives formal bounds for run-time and precision.
## Quality measures
There are two types of quality measures of the embeddings for local *mean average precision* (MAP) and for global *distortion* (D) is used.
- MAP tries to "guess" the closest neighbors on every vertex
	- $$MAP(f)=\frac{1}{|V|}\sum_{a\in V}\frac{1}{|N_a|}\sum_{i=1}^{|N_a|}Precision(R_{a,b_i})=\frac{1}{|V|}\sum_{a\in V}\frac{1}{deg(a)}\sum_{i=1}^{|N_a|}\frac{|N_a\cap R_{a,b_i}|}{R_{a,b_i}|} $$
- D tries to find an embedding that upholds the distances between vertices
	-  $$D(f)=\frac{1}{n \choose 2} (\sum_{u,v\in U:u\neq v} \frac{|d_V(f(u),f(v))-d_U(u,v)|}{d_U(u,v)})$$
## Embed graphs into trees
Since graphs generally don't deconstruct nicely into trees, Steiner nodes are introduced, which act as proxy nodes, that mean more or less *these nodes form a walk*. 
## Sarkar's construction
Takes trees and projects them into the two-dimensional, hyperbolic Poincaré disk.
### Algorithm
The algorithm works one node after the other and starts at the root, which lies at the origin. The algorithm embeds the children of a node the following:
- the node is reflected to the center of the hyperbolic space
- the parent is also reflected to some point *z*, according to the embedding starting form the parent
- the children then are equally placed in a circle with radius *d*, which is controlled by a hyperparameter *t*, which are maximally separated from *z*
	- $$ d=\frac{e^t-1}{e^t+1} $$
### Analysis
The algorithm keeps the neighborhood perfectly, which also means it can perfectly recover trees. The problem is, that the precision scales logarithmically with the degree of the tree and linearly with the maximum path length.
Also the distances can be recovered by dividing by *t*, here again, the bits required can be problematic.
### Augmentation
Instead of projecting to the Poincaré disk, project to the *r*-dimension Poincaré ball. This introduces a trade-off, the more dimensions, the smaller can *t* be, which means machine precision is less of a problem. Higher dimensions are better for larger degrees.
The algorithm works more or less the same, the only difference is that the children are placed on the corners of the hypercube instead of the unit circle.
## Hyperbolic multidimensional scaling
We try to find the locations of the points in the Poincaré ball by their respective distances from each other.
Unlike in Euclidean space, the centering assumption can not be used in hyperbolic space; we can not assume the points are centered around the origin, as in hyperbolic space, the distances scale with distance to the origin.
### Algorithm
The idea behind the algorithm is to transform the distances to a *hyperboloid model*, finding there the *pseudo-euclidean mean*, solve it and transforming it back to the original space.
- **hyperboloid model** &rightarrow; We use a diagonal matrix *Q* which has a 1 at the 0-th point and -1 at all the other *r* points on the diagonal, 0 off the diagonal. Given a point in *r* dimensional space (indicated by the arrow above), we can find the 0-th entry using the hyperboloid model. The hyperboloid model then is *M* and they give a distance *d*:
	- $$ M_r=\{x\in R^{r+1}|x^TQx=1\wedge x_0>0\} $$
	- $$ d_H(x,y)=acosh(x^TQy) $$
- **pseudo-euclidean mean** &rightarrow; Wherever the following variance term &Psi; is minimised, there is a pseudo-euclidean mean:
	- $$ \Psi(z;x_1,x_2, \dots,x_n)=\sum_{i=1}^n sinh(d_H(x_i,z))$$
	- $$\nabla_\overrightarrow{z}\Psi(z;x_1,x_2, \dots,x_n)|_{\overrightarrow{z}=0}=-2\sum_{i=1}^n x_{0,i}\overrightarrow{x}_i=-2X^Tu $$
- **recovery via matrix factorisation** &rightarrow; From the distances *d*, we can form a matrix *Y*, which allows to find *X* (up to rotation). Since x_0 is deterministic as seen above.
	- $$Y_{i,j}=cosh(d_H(x_i,x_j))=x_i^TQx_j=x_{0,i}x_{0,j}-\overrightarrow{x}_i^T\overrightarrow{x}_j $$
	- $$Y=uu^T-XX^T $$
## Experiments
### Datasets
The following datasets are used:
- Genetic tree of mosses
- WordNet hypernyms
- Ph.D advisor-advisee
- disease relationships
- protein interactions
- authorship relations
### Results
The (perfect) trees are nearly perfectly represented in hyperbolic space, with hardly any error for MAP or D using Sarkar's construction.
The more complicated the graph, the more useful the hyperbolic scaling variant becomes.
