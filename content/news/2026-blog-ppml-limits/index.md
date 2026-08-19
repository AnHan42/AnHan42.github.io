---
date: 2026-02-18
publishDate: 2026-02-18
image:
  caption: "Federated learning — distributed institutions, centralised aggregation"
  focal_point: Center
  preview_only: false
summary: "Federated learning, homomorphic encryption, differential privacy — the toolbox of privacy-preserving ML is impressive. But after years of working with these techniques, I have become more cautious about what they actually guarantee."
tags:
- Blog
- AI Privacy
- Federated Learning
title: "The honest limits of privacy-preserving machine learning"
type: "news"
layout: "single"
---

When I started my PhD in late 2021, I was enthusiastic about privacy-preserving machine learning in the way that most new researchers are enthusiastic about their chosen field: I thought the hard problems were mostly technical, and I thought solving them would mostly solve the underlying issue.

Four years and a thesis later, I am still enthusiastic — but differently so. The techniques are genuinely powerful. They also fail in ways that are easy to miss if you are not careful, and the field has a tendency to understate this in its marketing materials.

Here is what I actually think about the main tools in the PPML toolbox.

### Federated learning is good at one specific thing

Federated learning (FL) was designed to address a straightforward problem: you want to train a model on data held by multiple parties, but you cannot move the data to a central location. The solution is to move the model instead — each party trains locally, sends parameter updates to a server, the server aggregates them, and the process repeats.

This is genuinely useful. In our work on federated learning for transcriptomic data, we showed that you can train competitive models on gene expression data distributed across hospitals without ever pooling patient records. The model quality was close to the centralised baseline, and the privacy profile was meaningfully better.

What FL does *not* do, by itself, is provide formal privacy guarantees. Gradient updates can leak information about training data — sometimes a surprising amount of it. Membership inference attacks can determine whether a specific individual was in the training set. Gradient inversion attacks can, in some settings, reconstruct training samples from model updates alone. FL reduces the attack surface; it does not eliminate it.

### Homomorphic encryption: correct but expensive

Homomorphic encryption (HE) is the technique I find most intellectually satisfying and most practically frustrating. The idea — that you can perform arbitrary computations on encrypted data without ever decrypting it — sounds like it should not be possible, and for a long time it was not. The theoretical breakthrough came in 2009; practical, somewhat-efficient schemes followed over the next decade.

We used the CKKS scheme in several of our projects, including the quantum federated learning work published earlier this year. CKKS allows approximate arithmetic on encrypted real-valued data, which makes it well suited to the kinds of linear algebra operations that machine learning involves.

The catch is computational cost. In our experiments, HE operations were two to three orders of magnitude slower than their unencrypted equivalents. For a simple logistic regression, this is manageable. For a deep neural network on high-dimensional data, it becomes prohibitive. There is active research on making HE faster — better algorithms, hardware acceleration, compiler optimisations — and the numbers improve year over year. But "eventually this will be fast enough" is not the same as "this is fast enough now."

The other issue is that HE protects data from the server during computation, but it does not protect the *outputs*. If you train a model using HE and then publish the model weights, those weights may still leak training data. Privacy-preserving computation and privacy-preserving outputs are different problems.

### Differential privacy: rigorous but painful

Differential privacy (DP) is the only technique in this list that comes with a formal mathematical guarantee: the output of a DP mechanism does not change much whether any single individual's data is included or not, by a controlled amount ε. This is a strong statement, and it means something precise.

The problem is that making a mechanism DP requires adding noise, and the amount of noise required for meaningful privacy protection is often large enough to destroy the usefulness of the output. In our work on differentially private multi-label learning, we found that DP is significantly harder to apply in multi-label settings than the standard single-label case: the sensitivity of the gradient grows with the number of labels, which means you need more noise, which means more accuracy loss.

Choosing the right ε is also a governance problem dressed up as a technical one. What does ε = 1 actually mean for a person whose data is in the training set? The formal guarantee is rigorous; the intuitive meaning is opaque. Regulators and ethicists are still working out how DP interacts with legal frameworks like GDPR.

### What I would tell a colleague starting out

None of this is a reason not to use these techniques. They are the best tools we have for a genuine problem, and using them is better than not using them.

But I would encourage a new researcher to be honest about the threat model they are addressing. FL protects against a specific adversary. HE is slow. DP costs accuracy. Combining them can help — we showed this in several papers — but the combination is not magic.

The hardest part, I have come to believe, is not the cryptography. It is the system design: understanding what data is actually sensitive, who the adversary actually is, what level of privacy loss is actually acceptable, and who gets to make that decision. Those questions require input from ethicists, lawyers, and the people whose data is at stake — not just from the people building the models.

That is a more complicated answer than I would have given in 2021. I think it is a more accurate one.
