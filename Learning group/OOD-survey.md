# [Generalized Out-of-Distribution Detection: A Survey](https://arxiv.org/abs/2110.11334)
## Overview
Out-of-distribution (OOD) is critical in keeping the reliability of a model up to date. Like if the data changes over time, then the model should notify the user that there has been a shift.

There are two kinds of shifts:
- *covariate shift* &rightarrow; different **input** &rightarrow; OOD examples from a different domain e.g. pictures vs. painting
- *semantic shift* &rightarrow; different **label** &rightarrow; OOD examples from new/different class e.g. cat in dog pictures

## Different subtasks
There are many subtasks for OOD:
- *anomaly detection* &rightarrow; only one shift
  -  *sensory AD* &rightarrow; only covariate shift
  -  *semantic AD* &rightarrow; only semantic shift
- *novelty detection* &rightarrow; only semantic shift, new classes are a learning opportunity, many classes during training possible
- *open set recognition* &rightarrow; like novelty detection, but wants a classification into previously known classes
- *OOD detection* &rightarrow; novelty detection+ open set recognition
- *outlier detection* &rightarrow; both shifts possible, but no labels available

### Anomaly detection
E.g. get inputs and decide if a new input is also of the same class.
There are two types of techniques:
- unsupervised &rightarrow; all samples are positive samples
- (semi-)supervised &rightarrow; we have (partial) labels
Usually a confidence score is computed for each class.

### Novelty detection
Two subclasses based on the amount of classes during training. But for both multi-class and one-class ND the main goal is to find out if a test sample belongs to a new, never seen class or one of the previous classes.
### Open set recognition
Open set recognition is similar to novelty detection, but also expects that the model can classify in-distribution samples correctly additionally to finding OOD samples.

### Outlier detection
Process training- and test-set at the same time and not in the typical tran-test approach. Approaches are rather transductive compared to being inductive. Either due to semantic or covariate shift. This task is often needed to clean a dataset.

## Methods to solve these tasks
Methods to solve these tasks:
- density based &rightarrow; *ID* data is likely, *OOD* data is unlikely
- reconstruction based &rightarrow; for a *VAE*, the encoding and decoding of *ID* data usually works quite well, while it works worse for *OOD* data
- classification based &rightarrow; normal classification task where the negative samples have to be tricked
- distance based &rightarrow; e.g. k-nearest neighbours or # in radius
- gradient based &rightarrow; classify the gradients going through a model

### Density based methods
Fit multivariate distribution (like a Gaussian or a histogram) to *ID* samples and compute Mahalanobis distance between the test and the trained distribution.

This easy method work well for low-dimensional problems, but suffer e.g. for images under the curse of dimensionality. The quick fix for this problem is to instead of using the image as is, using the VAE lower-dimensional representation of the image.
### Classification based methods
One class classification only learns from one class as opposed from the usual two and finds decision boundaries on it's own just for this class.

Another kind of classification based method uses a positive-unlabelled dataset, where there are some positive labels and a bunch of unlabelled cases. To construct negative samples, the unlabelled cases are either clustered and the outliers are seen as negatives or the whole unlabelled dataset is seen as a noisy negative set.

Another way of solving the OOD problem is using surrogate tasks, such as next frame prediction for video. Where *OOD* data should perform much worse compared to *ID* data.

Systems such as ODIN can take confidences/probabilities in different classes and inverse the softmax to get a confidence to an OOD class.

OpenMax instead of SoftMax also gives the OOD class some probability.
### Reconstruction based
A nifty trick is to get a mix of *random* classes as negative samples.

## Evaluation metrics
For all tasks AUROC, AUPRC and F-scores are an important evaluation metric. Especially for open set recognition as well as for outlier detection CCR@FPR_x or similar is also used often, which computes a class-wise *recall*/*precision* at certain *FPR*/*TNR* thresholds.

