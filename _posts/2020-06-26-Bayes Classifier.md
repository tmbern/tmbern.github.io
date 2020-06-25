---
layout: post
title: Bayes Classifier
subtitle: Building a Bayes Classifier in Pure Python
tags: [Python, Probability, Classificiation]
comments: true
---

A quick intro to what the Naive Bayes Classifier is. Naive Bayes classifiers are simplistic classifiers that are used to predict or assign a class label to a dataset. The probability is derived from applying Bayes' theorem.


$$ P(A \mid B) = \frac{P(B \mid A) \, P(A)}{P(B)} $$

Wikipedia has a  [great intuitive explanation](https://simple.wikipedia.org/wiki/Bayes%27_theorem#:~:text=From%20Wikipedia%2C%20the%20free%20encyclopedia,that%20evidence%20given%20the%20hypothesis.) for the above equation So i will let them explain. 

To calculate the probability of it having rained on Sunday, given that it rained on Monday, we can take the following steps:

* We know that it rained on Monday. Therefore, the total probability is P(B).
* The probability it rained on Sunday is P(A).
* The probability it rained on Monday, given that it rained on Sunday is P(B|A).
* The probability of raining on Sunday AND raining Monday is P(A)*P(B|A).
* Therefore, the total probability of it having rained on Sunday, given that it rained on Monday, is the chance of it raining on Sunday and Monday divided by the total chance of it having rained on Monday.



