# [GPT-2](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)
## General
Before ML systems can be characterized as narrow experts of tasks and topics, for each needed a large dataset and training, rather than competent generalists, which should not need as much data and time for a new task.

One distinguishes between the general pre-training which takes hours or days and the task-specific fine-tuning, which takes minutes or hours.
## Approach
A general system should model p(output|input, task), many problems can be written as (task, input, answer), in the case of NLP, many of such tasks get answered in text themselves e.g. the translation of a word stands next to the original word, in a unsupervised fashion. If the corpus is large enough, this could be used and is known as *zero-shot* learning, because we never (explicitly) trained the model on this task.
## Dataset
A general language model should be trained on a general dataset, one promising source for that are crawled datasets, but quality can be a big issue, therefore the researchers created a new dataset which scraped outbound links from Reddit, which received at least three (probably human) upvotes, which would not upvote if they could not understand it.

The collection was finished in December 2017 and after de-duplication and cleaning had about 8 million articles and 40GB of text and afterwards lower-cased, tokenized, byte pair encoded (an encoding between character and word level e.g. *th*) and out-of-vocabulary tokens were removed.

## Model
A Transformer based architecture was used with a vocabulary size of 50'257, context size of 1024 and a batch-size of 512.

12 layers correspond to 117 million parameters and 768 and a dimensional model. Roughly every 12 layers, the parameter space doubles, while the dimensions grow much slower, about 250 every 12 layers.
## Experiments
GPT has the most problems with a very large dataset and having destructive pre-processing.

GPT is state-of-the-art in a children's book cloze test and finding the end of a sentence, BERT is better at (extractive) question answering, but only slightly and many bad answers are "answer to the question".

For summarizing, GPT-2 focusses too much on non-relevant information, such as the label on a hat and has a bias towards more recent text. Also adding a suffix helps the task.

For translation GPT-2 is worse than state-of-the-art (and according dataset), with translating word for word to French being better, but the other way round, coming from French to English, GPT-2 performs better.

## Generalization vs Memorization
With those kind of large and "blindly" collected datasets it is important to make sure, that the training and test set do not overlap, to make sure the model generalizes and not just simply memorize. For this the researches propose Bloom filters of 8-grams and find out, that there is more overlap between training and test set of the different tests compared to the test set and the pre-train dataset, with average overlap being somewhere around 6%.