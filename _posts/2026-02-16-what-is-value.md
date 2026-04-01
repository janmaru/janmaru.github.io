---
title: "What Is Value? A Function Backed by a Random Input"
date: 2026-02-16
author: Mauro Ghiani
category: Odds
tags: [Bitcoin, Cryptography, SHA256, ProofOfWork]
excerpt: "What if value was just a function, backed by a random input? At the heart of most cryptocurrencies lies a proof."
---

What is **value**? A direct **exchange** of services, a precious **commodity** like gold, or certain certificates backed by physical **reserves**, which shift value towards a promise of conversion or towards a value decoupled from **physical** assets, rooted instead in government decrees and institutional trust.

But what if it was just a function, backed by a random input?

### At the Heart Lies a Proof

At the heart of most cryptocurrencies lies a **proof**.

For example, Bitcoin mining involves finding a nonce—a 32-bit field included in the block header. The block header contains the "merkle root", a single hash representing all transactions in that block, the previous block's hash, and the **nonce**. Miners change the nonce and re-hash the entire header until the resulting hash is below a specific target.

Not necessarily a **certain** number of leading zeros, but the meaning is that: the hash must be numerically less than a specific "target" value.

It's a probabilistic race.

### The One-Way Function

Because SHA-256 is a "one-way function," there is no mathematical shortcut to find the right nonce other than brute-force guessing.

The probability P of finding a valid hash in a single attempt is:

`P = T / 2^256`

where T is the current target.

### Scarcity and Intent

Gold is scarce by geology and distance. No one would bet that an **original** source would be there to convey the **intent**.

If we analyzed every Bitcoin transaction for identity, would we be able to determine its **purpose**?
