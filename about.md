---
title: About
layout: default
permalink: /about/
extra_css: |
    .profile-container {
        display: flex;
        align-items: center;
        gap: 2rem;
        margin-bottom: 3rem;
    }
    .profile-pic {
        width: 150px;
        height: 150px;
        border-radius: 50%;
        border: 2px solid var(--border-line);
        padding: 5px;
        background: white;
    }
    .bio-lead {
        font-family: "Georgia", serif;
        font-size: 1.4rem;
        color: var(--cinnabar);
        margin-bottom: 0.5rem;
    }
    @media (max-width: 600px) {
        .profile-container {
            flex-direction: column;
            text-align: center;
        }
    }
    .repo-grid {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 1.5rem;
        margin-top: 2rem;
    }
    .repo-card {
        padding: 1.5rem;
        border: 1px solid var(--border-line);
        background: rgba(255,255,255,0.5);
    }
    .repo-card h4 {
        margin: 0 0 0.5rem 0;
        font-family: "Georgia", serif;
        color: var(--ink-black);
    }
    .repo-card p {
        font-size: 0.9rem;
        margin: 0;
        color: var(--smoke-gray);
    }
---

<div class="profile-container">
    <img src="https://github.com/janmaru.png" alt="Mauro" class="profile-pic">
    <div>
        <div class="bio-lead">Mauro (janmaru)</div>
        <p class="meta">Senior Developer • Taipei, Taiwan</p>
    </div>
</div>

<section class="content" markdown="1">

Senior Developer specializing in **C#, Scala, and F#**. My professional journey is driven by a deep fascination with *emptiness*, understood as a space of possibility and architectural clarity.

Through the lens of **Category Theory** and **Meta-rationality**, I explore new ways of conceiving software, seeking a balance between the expressive power of the functional paradigm and the robustness of enterprise systems.

### Main Interests

- Functional Programming (F#, Scala)
- Zero Knowledge Proofs & Cryptography
- High-attention-density Software Architectures

### Selected Projects

<div class="repo-grid">
    <div class="repo-card">
        <h4>mahamudra-core</h4>
        <p>Core & Guard Extensions for robust .NET development.</p>
    </div>
    <div class="repo-card">
        <h4>mahamudra-tapper</h4>
        <p>An opinionated wrapper for Dapper (.NET).</p>
    </div>
    <div class="repo-card">
        <h4>mahamudra-project-euler</h4>
        <p>Pure mathematical solutions written in F#.</p>
    </div>
    <div class="repo-card">
        <h4>oneKeyPad</h4>
        <p>One-time pad (OTP) library in C#.</p>
    </div>
</div>

</section>
