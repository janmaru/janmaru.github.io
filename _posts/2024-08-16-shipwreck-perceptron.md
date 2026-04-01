---
title: "The shipwreck of the Perceptron"
date: 2024-08-16
author: Mauro Ghiani
category: AI
tags: [Perceptron, NeuralNetworks, MachineLearning, NAND, BooleanLogic]
excerpt: "Frank Rosenblatt in 1957 introduced the Perceptron, a model inspired by human visual processing. The most compelling idea is creating a NAND operator and allowing itself to find the weight and bias to let it work."
---

A pessimist is an optimist who didn't make it.

Frank **Rosenblatt** in 1957 introduced the **Perceptron**, a model inspired by human visual processing.

Rosenblatt started from the **McCulloch-Pitts** neuron model.

It was a model of biological **neurons**. It used a simple binary system where inputs are summed with weight, and if the result exceeds a threshold, the neuron "fires."

It's a simple step function that outputs 0 or 1.

However, the Perceptron included a **learning rule**, which allowed it to adjust its weights based on input data to minimize error over time.

Let's start with a simple model. We want to decide whether to go to the movie theater basing our decision on three parameters:

- the weather
- friends
- proximity

Each of these parameters is weighted. Let's say that weather is important and weighs two as proximity. If the theater is close by, the weather is not that important. But we enjoy going to the movies with our friends, so we weigh in at 4.

We define a function: **2x1 + 4x2 + 2x3 + t > 0;** where t should determine the threshold that allows the "neuron" to fire or not.

In the Perceptron the threshold is the negative bias, but the concept is the same.

If we define a threshold of **t = -5** then the output is fundamentally biased toward friendship, and the neuron will behave accordingly.

Rosenblatt's idea was to construct a type of **linear classifier.**

This means it could only learn and represent decision boundaries that are linear, i.e., a straight line (in 2D), a plane (in 3D), or a hyperplane (in higher dimensions).

The decision boundary was defined by a linear equation.

The perceptron classifies an input vector **x** by calculating a weighted sum of its features (using the weights **w**) and then adding a bias **b**. If the result is greater than or equal to zero, the Perceptron classifies the input into one class; otherwise, it classifies it into another class.

**The most compelling idea is creating a NAND operator and allowing itself to find the weight and bias to let it work.**

Of course NAND is defined as NOT AND.

So first we create a learning algorithm. We initialize weights and bias, then we train the Perceptron starting from the expected output, since we know how a NAND gate should behave.

Since the Perceptron is a linear classifier we should expect the plane divided in two large areas.

Since any logical operator can be written through a NAND:

- **NOT (x1) = NAND(x1 , x2)**
- **AND(x1,x2) = NOT(NAND(x1,x2))**
- **OR(x1,x2)=NAND(NOT(x1),NOT(x2))**

Any digital circuit can be represented using logical operators. This principle is foundational in digital logic design and Boolean algebra.

A network of NAND gates can represent any complex digital circuit, and because a NAND gate is a universal gate, a Perceptron that mimics a NAND gate can also be used as a fundamental building block in a neural network to represent any logical function.

Rosenblatt tragically died in a boating accident in 1971, when the interest in the Perceptron waned in the 1970s due to its limitations.

As Giuseppe Ungaretti reported in the "L'allegria di naufragi", poetry that combines the desperation of the shipwreck with an unusual sense of liberation.

This epiphany is not, but rather a feeling of catharsis that emerges from destruction.
