# [GAMENet: Graph Augmented MEmory Networks for Recommending Medication Combination](https://arxiv.org/abs/1809.01852)
## Introduction
GAMENet incorporates drug-drug interaction \(DDI\) knowledge to advise patients, modelled as recurring visits, which are electronic health records \(EHR\), against certain drugs which might not interact well together.

The medications are one-hot-encoded.
## Model architecture
GAMENet consists of many modules:
- patient embedding
- graph augmented memory module
- output sigmoid
- loss
- training
### Patient embedding
The multi-hot encoded visits are each embedded using a simple matrix to a lower dimension. The visits then get aggregated together using a Dual-RNN, one for diagnosis and one for procedures \(cures\) to give a patient embedding in the last hidden state.

### Graph augmented memory module 
There are four memory components:
- **I**nput memory representation &rightarrow; transforms the hidden states to a query using an MLP.
- **G**eneralisation &rightarrow; generating facts\/knowledge
	- **Memory Bank** information is computed using two layers of GCN, where only the nodes with patient conditions\/procedures are are activated at the start. Both parts are again learnt separately and the graph representations are then combined using a hyperparameter &beta;.
	- **Dynamic Memory** is a mapping from the keys to the multi-hot vectors
- **O**utput memory representation &rightarrow; generates two outputs
	- one from the memory bank, with the query also attended on the memory bank matrix &rightarrow; static knowledge
	- the second from dynamic memory attended in two steps, first attention on the Dynamic Memory, where the query acts as query, which is then used in attending on the Memory Bank again. &rightarrow; temporal knowledge
- **R**esponse &rightarrow; put the query, and the two outputs inside a sigmoid and then return all probabilities of all cures.
### Loss
For the medication combination loss, both [binary cross entropy](https://towardsdatascience.com/understanding-binary-cross-entropy-log-loss-a-visual-explanation-a3ac6025181a) on the label and the [multi-label margin loss](https://pytorch.org/docs/stable/generated/torch.nn.MultiLabelMarginLoss.html) were used and added together weighted with hyperparameter &pi;.

The second part of the loss is to enforce the DDI information. This is done with elementwise multiplication of the adjacency matrix of the DDI and the dot product of the response.
### Training
The model is trained with two losses, which depends on the DDI rate. If the DDI rate is below a threshold, then the medication combination loss is used. If the DDI rate is above the threshold, then with a probability, which is proportional to the overshot, the DDI loss gets used, else the medication combination loss is used.
## Experiments
The recurring patients in MIMIC-III were used for EHR and TWOSIDES was used for DDI. The model has about the same parameter size as the baselines and it could be seen, that the recurring part is very important. The DDI rate is better than the EHR and the precision is the best compared to the baselines.

GAMENet can give a trade-off between effectiveness and DDI, where it breaks the DDI criteria for higher effectiveness.