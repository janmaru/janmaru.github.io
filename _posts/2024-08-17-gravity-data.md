---
title: "Gravity Is Not Data"
date: 2024-08-17
author: Mauro Ghiani
category: AI
tags: [MachineLearning, NeuralNetworks, NoFreeLunch, DataScience]
excerpt: "If we had enough data, we could infer Newton's law straight from itself. Let's see if we can."
---

**Gravity** by John Mayer is a narcissistic representation of love and connection, unlike Newton's physical force. It's a metaphor for the pull we feel toward something we can't escape, an emotional weight that keeps drawing us back.

> *Gravity is working against me*
> *And gravity wants to bring me down*

> *Oh, I'll never know what made it so difficult to put you out of my mind*

This kind of gravity is not about mass and acceleration. It's about the inescapable attraction between people, the force that keeps us tethered when we'd rather be free.

But Newton's gravity is different. It's precise. It's mathematical.

## Newton's Law

In **Philosophiæ Naturalis Principia Mathematica** (1687), Isaac Newton formulated the relationship between force, mass, and acceleration near the Earth's surface:

**F = mg**

Where **F** is force, **m** is mass, and **g** is the gravitational acceleration constant (~9.81 m/s²).

It's elegant. It's simple. And it's been verified through centuries of observation and experiment.

## The End of Theory

In 2008, Chris Anderson published an article in *Wired* titled **"The End of Theory: The Data Deluge Makes the Scientific Method Obsolete."**

Anderson's argument was provocative: with enough data, we don't need models or theories. The numbers speak for themselves. Correlation replaces causation. Patterns emerge without hypotheses.

If that's true, then we should be able to take raw data — masses and forces — and let a neural network discover **F = mg** on its own.

Let's try.

## A Keras Neural Network

We build a neural network with **64 neurons** and a **ReLU** activation function. We feed it data: mass values as input and corresponding force values as output.

```python
from keras.models import Sequential
from keras.layers import Dense

model = Sequential()
model.add(Dense(64, activation='relu', input_shape=(1,)))
model.add(Dense(1))
model.compile(optimizer='adam', loss='mse')
```

After training, we extract the learned relationship.

The first model learns:

**F = 0.132 * m + (-0.027)**

That's far from **F = 9.81 * m**.

## What Went Wrong?

The data itself isn't wrong. The relationship is there, buried in the numbers. But the model architecture — 64 neurons with ReLU — is too complex for what is essentially a linear relationship. It needs a scaling factor, not a deep abstraction.

## Rebuilding

We rebuild with **1 neuron**, a smaller batch size, and a simpler architecture. We strip the model down to its essence.

```python
model = Sequential()
model.add(Dense(1, input_shape=(1,)))
model.compile(optimizer='adam', loss='mse')
```

This time, the model gets much closer to the expected equation. The weight converges toward 9.81, the bias toward 0.

## The No Free Lunch Theorem

The **No Free Lunch theorem** states that no single machine learning algorithm is universally best for all problems. Every algorithm makes assumptions, and those assumptions work well for some data distributions and poorly for others.

Data alone won't convey meaning aside from the specific model we choose to interpret it through. The architecture matters. The assumptions matter. The framing of the problem matters.

Anderson was wrong — or at least, incomplete. Data without a model is noise. And a model without the right structure is just sophisticated curve-fitting.

And, in some sense, the model itself is just an educated guess.
