# [Explainable automated coding of clinical notes using hierarchical label-wise attention networks and label embedding initialisation](https://www.sciencedirect.com/science/article/pii/S1532046421000575)

## Introduction
Automatic ICD-coding is very important and recent models often trade interpretability for  performance. Another limitation is that these models often assume independence among labels, ignoring the correlations.

To address these concerns the paper introduces a Hierarchical Label-wise Attention Network (HLAN), which uses a label embedding (LE) initialisation approach.

## Method
The document is encoded in two levels each with an own attention layer. The first layer takes each word of a sentence to consideration and the second gives attention to each sentence. All of these are code specific and the tokens are aggregated with a Bi-GRU.

Instead of the final state, an attended aggregation of all the hidden states is taken as the representation.

The standard way for prediction is to use a softmax on the document to get probabilities of the labels.
### Label embedding initialisation
A topic that is seldomly covered in literature is the weight initialisation as it is often just standard ones like Xavier or Kaiming. To strengthen the correlation between labels, the attention weights, both for the sentence and the word level, as well as the final softmax are initialised with the word2vec embedding of the labels.
## Experiments
MIMIC-III full, 50 and COVID-shielding was used.

The baselines were CAML, Bi-GRU,HAN (only sentence attention) and HA-GRU, BioBERT performed much worse.

HLAN performed similarly to the baselines,but gives even more precise interpretability, paying for that with compute power and time requirements, suggesting the model would be more suitable in a situation with only few codes (e.g. 50).

The label embedding initialisation mostly helped the models slightly. The more labels there are, the more important attention gets.
