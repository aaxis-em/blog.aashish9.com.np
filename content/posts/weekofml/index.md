---
title: "Week of ML"
showTableOfContents: true
date: 2026-08-31
draft: false
---

## Introduction

Tech industry is on steroid with ml the developement that took more than 10 yrs are happenning less than a year because of AI and many stuff are taken as granted behaves as it is how it is supposed to be but I don't want to take it as granted and as a black box i want to understand it all from the begining.

## Motivation

I was semi unemployed and bored so i dedicated a week for ml to learn it all the way from linear regression to multimodel and state of art research on world models.This is my act to document stuff I still don't have habit to document digitally I have a notebook documented and writing this after that week hope you will like it if not i don't even care it's for me bitch.

## Linear Regression (1800)

Linear Regression is simplest fundamental supervised learning algorithm used to model relation between dependent variable and one or more independent variable.The goal is to find the best fitting straight line or hyperplane that represents the association between these variable.
![linearregression](/imgs/linearregression.png)

The training process looks like a random line is drawn and error is calculated MSE or RSS and we try to minimize error by changing slope and intercept according to error.

## K-Nearest Neighbors (1951)

K-Nearest Neighbor is another simplest supervised learning algorithm it is used for classification as well as regression,it's lazy learner algorithm meaning it doesn't actully learns it computes on inferece.

Main principle "If it quack and walk like duck it is a duck".

KNN Classification → majority vote of nearest neighbors.
KNN Regression → average of nearest neighbors' values.
![knn](/imgs/knn.png)

In above example k value was 5 what if k value was 3 it will be classified as circle false so value of k is hyperparamater and tuned according to data during validation step.Other use case is clustering using distance matrix.

{{< katex >}}

## Perceptron (1958)

A Perceptron is one of the simplest forms of an artificial neuron and is commonly used for binary classification.It is used when data is linearly separable using a line.
![perceptron](/imgs/perceptron.png)

### How Does a Perceptron Learn?

Unlike KNN, which mainly stores the training data, a perceptron actually **learns parameters**.

During training, it learns:

1. The weights $algorithmw_i$
2. The bias $b$

#### 1. Compute the Weighted Sum

$$
z = \sum_{i=1}^{n} w_i x_i + b
$$

#### 2. Apply the Step Function

$$
\hat{y} =
\begin{cases}
1 & \text{if } z \geq 0 \\
0 & \text{otherwise}
\end{cases}
$$

#### 3. Calculate the Error

$$
\text{error} = y - \hat{y}
$$

#### 4. Update the Weights

$$
w_i \leftarrow w_i + \eta (y - \hat{y})x_i
$$

where $\eta$ is the **learning rate**.

#### 5. Update the Bias

$$
b \leftarrow b + \eta (y - \hat{y})
$$

#### 6. Repeat for Multiple Epochs

The perceptron processes the training dataset repeatedly.

One complete pass through the entire training dataset is called an **epoch**.

## Naive Bayes (1960)

Also supervised learning algorithm based on bayes theorem. Here, Naive means every variable used is independent.

Bayes' theorem describes how we update the probability of an event when we obtain new evidence.

The formula is:

$$
P(A \mid B) = \frac{P(B \mid A)P(A)}{P(B)}
$$

Where:

- $P(A \mid B)$ — probability of $A$ given $B$ (**posterior**)

- $P(B \mid A)$ — probability of $B$ given $A$ (**likelihood**)

- $P(A)$ — probability of $A$ before observing the evidence (**prior**)

- $P(B)$ — probability of observing $B$ (**evidence**)

With conditional independence assumption (Naive Bayes):

$$P(A|B,C,D,E,\ldots,N) = \frac{P(B|A) \cdot P(C|A) \cdot P(D|A) \cdot P(E|A) \cdots P(N|A) \cdot P(A)}{P(B,C,D,E,\ldots,N)}$$

### Data Table

| Person | COVID (Yes/No) | Flu (Yes/No) | Fever (Yes/No) |
| ------ | -------------- | ------------ | -------------- |
| 1      | Yes            | No           | Yes            |
| 2      | No             | Yes          | Yes            |
| 3      | Yes            | Yes          | Yes            |
| 4      | No             | No           | No             |
| 5      | Yes            | No           | Yes            |
| 6      | No             | No           | Yes            |
| 7      | Yes            | No           | Yes            |
| 8      | Yes            | No           | No             |
| 9      | No             | Yes          | Yes            |
| 10     | No             | Yes          | No             |

#### Step 1: Prior Probability

$$P(\text{fever} = \text{yes}) = \frac{7}{10}$$

$$P(\text{fever} = \text{no}) = \frac{3}{10}$$

#### Step 2: Conditional Probability

How to observe is example for 1st cell we see covid yes and fever yes 2nd cell covid yes fever no and divide them by total no of yes and no in data.
| | Yes | No |
| ----- | ------------- | ------------- |
| COVID | $\frac{4}{7}$ | $\frac{1}{3}$ |
| Flu | $\frac{3}{7}$ | $\frac{1}{3}$ |

#### Bayes' Theorem Formulas

Given Person (Flu, COVID):

$$P(\text{Fever = Yes} \mid \text{Flu, COVID}) = P(\text{Flu} \mid \text{Yes}) \cdot P(\text{COVID} \mid \text{Yes}) \cdot P(\text{Yes})$$

$$= \frac{3}{7} \times \frac{4}{7} \times \frac{7}{10} = \frac{84}{490} = 0.171$$

$$P(\text{Fever = No} \mid \text{Flu, COVID}) = P(\text{Flu} \mid \text{No}) \cdot P(\text{COVID} \mid \text{No}) \cdot P(\text{No})$$

$$= \frac{1}{3} \times \frac{1}{3} \times \frac{3}{10} = \frac{3}{90} = 0.033$$

So here probability of person having fever is more than not having fever when having covid and flu so it's classified as fever.

## Backpropagation (1986)

Lets take an example of Multi layer perceptron as.
![mlp](/imgs/mlp.png)

How does it learn is Backpropagation it's is the algorithm neural networks use to learn by adjusting weights to reduce prediction errors. It works backwards through the network, calculating how much each weight contributed to the error.

The core idea is

- Forward Pass: Data flows through network → prediction
- Calculate Error: Compare prediction to actual answer
- Backward Pass: Error flows backwards → calculate how much to fix each weight
- Update Weights: Adjust weights to reduce error

## Decision Trees (1986)

## Support Vector Machine (1992-1995)

## Bagging (1994)

## AdaBoost (1995)

## Recurrent Neural Network ()

## Long Short Term Memory (1997)

## Random Forests (2004)

## Gradient Boosting Machine (2001)

## Deep Belief Network (2006)

## CNNs

## AlexNet (2012)

## Word2Vec and GloVe(2013-2014)

## VGG16 and GoogleNet (2014)

## Sequence to Sequence (2014)

## Attention (2015)

## ResNet (2015)

## XGBoost (2014)

## Transformer "Attention is all you need" (2017)

## BERT "Encoder only transformer" (2018)

## GPT series "Decoder only transformer" (2018 onward)

## Diffusion Model (2015 theory,2020 practical products)

## Multimodal Models (2020)

## Lightweight distilled,quantized,compressed model (2020)

## World Model and JEPA ()
