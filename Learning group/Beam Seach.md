# [If Beam Search is the Answer, What was the Question?](https://arxiv.org/pdf/2010.02650.pdf)
## General
Regularized decoding:
![$$Y*=argmax_{y\in language, |Y|=k}(log(p_\theta(Y|x)-\lambda R(Y)))]()

Surprise (smallest surprise means highest probability):
![$$u_t(y)=-log(p_\theta(y|x,<_{<t}))$$]()
## Maximum a posteriori (MAP)
MAP can be expressed in the formula of regularized decoding with &lambda;=0
![$$Y*=argmax_{Y\in language}log (\Pi_{t=1}^{|y|}p_\theta(y_t|x,y_{<t}))$$]()
 should generate good results, because it is exact, but a regularized beam search performs often better (e.g. in machine translation) and is cheaper too. But since MAP delivers the definitive answer for the most probable string, what does beam search deliver that makes it better than MAP for these tasks?
## Beam search
Beam search expressed in the formula of regularized decoding with &lambda; going into infinity:
![$$Y_t=argmax_{Y'\in B_t,|Y'|=k} log(p_\theta(Y'|x))$$]()
With B being the candidate set which gets updated by the best *k* candidates at each time step:
![$$B_t=\{y_{t-1}concat\;y| y \in Vocabulary \cap y_{t-1}\in Y_{t-1}\}$$]()
And the choosing strategy:
![$$R_{beam}(Y)=\Sigma_{t=1}^{n_{max}}(u_t(Y_t)-min_{Y'\in B_t, |Y'|=k}u_t(Y'))^2]()

## MAP vs beam search
One big difference between MAP and beam search is that beam search constantly searches for the smallest surprisal at each word while MAP optimizes globally and is happy to take a hit at some word while being able to use a much smaller surprisal later on or even shorten the sentence. 

In practice, increasing the beam size beyond 5 can hurt model performance in terms of downstream evaluation metrics, while MAP often returns small strings of (too) common words, which can be countered a bit by rewarding a length of the sentence and rewarding recycling of a word from the context.
## Uniform information density (UID)
The UID hypothesis states that humans prefer sentences that distribute information over the whole sentence evenly and do not like it if a word holds two pieces of information, where others hold none.

UID only works if grammaticality and information content are held constant, which we assume to be true, because NLP systems are now generally grammatically correct and the given context constraints the information content well enough.

Using UID it can be explained why MAP performs worse in translation, because of the taking a hit and hitting back harder behaviour of the global optimization. The larger the beam, the more this effect can also come up. 
## Regularising on UID
UID may used to regularise by setting the choosing strategy (R) as minimizing: 
- the overall variance of the surprisals 
- local surprisal changes between neighbouring words
- the maximal surprisal
- squared penalty on all surprisals

## Experiments
### Setup
To test the hypothesis [NMT systems](https://en.wikipedia.org/wiki/Neural_machine_translation) were trained and the [BLEU](https://en.wikipedia.org/wiki/BLEU) score was used. The datasets used for the experiments are from the [fairseq NMT repository](https://github.com/pytorch/fairseq/tree/master/examples/translation). All scores are bounded by the maximum-length criterion.
### Results
With UID regularisation, the beam sizes hardly degenerate with bigger search size as opposed to the greedy beam search algorithm.

Also variance and local surprisal changes perform worse than the other two, as they inherently enforce small surprisals.

Combining with the MAP objective gives a benefit, but combining the different UID regularisers hardly increases performance.