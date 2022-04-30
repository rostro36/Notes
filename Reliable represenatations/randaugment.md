# [RandAugment: Practical automated data augmentation with a reduced search space](https://arxiv.org/abs/1909.13719)
## Overview
Many other research projects have already shown that data augmentation is important to increase the performance and also make the model more stable.

This data augmentation is also a big topic in other research papers, many of those have a separate training loop to find the best augmentations. Since this training can be very costly, it is often done with a proxy task, which is either/or a smaller model or a smaller dataset.

This paper argues that these proxy tasks are not good enough and propose a simpler method for data augmentation that does not need a complicated training loop and can be done in a grid search.
## RandAugment
**RandAugment** uses 14 different off the shelf augmentations and uses them with a probability of 
$$\frac{1}{N}$$
This order can also be shuffled.

These augmentations often need a parameter, which are bundled in a magnitude *M*, such as the level of shear or the complexity of a rotation. This magnitude can be used for all augmentations and spans from 1-10.

To train the model, only a grid search is performed over *N* and *M*; in the experiments they used about 3 choices each.
## Experiments
### Setting
For the experiments (Wide-)ResNet was used on the image datasets CIFAR, CIFAR-10, SVHN, ImageNet and COCO, which gives a rather large disparity of data. For all of those tasks, RandAugment achieved state-of-the-art or near state-of-the-art performance, with a comparatively small amount of training and complexity.
### Proxy tasks
It was shown that proxy tasks do not properly work. Proxy tasks on smaller datasets and models seem to favour lower orders of magnitude compared to the full task. The grid search over all possibilities does check for this eventuality.
### Influence of different augmentations
Unsurprisingly, different augmentations have different effect on the following task. In the tests conducted in this paper, it was shown that geometric augmentations, such as *rotation* or *shear*, perform better than colour augmentations, such as *gray scale*. It was shown that *rotation* was the best performing augmentation, while *posterize* even makes the performance worse. 
### Different augmentation frequency
Given the results from the point above, it comes natural, that different augmentations should have different frequencies (e.g. *posterize* should have no frequency). This gave a slightly better performance at the cost of a significant increase of training resources.
### Magnitude change
During training, the magnitude should be increased to make the later augments harder than the first ones, as the model has already learnt something. This is something that RandAugment can provide.