# [ Learning a Health Knowledge Graph from Electronic Medical Records ](https://www.nature.com/articles/s41598-017-05778)
## Introduction
The need for good knowledge bases is high, but they are hard to create. This paper tries to do create a disease-symptoms knowledge base by mining EHR data.

But the EHR data has to deal with some problems:
- the text is written by medical professionals, not textbook writers
- those are real cases, not textbook disease progressions
- there are only co-occurences, no causation indication
- the annotator may has deemed some for the model important circumstances as unimportant for the record
## Data
The data consists of 273 174 patient visits that consist of NLP notes and ICD-9 codes. UMLS was used as a thesaurus to map the ICD-9 codes to the Google Health Knowledge Graph \(GHKG\). Also simple negation correction was used.

The GHKG is provided by data mining and clinical help, which focussed on correctness of edges rather than completeness. Diseases which were seen at least 100 times and symptoms which were seen at least 10 times were considered. The nodes were also annotated with a binned frequency, which was additionally separated by binned age categories.
## Knowledge graph construction
There are three parts in constructing a knowledge graph:
- extracting the mentions \(as done above\)
- learn statistical models between diseases and symptoms
- translate the learnt model into a knowledge graph

### Statistical models
Three statistical models were used and translated to graphs or graph edges:
- **naive Bayes** &rightarrow; learn a model for each disease and use ML estimator; take a disease, if given a symptom, the probability of the disease being there is bigger than it not occurring 
- **logistic regression** &rightarrow; learn a model for each disease and use ML estimator; take a symptom if the weight for that disease is positive
- **Bayesian network with noisy OR gates** &rightarrow; this model assumes a disease causes a symptom with probability *f* and the symptom gets activated without the disease with probability *l*
	- the chance of the symptom given the diseases is therefore: P(*x_i*|disease vector)=1-(1-*l*)&Pi;_{disease in diseases} *f*^{disease}
	- the importance of the edge is then 1-*f*
	- the model is learnt jointly and therefore takes disease interactions into account
## Evaluation
The naive Bayes and Bayesian network both perform much better than logistic regression and slightly better than GHKG, according to surveyed medical professionals. Bayesian network performs statistically significantly better than naive Bayes.

The model finds some edges GHKG did not have and excelled at rarely happening, yet relevant symptoms. But the model was trained on more severe cases than the more general GHKG.