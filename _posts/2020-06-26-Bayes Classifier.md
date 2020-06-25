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


Now that we have a bit of an intuition regarding what bayes theorem is, we want to construct a class in Python using only the numpy, and scipy libraries. After walking thru the set up and how to use our Bayes Class we will compare the results to sklearns Gaussian Bayes Classifier. 

## The Full Class:

```
class BayesClassifier:
    '''BayesClassifer takes in a numpy array like list
    of a X: featurs and y: target. 
    
    The X and y array must be of the same length'''
    
    def __init__(self):
        pass

    
    def separate_classes(self, X, y):
        '''
        Takes the feature and target arrays, and seperates them in to
        a subsets of data by the unique classes. This is a helper function
        That will be used in the fit method of the class. 
        '''
       
        self.X = X
        self.y = y
        self.classes = np.unique(y)
        
        # we need the frequencies of the classes.
        # these frequencies will be used in the predicting the 
        # target class of an unobserved datapoint.
        class_type, num_of_class = np.unique(y, return_counts=True)
        self.class_counts = dict(zip(class_type, num_of_class))
        self.class_frequencies = {}
        # frequencies currently is just the total count. We want the proportions. 
        for key, value in self.class_counts.items():
            self.class_frequencies[key] = self.class_counts[key]/sum(self.class_counts.values())
        
        # need to be able to get a subset of arrays by class type. 
        # we will store these in a dictionary. The keys being the class type
        # and the values will be each feature array that has the class as its corresponding target
        
        class_indexes = {}
        subsets = {}
        for i in set(y):
            class_indexes[i] = np.argwhere(y==i)
            subsets[i] = X[class_indexes[i]]
        
        return subsets
    
    def fit(self, X, y):
        '''fit function. Fit the function to the training set.
        takes in an X feature array, and a y Target vector. 
        '''
        
        # to fit the data we need to get the means and the standard deviations for 
        # each class. Will call the separate_classes method to get the unique feature
        # arrays for each class. Then we will get the means and standard deviations
        # for each feature by class. 
        subsetted_X = self.separate_classes(X, y)
        
        self.class_means = {}
        self.class_standard_dev = {}
        for i in subsetted_X:
            self.class_means[i] = subsetted_X[i].mean(axis=0)
            self.class_standard_dev[i] = subsetted_X[i].std(axis=0)
    
    def probability_density_func(self, class_index, x):
        '''
        HT https://en.wikipedia.org/wiki/Gaussian_function
        
        Use the probability function to calculate the relative liklihood that a value
        of our random variable 'x' would equal our sample.
        
        
        '''
        
        class_mean = self.class_means[class_index]
        class_stdev = self.class_standard_dev[class_index]
        
        exponent = np.exp(-((x - class_mean)**2 / (2 * class_stdev**2)))
        base = 1 / (class_stdev * np.sqrt(2 * np.pi))
        return (base * exponent)
    

        
    def predicted_probabilities(self, x):
        '''Get a list with the probabilities for each class and return the class
        with the highest probablity. This will make our output a class, not a probability'''
        
        pred_probabilites = []
                       
        for idx, cls in enumerate(self.classes):
            cls_frequency = np.log(self.class_frequencies[cls])
            pred_proba = np.sum(np.log(self.probability_density_func(idx, x)))
            pred_proba = pred_proba + cls_frequency
            pred_probabilites.append(pred_proba)
        
        return self.classes[np.argmax(pred_probabilites)]

    def predict(self, X):
        '''Get the predicted class for each target x in the given X dataset'''
        y_pred = [self.predicted_probabilities(x) for x in X]
        
        return y_pred
        ```

## Breakdown

