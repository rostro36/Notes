# [Adam: A Method for Stochastic Optimization](https://arxiv.org/abs/1412.6980)
## Overview
Adam stands for **Ada**ptive **m**oment estimation and is 
- straight forward
- efficient
- little memory consuming
- diagonally scaling invariant
- suited for large data and parameter spaces
- good for non-stationary objectives (like in different batches)
- efficient as it only uses first order derivations

It can be described as a combination of AdaGrad and RMSProp.
## Algorithm
We want to get the best parameters &theta; for function *f*

First define some preliminaries and notation
- element-wise square:
    - $g_t^2 = g_t \odot g_t $
- 1st order moment
    - $m_0 =0$
- 2nd order moment
    - $v_0 =0$
- time step
    - $t =0$

And then perform the loop operations until &theta; has converged:
- update time step
    - $t = t+1$
- get first order gradient
    - $g_t = \nabla_\theta f_t(\theta_{t-1}$)
- update first moment with parameter &beta;_1
    - $m_t=\beta_1 m_{t-1}+(1-\beta)g_t $
- update first moment with parameter &beta;_2
    - $v_t=\beta_2 v_{t-1}+(1-\beta)g_t^2 $
- scale first moment for bias correction
    - $\hat{m}_t=\frac{m_t}{1-(\beta_1)^t}$
- scale second moment for bias correction
    - $\hat{v}_t=\frac{v_t}{1-(\beta_2)^t}$
- update parameters with step size &alpha; and scaling parameter &epsilon;
    - $\theta_t=\theta_{t-1}-\alpha \frac{\hat{m}}{\sqrt{\hat{v}}+\epsilon} $
## Proofs
### Step size
For $\epsilon=0$ the step size (triangle) is
$$|\triangle_t| = \alpha\frac{\hat{m_t}}{\sqrt{\hat{v_t} }} $$
And has two upper bounds possible bounds:
$$ |\triangle_t| = \alpha\frac{1-\beta_1}{\sqrt{1-\beta_2 }}\cup \alpha $$ 
The step size cancels any scaling factors with the fracture.
### Bias correction term
Without bias correction we have:
$$v_t=(1-\beta_2)\sum_{i=1}^t (\beta_2)^{t-i}*g_i^2$$
With a moving average we need to get expections and then we can multiply this equation out:
$$\mathop{\mathbb{E}}[v_t] = \mathop{\mathbb{E}}[g_t^2]*(1-(\beta_2)^t+\xi $$
With &xi; being a correction term for the movement.

This equation gives the exact formulation of the error correction term.
### Theoretical convergence analysis
Under common conditions, Adam achieves the state-of-the-art theoretical convergence limit for regret *R*.
$$R(T)=\sum_{t=1}^T[f_t(\theta_t)-f_t(\theta^*)] $$
$$\frac{R(T)}{T}=O(\frac{1}{\sqrt{T}}) $$
## Extensions
On top of the vanilla Adam, which has nice proofs, the paper also shows two extensions that should work even better.
### AdaMax
Instead of using the second order momentum, which then later need to be square rooted and bias correction, the paper introduces AdaMax. AdaMax uses the max instead of the second momentum and for no term, bias correction is needed. Unfortunately the max takes a lot more computing power.
### Temporal averaging
The other extension is temporal averaging, which changes the update step:
$$\bar{\theta_t}=\beta_2\theta_{t-1}+(1-\beta_2)\theta_t$$
This term then has to be corrected to use properly:
$$\hat{\theta_t}=\frac{\bar{\theta_t}}{1-(\beta_2)^t} $$
## Experiments
### Setup
For datasets MNIST and IMDB movie review was used for a CNN model and a RNN model respectively. The step size was scaled by the reciprocal value of the square root of *t*. The other baselines were *AdaGrad* and *SGD* with momentum.
### Results
What stands out is that in all tests, Adam has not shown any weakness; Adam was at least about as good as the baselines in all the tasks, while a baseline usually failed at least in one test.

It was also shown that the second order moment is not as important as the first order moment and that bias correction is even more for larger &beta;_2 to get the best parameters.