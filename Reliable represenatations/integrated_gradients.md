# [Axiomatic Attribution for Deep Networks](http://proceedings.mlr.press/v70/sundararajan17a.html)
## Overview
The main goal of this method is to attribute the prediction of a model to the input features in contrast to a baseline prediction.

The baseline prediction is needed as a counterfactual, where the information is not there. One standard baseline would be the all black image. It is important that this baseline is cleverly chosen, as e.g. a noisy picture might be easily changed to say an elephant, but takes much more to form a microscope.

## Fundamental axioms
For attribution, the straight forward method is to sum and multiply all weights and gradients in a neural network and then trace them back to every feature, where we then can compare them.

The problem with this method is that it breaks **sensitivity**

### Sensitivity
If there is only one differing feature between different predictions, then this feature should have a non-zero attribution.

A counterexample that breaks the sensitivity of the straight forward method is a ReLU activation that has a 0 gradient at both 0 and the rectified part.

Another line of work gets around the sensitivity problem by taking a big discrete step to avoid flat regions. But the problem is that they violate **implementation invariance**.

### Implementation invariance
Two models that produce the same output for the same input should have the same attributions. Thanks to the chain rule this always holds as the implementation detail *h* gets multiplied out:

$$\frac{\delta f}{\delta g} = \frac{\delta f}{\delta h} * \frac{\delta h}{\delta g}$$

This does not hold for the contrasting discrete gradients in general because of dividing by 0:
$$ \frac{f(x_1)-f(x_0)}{g(x_1)-g(x_0)} \neq \frac{f(x_1)-f(x_0)}{h(x_1)-h(x_0)}*\frac{h(x_1)-h(x_0)}{g(x_1)-g(x_0)} $$

What if we have two identical features x and x', two models that ignore the other feature would always post the same prediction, but give completely different attributions.

## Path methods
Instead of only taking the grads at discrete points, take the grads at all of the points in the path between the point *x* and the baseline *x'* along a dimension *i*. 

A path is given by &gamma;(0)= *x'* and &gamma;(1)=*x*. And the path gradients are computed as follows:
$$PathGrads_i^\gamma(x)=\int_{\alpha=0}^1\frac{\delta F(\gamma(\alpha))}{\delta\gamma_i(\alpha)}\frac{\delta\gamma_i(\alpha)}{\delta\alpha}d\alpha $$

All path gradients fulfill **completeness**, which means that the attributions sum up to the difference of the two outputs. Which yields:
$$\sum_{i=1}^n PathGrads_i(x) =F(x)-F(x') $$

Path gradients fulfill both of the fundamental axioms. *Completeness* implies *sensitivity* and we never looked at the architecture of *F*, so *implementation invariance* is also upheld.

Additionally to the fundamental axioms, path gradients also fulfill *negative sensitivity*, which gives 0 attribution to ignored variables. Also *linearity* is upheld, which says that a linear combination of two models should result in the the same linear combination of the previous attributions.

## Integrated gradients
The proposed method are *integrated gradients*, which take the most direct path possible.
$$ Integrated Grads_i(x) = (x_i-x'_i)*\int_{\alpha=0}^1\frac{\delta F(x'+\alpha*(x-x'))}{\delta x_i}d\alpha$$

This is chosen because it is the most natural path as well as it *preserves symmetry*. Symmetry preserving means that if there are two dimensions, which have the same input in the baseline and the main point, then both variables should have the same attribution.

To speed-up the process one can take the Riemannian approximation of choosing *n* equidistant values in the path instead integrating over the whole path.
## Experiments
There are only empirical experiments conducted, but on many different models and datasets, where integrated gradients performed very well in image classification, retina image classification, NLP question classification, neural machine translation and a chemistry model.

It was also shown that using this method one could find problems with the underlying model as the attribution acted weirdly.