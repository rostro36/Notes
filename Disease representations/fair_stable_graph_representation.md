# [Towards a Unified Framework for Fair and Stable Graph Representation](https://arxiv.org/abs/2102.13186)
Graphs get used more and more in the real world and also for high-stakes settings, such as medical applications or also in criminology. In both cases it is important that the representations are fair towards sensitive features such as gender or race. Also stability is important to be used in practice.

The message passing frame work of GNNs usually promotes biases even more than normal FNNs, so this paper introduces *NIFTY* that increases both: stability **and** fairness.

## Types of fairness
1. **Group fairness** &rightarrow; minority group gets the same treatment as the majority group
1. **Individual fairness** &rightarrow; similar individuals get similar treatment
1. **Counterfactual fairness** &rightarrow; decision is fair if changing the sensitive feature does not change decision

## Stability by Lipschitz continuity
Small pertubations (tilde) should not drastically change representation (*b*) of node *u*

||ENC(\tilde{U})- ENC(U)||_p \leq L||\tilde{b}_U-b_u||_p

## NIFTY
1. Augment graph
 - Chance of 1-p_e to drop edge
 - \tilde{X_u} =X_u+r \ocircle \delta
  - With r from Bernoulli
  - \delta from Normal distribution
2. Change update rule
- h_u^k=\delta(\tilde{w}_a^k h_u^{k-1}+W_a^k\sum_{v\in Neigh(u)} h_v^{k-1})
3. Train siamese network with augmented loss
L_S = \mathds{E}_u(\frac{1}{2}(D(t(z_u)),sg(\tilde{z_u}))+D(t(\tilde{z_u})),sg(z_u)))
- t &rightarrow; predictor FNN to match representation
- D &rightarrow; cosine similarity
- sg &rightarrow; stop-grad
Loss = \mathds{E}_v \[(1-\lambda)L_C]+\lambda L_S
Where L_C is the classification loss
## Scoring functions
### Performance scores
The known AUROC and F1 scores
### Fairness scores
- Group fairness &rightarrow; statistical parity &rigtharrow;
- Equal opportunity &rightarrow;
- Counterfactual fairness &rightarrow; glipped labels if sensitive attribute is flipped
### Stability score
- Instability score &rightarrow; flipped labels if random noise is added
## Results
Different GNN architectures were tested. On one hand vanilla and the other variant with *NIFTY*. Also previous methods like NIFTY are used to compare. Those other metheds only optimise to stability or fairness, but not both.

The performance of the *NIFTY* variants is worse, but only slightly. *NIFTY* gives the worst performance compared to the other stability/fairness methods, but has the best values for stability and fairness.
### Ablation
The architecture changes of *NIFTY* lead to worse performance, but better stability. The objective function exaggerates this effect further.
