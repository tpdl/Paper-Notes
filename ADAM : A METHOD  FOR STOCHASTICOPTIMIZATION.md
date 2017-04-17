# Paper summary:

## *ADAM : A METHOD  FOR STOCHASTICOPTIMIZATION*


We propose
Adam
, a method for efficient stochastic optimization that only requires first-order gradients with little memory requirement. The method computes individual adaptive learning rates for
different parameters from estimates of first and second moments of the gradients; Some of Adam’s advantages are that the magnitudes of parameter updates are invariant to
rescaling of the gradient, its stepsizes are approximately bounded by the stepsize hyperparameter,
it does not require a stationary objective, it works with sparse gradients, and it naturally performs a
form of step size annealing.the name
Adam
is derived from adaptive moment estimation.

[Comparison of various optimization models](http://colinraffel.com/wiki/stochastic_optimization_techniques)




