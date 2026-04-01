---
title: "Detect Deterministic Behavior in Ciphertexts"
date: 2021-01-17
author: Mauro Ghiani
category: Odds
tags: [Cryptography, Entropy, OneTimePad, Shannon, Vernam, NLP]
excerpt: "Entropy is the measure of chaos. While Information is Order. Hence both are at opposite ends."
---

**Entropy** is the measure of chaos. While Information is **Order**.

Hence both are at opposite ends.

All-natural phenomena, related to information, face a natural **disorder**. For instance, if we throw one tennis ball in a room what's the **probability** to find the ball in half of the room? And if progressively we throw 2, 4, 6 balls? The probability to find all the balls in a specific squared meter place is very low. What is expected is that the tennis balls are scattered "*randomly*" on the floor.

Let's consider a number of 26-faced cubes with on each face a letter. If those are thrown on the floor what we expect is a random string: ADSIHOASDIOHDASIONHADS".

How does it take to transform this sequence of letters into a **phrase**? What is missing?

A code is a natural language with a lexicon (vocabulary), a grammar that deals with rules to combine the atoms (words) of the language, and a semantic, a way to define the correctness of meaning in the structure.

A natural language has a memory, like human beings, of growing into something more sophisticated.

Languages are living organisms.

Cryptography was born in ancient times with a simple idea, use a transformation function to substitute the original message (plain text) and create a ciphertext:

```
Ci = pi + ki(mod 26) (*)
```

(English natural language, 26 letters)

This approach is sensitive to **frequency analysis**. Any natural-language has a certain letter and word frequency. In the English language, the most common single-letter word is "a" and the most common three-letter word is "the", followed closely by the word "and." Also, the most common two-letter sequences are "oo" and "ee".

Once the original natural language is used in the **plain text**, the letter and word frequency issues can be used to crack the **cipher**.

```csharp
public static Dictionary<char, double> Frequency { get; set; } =
    new Dictionary<char, double>()
    {
        { 'a' ,  0.082 },
        { 'b' ,  0.015},
        { 'c' ,  0.028},
        { 'd' ,  0.043},
        { 'e' ,  0.013},
        { 'f' ,  0.022},
        { 'g' ,  0.02},
        { 'h' ,  0.061},
        { 'i' ,  0.07},
        { 'j' ,  0.0015},
        { 'k' ,  0.0077},
        { 'l' ,  0.04},
        { 'm' ,  0.024},
        { 'n' ,  0.067},
        { 'o' ,  0.075},
        { 'p' ,  0.019},
        { 'q' ,  0.00095},
        { 'r' ,  0.06},
        { 's' ,  0.063},
        { 't' ,  0.091},
        { 'u' ,  0.028},
        { 'v' ,  0.98},
        { 'w' ,  0.024},
        { 'x' ,  0.0015},
        { 'y' ,  0.02},
        { 'z' ,  0.00074}};
```

America's National Security Agency released **papers** from John Tiltman, one of Britain's top cryptanalysts during the Second World War in response to a Freedom of Information Act request.

Russians began using **one-time pads** in 1928, but they made the critical cryptographic error of allowing these pads to be used twice.

The USA started **Operation Venona** in 1943 to decrypt messages using one-time pads, and beginning, so, the science of cryptanalysis.

Bob Morris, the chief scientist at the NSA, warned anyone enigmatically of the **Two-time pad problem**.

It came up that Russians had **reused** pads under the pressure of war when it was hard to get couriers through to embassies.

And it seems to have been Russian policy since today.

Russians felt religious respect for the father of information theory, **Shannon**, and his work on the one-time-pad.

Shannon spoke about **Perfect Secrecy**.

"*It is defined by requiring a system that, after a cryptogram is intercepted by the enemy, the a posteriori probabilities of this cryptogram representing various messages be identically the same as the a priori probabilities of the same messages before the interception. It is shown that perfect secrecy is possible but requires, if the number of messages is finite, the same number of possible keys. If the message is thought of as being constantly generated at a given "rate" R (to be defined later), the key must be generated at the same or a greater rate.*"

In cryptography, the one-time pad (**OTP**) is an encryption technique where the plaintext is somehow altered by a random string of data so that the resulting ciphertext is truly random.

It requires the use of a one-time pre-shared key the same size as, or longer than, the message being sent.

Each bit or character of the plaintext is encrypted by combining it with the corresponding bit or character from the pad using modular addition.

The resulting ciphertext will be impossible to decrypt or break if the following four conditions are met:

- The key is truly **random**.
- The key is at least **as long as** the plaintext.
- The key is never **reused**.
- The key is **secret**.

In 1919, Vernam patented this idea. In Vernam's method, he used the binary XOR (Exclusive OR) operation applied to the bits of the message.

```csharp
public BitArray Cypher()
{
    var t = _text.GetKeyBitArray();
    var c = t.Xor(_key.GetKeyBitArray());
    _cypher = new Cypher(c);
    return c;
}
```

To detect any intention in the wannabe text for the "Vernam Key" we're going to test:

1. if the frequency of the letters, in a given natural language, (in this case English) is "natural" or random.
2. if inside the text that represents the Vernam Key we could detect any human natural language meaningful text.

```csharp
public async Task<bool> FailsNLP(string vernamKey)
{
    var abuse = await _apiClient.Abuse(vernamKey);
    var result = abuse.Abusive <= 0.1
        && abuse.HateSpeech <= 0.1
        && abuse.Neither >= 0.98;
    var emotion = (await _apiClient.Emotion(vernamKey));
    result = result
        && emotion.Emotion.Angry <= 0.3
        && emotion.Emotion.Bored <= 0.3
        && emotion.Emotion.Excited <= 0.3
        && emotion.Emotion.Fear <= 0.3
        && emotion.Emotion.Happy <= 0.3
        && emotion.Emotion.Sad <= 0.3;
    return result;
}
```

We're going to use ParallelDots AI APIs. A set of document classification and NLP APIs built for software developers. The NLP models are trained on more than a billion documents and provide state-of-the-art accuracy on most common NLP use-cases such as sentiment analysis and emotion detection.

```csharp
public async Task<bool> HighEntropy(string vernamKey)
{
    return await FailsNLP(vernamKey) && FailsFrequency(vernamKey);
}

[TestMethod]
public async Task HighEntropy_ShouldReturn_True()
{
    string randomKey = _random.RandomString(255);
    var result = await _text.HighEntropy(randomKey);
    Assert.IsTrue(result);
}
```

[The entire project is available here.](https://github.com/janmaru/mahamudra-cryptography-oneKeyPad)
