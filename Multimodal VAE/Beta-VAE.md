# [&beta;-VAE: learning basic visual concepts with a constrained variational framework](https://openreview.net/pdf?id=Sy2fzU9gl)
## Introduction
Representations that suit the particular domain and task well can improve the learning success and robustness of the chosen model, also transfer learning might be (easier) achievable.

One sign of a good representation is being **disentangled**, where the change in one real factor (e.g. colour of the object) only affects one latent variable and hardly the others.

Independent latents are not necessarily disentangled, as they may lack interpretability.

Before this paper the only model that enforces disentanglement was a GAN, which are inherently hard/unstable to train, even when one knows a good prior. Also there was no measurement of disentangled before.

## Deriving the &beta;-VAE
We want to optimise the probability of the data:

![max\[E\_{*x*~*D*}\[E\_{q\_&phi;\(*z*|*x*\)\}\[log(p\_&theta;(*x*|*z*)\]\]\]](https://latex.codecogs.com/gif.latex?%5Cmax%5C%5BE_%7Bx%5Capprox%20D%7D%5BE_%7Bq_%5Cphi%28z%7Cx%29%7D%5Blog%28p_%5Ctheta%28x%7Cz%29%5D%5D%5D)
 
With the additional constraint that q\_&phi;\(*z*|*x*\) is close to p\(*z*\), which is checked with the KL-divergence: D\_KL\(q_&phi;\(*z*|*x*\)||p\(*z*\)\) being smaller than some &epsilon;

Using this constraint as a Lagrangian under the [KKT conditions](https://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions):

![F\(&theta;,&phi;,&beta;,*x*,*z*\)=E\_{q\_&phi;\(*z*|*x*\)\}\[log\(p_&theta;\(*x*|*z*\)\]-&beta;\(D\_KL(q\_&phi;\(*z*|*x*\)||p\(*z*\)\)-&epsilon;\)](https://latex.codecogs.com/gif.latex?F%5C%28%5Ctheta%2C%5Cphi%2C%5Cbeta%2Cx%2Cz%5C%29%3DE_%7Bq_%5Cphi%5C%28z%7Cx%5C%29%7D%5C%5Blog%5C%28p_%5Ctheta%5C%28x%7Cz%5C%29%5C%5D-%5Cbeta%5C%28D_%7BKL%7D%28q_%5Cphi%5C%28z%7Cx%5C%29%7C%7Cp%5C%28z%5C%29%5C%29-%5Cepsilon%5C%29),

where the last &epsilon; can be omitted as we only need a lower bound.
## Disentanglement metric
We assume to have a set of images *X*, the independent data generative factors *V* and some remaining factors *W*.

1. Choose *y*~Unif\[1..*K*\], that is the factor that will be kept fixed
1. For a batch of *L* samples:
	1. Sample two sets of representations *v^a* and *v^b*, where the *y*-th component is equal in both of them 
	1. Generate images (*x*) based on the representations *v* and then encode those images again to get *z* 
	1. Compute the absolute \(Manhattan\) distance between *z^a* and *z^b* as *z\_\{diff\}*
1. report the **disentanglement metric score**, which is the accuracy of a simple predictor given ![*z_\{diff\}^b*=1\/*L*&Sigma;\_\{*l*=1\}^L *z\_\{diff\}^l*](https://latex.codecogs.com/gif.latex?z_%7Bdiff%7D%5Eb%3D%5Cfrac%7B1%7D%7BL%7D%20%5CSigma%20_%7Bl%3D1%7D%5EL%20z_%7Bdiff%7D%5El) in predicting p\(*y*|*z_\{diff\}^b*\)

## Experiments
celebA, chairs and faces are used. The competition is the normal VAE, InforGAN and the semi-supervised DC-IGN.

The &beta;-VAE could generate some images, with a feature combination which have not been seen before; like an armchair with a round office chair base.

Overall, &beta;-VAE tends to discover more latent factors and learn cleaner disentangled representations.

The higher &beta; the better the disentanglement, but the picture gets blurrier and small changes get lost.

Higher latent layer size require higher &beta;

The most informative latent units *z\_m* of β-VAE have the highest KL divergence from the unit Gaussian prior \(p\(*z*\)=N\(0,I\)\), while the uninformative latents have KL divergence close to zero.