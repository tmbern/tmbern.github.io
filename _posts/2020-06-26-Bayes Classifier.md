---
layout: post
title: Bayes Classifier
subtitle: Building a Bayes Classifier in Pure Python
tags: [Python, Probability, Classificiation]
comments: true
---

A quick intro to what the Naive Bayes Classifier is. Naive Bayes classifiers are simplistic classifiers that are used to predict or assign a class label to a dataset. The probability is derived from applying Bayes' theorem
\begin{equation}
\label{eq:bayes}
P(\theta|\textbf{D}) = P(\theta ) \frac{P(\textbf{D} |\theta)}{P(\textbf{D})} ~~~~~|| I,
\end{equation}

. Using the below table as a very simple example, we want to predict the gender of an individual based on their height and shoe size. 

| Height | Shoe Size | Gender | 
| :----- | :----- | :----- |
| 6'| 13.5 | M |
| 5' 3"| 7 | F |
| 5' 9| 11.5 | M |
| 5'5| 8.5 | F |
| 5'8| 9 | F |
