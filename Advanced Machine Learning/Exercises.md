# AML HS20 Exercises
## General
- In general the exercises do not get necessarily harder during the semester.
- Obviously I do not know how the exercises stand in relation to the final exam.
- These are my impressions of the exercises, if you have something you want to talk about, please notify me.
- TBD: Questions from the lecture
## Best of
I would advise you to at least read through all other exercises at least once and repeat the best-of exercises before the exam.
- 1.1. To get started again.
- 2.2&2.3. Very susceptible for an exam and not too bad of an exercise in general.
- 3.3. Maybe also 3.2.1&3.2.2. to repeat Taylor series and the Newton Method again.
- 4.3. To repeat kernels
- 5.2. High-level repetition of boosting and bagging.
- 7.1.1&7.1.2 If neural network backpropagation is not already safe
- 8.3 Covers most about PAC-learning

### To get some maths training:
- 1.3 EM-Algorithm, check the notes
- 5.1 Lagrangian repetition
- (4.2.) As the least important to repeat Fisher's LDA
## Exercise sheet 1
1. (Regression) 
	- Good exercises to start and revise some basic concepts without books
1. (Comprehension Questions)
	- Good, fast exercises to repeat the basic concepts of overfitting, can be skipped if overfitting is clear
1. (EM-Algorithm for Gaussian Mixtures)
	- Harder than the previous and an upper limit of the mathematical complexity that I expect at the final exam; if this question can be solved, then I am confident in the derivations on the exam
	- <details><summary>Hints 3.</summary>
	
		The solution to 3. also uses similar tricks as the [cross-entropy loss](https://en.wikipedia.org/wiki/Cross_entropy)!
		
		</details>
   - Check the solution after step 3, as it makes the whole problem much easier and the step is not trivial!
   - 6 is hard, such that it may not work in the first try, but maybe read the solution and solve it the next day again.
## Exercise sheet 2
As said in the tutorial session, use the [matrix cookbook](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf) for the matrix derivatives and definitions of certain distributions.

Problem 2, 3 and 4 use the technique of "completing the triangle", that I can see at the exam as the nightmare of everybody, that has not done the exercises, as the whole series was about that. 
1. (MLE for Gaussians)
	- All the averages should be easy to to compute, the STD's are also with the matrix cookbook hard. 
	- For the STD, read the solutions on one day and try to solve the exercise on the next day.
1. (Conditioning a Gaussian)
	- Important, gives the basics for "completing  the triangle"
1. (Bayesian Regression)
	- Important application of the previous basics, should not be too hard with that knowledge
1. (Intro to Prediction in Gaussian Processes)
	- This should not be hard, but I am too dumb/should spend more time.

## Exercise sheet 3
1. (Gaussian Process Kernels)
	- Checks the properties of kernels, very likely to also show up at an exam in some form.	
	1. For some questions, we need the property, that a [Gramian Matrix](https://en.wikipedia.org/wiki/Gramian_matrix) is a valid kernel.
	1. I am not too pleased about the exercise questions, just play with the [source website](https://www.jgoertler.com/visual-exploration-gaussian-processes/) for like 5 minutes.
	1. I am not too pleased about the exercise questions, just play with the [source website](https://www.jgoertler.com/visual-exploration-gaussian-processes/) for like 5 minutes.
1. (Newton Method)
	- Also likely to occur in some form, especially the very first question.
	- 2, is not hard, once the solution is known. Should be in the repertoire at the exam.
	- 3, Too complicated for me.
1. (Gradient Descent)
	- Important and doable.
## Exercise sheet 4
1. (Perceptrons)
	- Easy exercises a) only takes two iterations if done right.
1. (Fisher's Linear Discriminant Analysis)
	- Quite a bit of work, but should be possible
	- <details>
		<summary>Hint</summary>
		Derive w_o first and use the result when deriving w. The exercise is not that hard.
		</details>
1. (Warm-up: Kernel Function)
	- Checks the properties of kernels, very likely to also show up at an exam in some form.	
	- Remember, a [Gramian Matrix](https://en.wikipedia.org/wiki/Gramian_matrix) is a valid kernel.
1. (SVMs as Nearest Neighbor Classifiers)
	- Give the unique nearest neighbour a name
	- <details><summary>Hint</summary>
		Show that the influence of the nearest neighbour is bigger then the influence of all the other points, for some h_0.
		</details>
## Exercise sheet 5
1. (Structural SVMs)
	- Not easy, but should be able to solve at exam. You can check what the Lagrangian is and start from there.
1. (Ensemble Methods)
	- High-level questions about ensemble methods, usable for repetition
1. (Bagging)
	- High-level question again, solve once and you should know it from then.
1. (Boosting)
	- Simply puts in the function definitions into the algorithm. Once boosting is understood, probably not needed any more.
## Exercise sheet 6
1. (AdaBoost and forward stagewise additive modeling)
	- Pretty hard exercise, I could not solve any exercise without hints
	-  <details><summary>Hints 1</summary>
		What does the expectation mean? Write it out and the solution can be derived.
		</details>
	- I could not solve 2, if you don't succeed, look at solution
	- <details><summary>Hints 3</summary>
		The solution is easier than the previous exercises.
		Simplify the solution first, then derive the loss and make a case distinction.
		</details>
	- <details><summary>Hints 4</summary>
		Simplify a bit and then case distinction on I.
		</details>
1. (Activation functions in Neural Networks)
	- Depending on how good the solution should be, this question can be quite easy. I would recommend to solve it, don't try to simplify and look at the solutions.
	- <details><summary>Hints</summary>
		Start with the activation and remember to adjust the y and the activation in the loss.
		</details>
1. (Regularization in DNNs)
	- Interesting high level questions about DNNs, solve once and know it for the rest.
1. (Convergence of the Robbins-Monro algorithm)
	- Interesting background, okay exercise.
	- 1, you can keep &theta;* as is.
	- 3, write &theta;^k in terms of &theta;^1, the rest of the exercise is then just the same as exercise 2 and not worth the time.
	- 4, take the solution from 3, it is only expected to bound the first half of that term.
	- 6, a handwavy argument is good enough, finally fill in the assumptions on &eta;
	- 7, this is one sentence
## Exercise sheet 7
1. (Backpropagation)
	- Standard backpropagation of a neural net, you should definitely know how to do that.(although I am still shaky)
	- Don't bother with writing too much/setting the functions inside etc.
	- <details><summary>Simplification of Sigmoid derivation</summary>
		sig'(x)=sig(x)(1-sig(x))
		</details>
1. (Cluster quality evaluation)
	- Not relevant for exam, solvable, but I don't like the solutions, [here](https://github.com/rostro36/Notes/blob/master/Advanced%20Machine%20Learning/Solutions_sheet_7_2.pdf) are mine.
	- <details><summary>Hints 2</summary>
		You have to show both inequalities, I being bigger-equal 0 is easier the comparison with H.
		For the first inequality, you should use Jensen's inequality that was used in the alternate proof of the [Gibbs inequality](https://en.wikipedia.org/wiki/Gibbs%27_inequality)
		Try to rewrite I as H for the higher bound and use that the union of the U's form X.
		</details>
	- 3, Depending on the edge case from 1. this can be impossible to be found, make your thoughts and then read the solution.
1. (Dirichlet process)
	- I don't like that exercise, stop when you have a tractable sum that you can put into [WolframAlpha](https://www.wolframalpha.com/), the computation of the sum is too complicated for me.
## Exercise sheet 8
All exercises are proofs of PAC learning and therefore interesting. With the amount of information, we already have, most of the exercises should be rather easy, so don't be scared of the latin letters.
1. (Empirical Risk Minimizer)
	- Depending how exact one wants to work it gets a bit more complicated, but the exercise is easily solvable and is a essential proof for PAC learning.
1. (PAC Learnability)
	- Decent exercise, some simple inequalities lead to the solution.
1. (Learning Concentric Circles)
	- Hardest, but because of that also best exercise for me. Still not hard.
	- 1, Mistake in exercise sheet: it should be P(r_min **<** r_epsilon)=(1-epsilon)^n
		First If you don't understand r_epsilon, maybe check the exercise sessions or the solutions.
	- I checked the solutions for 1 and with 3 it clicked for me.
	