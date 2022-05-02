---
layout: post
title:  "Cramer-Rao LFisher Information - Some Intuition"
date:   2021-06-20 00:15:08 +1000
categories: jekyll update
---

TEST PUSH

Fisher Information is one of those topics that many will cover once in their studies on the way to more applied areas of statistics. The practical use of the CRLB is limited to cases where the sample at hand has a known distribution with an analytic form, which often doesn't apply.

These posts will take a brief look at the Fisher Information and the CRLB from a less formal point of view to gain some intuitive sense of what they are doing.

Score

The score can be calculated as:

$$ V\left( X,\theta \right) =\dfrac{\partial }{\partial \theta }\ln L\left( X,\theta \right) $$

$$ x=y+z $$

A good way of thinking about score is as sensitivity of the log-likelihood to theta. A natural question about score is why it is calculated using log-likelihood and not just likelihood. One answer is that it often makes the calculation easier. Consider the example:...

Come up with a graph in R
demonstrating that where theta = mle, v = 0

Fisher information

How to demonstrate the score?
Generate samples from two normal distributions with different variances and the same mean, show graph of
log likelihood, score, and score squared
Consider calculating the score of two different distributions. One has
Strictly, Fisher Information is just the variance of score with respect to theta. If the score is very
However, Fisher Information has some interesting properties.

Part 1- Definition of Fisher information

Part 2- An Alternative Approach to Fisher information

Part 3 - Fisher Information, Sample Size and CRLB
