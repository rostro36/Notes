# [Causability and explainability of AI in medicine](https://wires.onlinelibrary.wiley.com/doi/full/10.1002/widm.1312)
## Overview causability
Explainable medicine needs **causability**.

*Causability* is a property of a person, while *explainability* is a property of a system.

Medicine is a hard domain as there is a lot of uncertainty and there are datasets, which are in many ways suboptimal, while there are very high stakes.

Labelled data is hard to come by with busy expert annotations, large file size, small training data available, inballanced classes etc.

Humans often also can't clearly explain their decisions and *just see* things.
## Three kinds of explanations
There are three kinds of explanations based on who should be the recipient and user:
- **peer-to-peer explanation** &rightarrow; from physicians for reporting, the focus of this paper
- **educational explanation** &rightarrow; from teacher to student
- **strict scientific explanation** &rightarrow; science theory with logic etc.

What is physically understandable for a human &rightarrow; data in R^3 or lower, not abstract spaces like word embeddings or non-understandable symbols like elfish

## Two problems often faced in machine learning
There are two different problems listed that can be encountered when doing medical machine learning.
1. Ground truth can't always be well defined, especially when making medical diagnosis.
2. Unlike ML models, human models/people are often based on causality as the ultimate goal, with correlation only as an intermediate step.

## Definition of causability
- *Explainability* &rightarrow; highlights decision relevant parts of the model either for training or an inference example
- *Causability* &rightarrow; an explanation of a statement achieves a specific level of causal understanding and satisfaction in a human
Explanation feeds into an explanation interface. This interface is used by an user, when causability can be achieved.

## Explainability techniques
There are two general types of interpretable approaches:
- *post-hoc* &rightarrow; make explanation after training like *LIME*
- *ante-hoc* &rightarrow; during training with an interpretable model like a decision tree

## Different levels of explanation
- *Uncertainty* &rightarrow; small peturbations change models much
- *Attribution* &rightarrow; link output variables to input variables
- *Activation maximation* &rightarrow; produce synthetic data (like an image) that maxes out an activation prototype/expert
