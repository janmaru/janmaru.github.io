---
title: "The Evolution of Learning in Neural Networks"
date: 2025-02-25
author: Mauro Ghiani
category: AI
tags: [NeuralNetworks, Perceptron, Backpropagation, ReLU, MachineLearning]
excerpt: "A perceptron is the most basic type of artificial neural network. You can see it as a simple decision-making unit: it takes multiple inputs, multiplies them by corresponding weights, sums them up, and applies an activation function."
---

A **perceptron** is the most basic type of *artificial neural network*, consisting of inputs as **feature** values, **weights** as modifiable parameters, and a **bias** as the additional parameter to shift the decision boundary.

Normally, **the activation function** is a step function or *sigmoid*, which squashes the output between 0 and 1, and the output is a prediction based on input processing.

You can see it as a simple decision-making unit: it takes multiple inputs, multiplies them by corresponding weights, sums them up, and applies an activation function to produce an output.

**y = (x1 * w1) + (x2 * w2) + ... + (xn * wn) + bias**

*So let's create a simple AND single neural network.*

**The perceptron's algorithm** iteratively adjusts the weights and **bias** of the perceptron to **minimize** the error between the predicted output and the target output.

*The learning rate controls the step size of the weight adjustments.*

The process will be repeated for a specified number of **epochs**.

The code in the example uses a step function as its activation:

*output = 1 if weighted_sum >= 0 else 0.*

This means that if the weighted sum is greater than or equal to 0, the output is 1; otherwise, it's 0.

It's a simple threshold activation.

Instead using **ReLU Activation,** as **ReLU** is defined as:

*f(x) = max(0, x)*

it outputs the input if it's positive, and 0 if it's negative.

So the first difference would be that the step function produces a **binary output** (0 or 1), while ReLU produces a **continuous** output, i.e. any non-negative number.

*The step function is not differentiable at 0, while the ReLU function is differentiable almost everywhere.*

And this differentiability is very important for backpropagation in deeper networks.

**So the perceptron learning rule** is a simpler learning algorithm working for single-layer perceptrons.

It directly updates the weights facing the error between the predicted and actual outputs.

While the code does incorporate the ReLU activation function, it is still using the perceptron learning rule for a single-layer perceptron.

**Backpropagation** is a more general algorithm used to train multi-layer neural networks.

It concerns calculating the gradient of the error concerning the weights and biases throughout the network and then using this gradient to update the parameters.

ReLU is a computationally efficient activation function often used in conjunction with backpropagation in multi-layer networks.

But it is not enough.

The last code uses **backpropagation** to adjust the weights and bias based on the error between its predictions and the actual values. It can use either a ReLU or a **Sigmoid** activation function.

If the problem is a **binary classification** problem like the AND function, then a Sigmoid is often a good choice for the final activation layer since it provides a probability between 0 and 1.

However, ReLU may also be used, even tho when the output is a continuous value or if the problem is a multi-class classification problem, ReLU might be a more suitable choice.

Most of the time you need to test different values to see what works best for your specific problem. And using multiple activation options, and **observing** their behavior, helps to better understand the problem.

It depends on the specific characteristics of the task and dataset, using **empirical evaluation**.
