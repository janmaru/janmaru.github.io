---
title: "Coulored Money"
date: 2020-06-23
author: Mauro Ghiani
category: Odds
tags: [Bitcoin, Blockchain, ColoredCoins, Cryptography, DigitalIdentity]
excerpt: "A theory of value is an economic theory that attempts to explain why goods and services are priced as they are. Does Bitcoin or any other cryptocurrency have an intrinsic value?"
---

A **theory of value** is an *economic theory* that attempts to explain why goods and services are priced as they are, how the value of goods and services comes about, and, if such a value exists, how to measure it.

One of the questions always asked is: does Bitcoin or any other cryptocurrencies have an intrinsic value? How do we compare them with Fiat Currency? Since the latter are not backed anymore by any gold or silver and they are similar to cryptocurrencies, being just a measure of *the **trust** that people have over their government or state.*

Bitcoin holds its value on the shoulder of millions of people, miners, and traders, all together in the same network, **a trusted network**, deciding its price on the sole principle of its demand and supply.

But Bitcoin has no sense without an environment where life is easy and gay. That's a **blockchain**!

A blockchain is a ***distributed*** public system where each node preserves a **ledger** read and append-only where **transactions** are recorded. The ledger is a linked list of blocks that, when are consolidated, cannot be tampered.

In the blockchain, everyone is a pseudo-anonymous identity. And when we talk about it, we mean:

Digital Identity:

- A Key pair: **(Sk, Pk)** (a secret key and a public key)
- **Pk** is public and should be published on any channel (for instance on a public website.)
- Public key **(Pk) == identity**. The public key is the real identity in the blockchain.
- Exists a boolean function, called **Verify (Pk, message, signature) == true** that has the public identity, the message, and the signature of the message as input parameters and gives out a boolean. True if the public identity is recognized as the one that signed the document.

So in the end, we talk about **authenticity**, in the sense that our public key says that a particular "message" is true. For instance, it has been published in that format at a particular time. An identity, a **certification authority**, for example, can sign any message with its private key. Then publish its public key and anyone, given the message, the signature, the public key can check that the message was held/created at a certain moment by the authority.

In the Bitcoin blockchain, any transfer of data is "money" **transaction**. The sender signs with its private key the transaction that has a public identity as a receiver. An amount of money is transferred.

A Bitcoin can be viewed as a pseudo-anonymous **transcription** of data, in a **distributed** environment, on a public "read and append-only" **register**.

**Colored coins** were created as a way to issue and transfer assets on the Bitcoin blockchain. A colored coin can be issued to represent anything at all, including stocks, bonds, commodities, real estate, fiat currencies, and even other cryptocurrencies.

Let's make an analogy with real-world money that can help to understand.

A public transportation **authority** has a *public identity* and *private key* for emitting, selling and signing the train tickets.

It does not provide physically a ticket but requires a bill, for instance, a 5 euro money bill, where the customer reads the unique serial numbers on it.

The customer is required only to show the bill with a unique number. It doesn't have to pay with it. The money is just a placeholder of a **property**, an **asset.**

Then the public authority reads the unique number and **signs** with its private key the information and provides a **QR code**.

```
S(Sk,SF5128165575) -> Sign (QR code)
```

When the customer arrives at the train he will find a ticket clerk that doesn't hold the private key of the authority but knows only the public key. It will read the unique number on the ticket, the QR code/signature and using the public key will verify it.

```
V(Pk, Sign, SF5128165575) == true
```

A **Colour Bitcoin** allows **real-world value** to be attached to a cryptocurrency and represents an **asset** or an issuer's promise to **redeem** it for some goods or services.

And the funny thing is that once on the train, the 5 euro bill can be used to buy a nice coffee!!!

So creating assets on money does not delete the **value of the money** once the asset itself is burned.
