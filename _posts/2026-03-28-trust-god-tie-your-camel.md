---
title: "Trust in God, but Tie Your Camel"
date: 2026-03-28
author: Mauro Ghiani
category: AI
tags: [SoftwareEvolution, GenerativeAI, AIAgents, CleanCode]
excerpt: "From deterministic features to neural networks to agents—three eras of programming told through one camel."
---

"When the child was a child, it walked with swinging arms, wished the stream were a river, the river a torrent, and every puddle the sea." (*)

This was since the world was known. A dev **created** every instruction from nothing, unless F11. The real problem was to understand, for instance a camel, using **deterministic** features to figure out a simple classification.

```csharp
// The "Camel" Implementation
bool IsCamel(Image image)
{
    return HasHumps(image) && ResistsDehydration(image);
}
```

Around the last decades a new paradigm emerged: instead of writing specifications, a dev would **train neural networks**. The fundamental change was that neural networks become the transformers of input data and expected data in order to create recognizers.

```csharp
bool IsCamel(Image image)
{
    // The network output is a confidence score [0, 1]
    // that the features in the image match a camel.
    float probability = camelDetectionModel.Predict(image);

    return probability > 0.75f; // Slightly higher threshold for accuracy
}
```

With the arrival of agents today, something fundamental changed. **Everyone can program** because everyone speaks their own language and behavior is not specification but some kind of conversation.

```csharp
async Task<bool> IsCamelAsync(Image image)
{
    var prompt = """
    Analyze this image. If the animal is a camel (Dromedary or Bactrian),
    respond with 'true'. Otherwise, respond with 'false'.
    """;

    var result = await llm.QueryAsync(prompt, image);

    // Modern LLMs are better at following boolean returns
    return bool.TryParse(result.Trim(), out bool isCamel) && isCamel;
}
```

But as they say: "**Trust in God, but tie your camel.**"

(*) *Lied vom Kindsein*, Peter Handke
