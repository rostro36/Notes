# [BEHRT: Transformer for Electronic Health Records](https://www.nature.com/articles/s41598-020-62922-y)
## Data
There are two sets of data. The first one being [Clinical Practice Research Datalink](https://www.cprd.com/), which is a representative collection of data records from 674 health practices in the UK.
The second set, [HES](https://digital.nhs.uk/data-and-information/data-tools-and-services/data-services/hospital-episode-statistics), contains data on hospitalisations and admissions to the [NHS](https://www.nhs.uk/) in England.
The data used here only consists of the five 1.6 million patients that allowed for the link between the HES and the CPRD and that have at least 5 visits in their EHR.
The (ICD) diagnosis codes from the EHR are transformed to 301 [Caliber codes](https://www.caliberresearch.org/portal)
## Methods
### BERT input
BERT is used with:
- codes &rightarrow; words
- visits &rightarrow; sentences
- patients &rightarrow; documents

The input vectors then consist of:
- diagnosis code
- position in sentence
- patient age at the time of the visit
- visit segment to segment two visits
### BERT training
The BERT-standard masked language model is used to solve the Kleene task.
## Experiments
### Tasks
There are three experimental tasks:
- disease prediction at next visit
- disease prediction in the next 6 months
- disease prediction in the next year
### Results
The disease embeddings are already reasonably well clustered in the t-SNE plot according to the Caliber chapters. Also shown to a practitioner, he reassures that the proximities are sensible. The attention mechanism also highlights sensibly. There are also hardly any gender specific disease assigned to wrong sex.
BEHRT performs best in the disease prediction tasks compared to Deepr and RETAIN.

