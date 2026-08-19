---
date: 2026-01-20
publishDate: 2026-01-20
image:
  caption: "DNA double helix — federated genomic analysis"
  focal_point: Center
  preview_only: false
summary: "Your genome is arguably the most sensitive data that exists about you. So why do we keep asking people to hand it over to centralised databases? A look at what PP-GWAS gets right — and where the hard problems remain."
tags:
- Blog
- AI Privacy
- Genomics
title: "Your DNA should not have to leave the building"
type: "news"
layout: "single"
---

A genome-wide association study, or GWAS, is one of the most powerful tools we have for understanding the genetic basis of disease. By comparing the genomes of thousands of people — some with a condition, some without — researchers can pinpoint variants that raise or lower risk. The approach has already reshaped our understanding of cancer, Alzheimer's, diabetes, and dozens of other conditions.

There is, however, a serious catch. To do a meaningful GWAS, you need *a lot* of participants. Tens of thousands, ideally. And traditionally, that meant aggregating all that raw genetic data in one place — a central database maintained by a hospital network, a biobank, or a consortium of research institutions.

I find this uncomfortable. Not because the researchers involved are untrustworthy, but because genomic data is uniquely, irreversibly personal. Your genome cannot be changed if it is leaked. It contains information not just about you, but about your relatives. It can be used to infer ethnicity, predisposition to mental illness, and traits you may not even know about yourself. The asymmetry between what we ask patients to share and what we can guarantee about how that data will be protected is a problem I spent much of my PhD thinking about.

### The federated alternative

The core idea behind our PP-GWAS work is simple, even if the cryptography underneath is not: what if the genetic data never had to leave each institution in the first place?

In a federated setup, each hospital or research site keeps its participants' data locally. Instead of sending raw genomes to a central server, the sites exchange only the results of local computations — aggregate statistics, model parameters, or encrypted intermediate values. The final analysis is assembled from these pieces without any single party ever seeing the complete dataset.

We combined this with homomorphic encryption (HE), a technique that allows computation to be performed *directly on encrypted data*. The server that orchestrates the study sees only ciphertexts — it can compute correlations and p-values without learning anything about individual participants.

The results, published in *Nature Communications*, showed that PP-GWAS can identify the same genetic associations as a conventional centralised study, with statistical power that degrades gracefully rather than catastrophically. There is overhead — HE is computationally expensive, and distributing a GWAS across heterogeneous hospital systems introduces coordination complexity — but the tradeoff is, I think, worth it.

### What this does not solve

I want to be careful not to oversell this. Privacy-preserving techniques are not a free lunch.

Federated learning protects against a specific threat model: a curious-but-honest server that follows the protocol but would exploit raw data if it could see it. It is less effective against a server that deviates from the protocol, or against inference attacks where an adversary reconstructs individual records from the aggregate outputs. Differential privacy — injecting carefully calibrated noise into the outputs — can help here, but it comes at a cost to statistical power that is particularly painful in genomics, where effect sizes are often tiny.

There is also the question of consent. Technical privacy guarantees are not a substitute for meaningful informed consent. Participants should understand not just that their data is encrypted, but what the study is trying to learn, how long their data is retained, and who benefits from the research. These are governance questions, not cryptographic ones.

### Why this matters beyond genomics

I think the genomics case is useful precisely because it is extreme. If we can build privacy-preserving infrastructure for data as sensitive as a human genome, we have a template for a much wider class of problems: medical imaging, electronic health records, financial data, location histories.

The pattern is the same in each case: valuable research requires data from many people; centralising that data creates risk; federation plus encryption can decouple the scientific value from the privacy cost. The technical pieces exist. What we need now is the will to build systems around them — and the governance frameworks to make sure those systems are used responsibly.

That last part is what brought me to SCRAI. The cryptography is, in some ways, the easier half of the problem.
