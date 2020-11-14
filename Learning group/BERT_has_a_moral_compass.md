# [BERT has a Moral Compass](https://arxiv.org/pdf/1912.05238.pdf)
## General
Ultimately, if AI systems carry out choices, then they implicitly make ethical and even moral choices e.g. self-driving cars should know where to crash into. But can AI learn human ethical judgements?

Human language reflects biases e.g. the right party and language models might learn those biases.

## Embeddings
The authors follow the pipeline of [Jentzsch et al](https://ml-research.github.io/papers/jentzsch2019aies_moralChoiceMachine.pdf), which can use **Sentence-BERT** or Universal Sentence Encoder (USE) embeddings and analyse those.

Bias(question Q, choice A, choice B)= cos(Q, A) - cos(Q,B)

If the bias is positive it is leaning more to A, if the bias is negative it leans more to B.

They ask questions such as "Should I ..?" and replace the dots with an action e.g. "murder someone" and as answer choices "Yes, you should." or "No, you should not.".

## Moral subspace

To rank the "morality" of an embedding according to [Bolukbasi](https://arxiv.org/abs/1607.06520) one first has to find the "moral subspace" that captures that bias. To do so, they propose to take **pre-labelled** antonyms on opposite sides of the moral spectrum and compute its principal component and test it with eigenvalue analysis.

In this paper, the authors compute PCA on selected sentences and expect that these actions represent the moral subspace.

Dos and Dont's have been extracted on the Google Slim embedding and are only about 30.

## Evaluation
Correlation of Do/Don't score to WEAT values is measured with Pearson. USE has a correlation of r=0.73 and BERT has one of 0.88.

One can not project into a moral subspace with more complex sentence construction with USE, BERT also gets worse, but still gives usable predictions.