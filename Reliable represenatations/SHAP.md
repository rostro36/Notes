# [A Unified Approach to Interpreting Model Predictions](https://arxiv.org/abs/1705.07874)
## Overview
The most performant models are often very complicated, such that even experts have trouble interpreting them. This paper introduces *SHAP*, that assigns each features an importance value per prediction.

## Key findings
The paper makes a unification of previous models, that leads to three key findings:
- Any explanation of a prediction is a model itself called *explanation model*. The paper defines a class of *additive features attribution methods*.
- These methods can be solved using game theory and *SHAP* values are introduced as an approximation.
- Those *SHAP* values were state-of-the-art in 2017.

## Shapley values
TO compute Shapley regression values of feature importances, the model has to be trained for each feature subset *S* of features *F*. THere are approximation possible, so not every subset has to be trained though.
$$\phi_i=\sum_{S\subseteq F  \setminus i} \frac{|S|!(|F|-|S|-1)!}{|F|!}[f_{S\cup i}(x_{S\cup i})-f_S(x_S)]$$
Where *f* is the scoring function.

## Additive Feature Attribution Methods
### Definition
Additive feature attribution methods have by definition an explanation model that is a linear function of binary variables:
$$g(z')=\phi_0+\sum_{i=1}^M\phi_i z'i $$
With:
- **g** &rightarrow; explanation model
- **z'** &rightarrow; simplified input in the vicinity of the simplified input of **x** 
- &phi; &rightarrow; linear number
- **M** &rightarrow; number of features

### Examples
Examples of this framework are:
- LIME
- DeepLIFT
- layer-wise relevance propagation 
- classic Shapley value estimation
    - Shapley regression values
    - Shapley sampling values
    - Quantitative input influence
### Unique solution
There is a unique solution for these models if we want three properties to be fulfilled:
- local accuracy &rightarrow; f(x)=g(x')
- missingness &rightarrow; x_i = 0 &rArr; &phi;_i =0
- consistency &rightarrow; for any two explanation models *f* and *f'* then $$ \forall z' f'_x(z')-f'_x(z' \setminus i) \geq f_x(z')-f_x(z' \setminus i) \Rightarrow \phi_i(f',x)\geq \phi_i(f,x) $$

The solution to this method are the Shapley values.

With taking more assumptions such as feature independence or model linearity, the computation complexity can be lowered even further.
## Evaluation
It is shown that *SHAP* and *LIME* values can vary significantly due to the added consistency criteria.

The *SHAP* values often scored higher with AMT testers.

It was also shown that adding *SHAP* values to the *DeepLIFT* framework added more consistency to it.