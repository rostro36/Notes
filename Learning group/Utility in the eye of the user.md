# [Utility is in the Eye of the User: A Critique of NLP Leaderboards](https://arxiv.org/pdf/2009.13888.pdf)

## Introduction
Benchmarks are there to test how accurate models are and are doing that well. But these benchmarks often ignore fairness/robustness, energy efficiency and only care about rank, not even accuracy.
## Utility
In this context *utility* takes the meaning from economics and denotes the benefit a consumer receives from it. The look is especially on *cardinal utlity*, which gives the the currency *util* to a good, that can be compared across all goods.

**Leader-boards** are consumers (to make the comparison with practitioners possible) who only draw utility from accuracy, but we are not sure how much util we spend for rank.

**NLP practitioners** are also consumers, but are harder to model, as their resources and demands may differ, but also there cheaper, more accurate is better. Unlike leader-boards, they care about accuracy instead of the rank.

Resources can be considered by: 
- model size
- energy-efficiency
- training time
- inference latency

To get a good measurement of util, you can either make questionnaires (but whom do you send them to, with what exact questions) or check how much models are used (sometimes choices are a secret, practitioners also err).

# There is no way to model all practitioners accuratly, but transparancy of model resources can be demanded.
With this transparency, a practitioner could make weights his own leaderboard, based on more common leader-boards e.g. for accuracy or latency.

There exists a bias benchmark in [StereoSet](https://stereoset.mit.edu/) or a energy effort by [Green AI](https://arxiv.org/pdf/1907.10597.pdf).

You can hand-in multiple times to leader-boards, which makes overfitting possible.