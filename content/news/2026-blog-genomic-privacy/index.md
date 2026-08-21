---
date: 2026-01-20
publishDate: 2026-01-20
image:
  caption: "DNA double helix, federated genomic analysis"
  focal_point: Center
  preview_only: false
summary: "Your genome is arguably the most sensitive data that exists about you. So why do we keep asking people to hand it over to centralised databases? A look at what I found comparing two federated learning frameworks on real transcriptomic data."
tags:
- Blog
- AI Privacy
- Genomics
- Federated Learning
title: "Your DNA should not have to leave the building"
links:
  - icon_pack: fas
    icon: scroll
    name: "FL on transcriptomic data (ICCS 2024)"
    url: '/publications/2024_fl.html'
  - icon_pack: fas
    icon: scroll
    name: "PP-GWAS (Nature Communications)"
    url: '/publications/2025_ppgwas.html'
type: "news"
layout: "single"
---

Genomic and transcriptomic data are arguably the most sensitive data that exist about a person. Your genome cannot be changed if it is leaked. It contains information not just about you, but about your relatives, and it can be used to infer ethnicity, predisposition to disease, and traits you may not even know about yourself. Transcriptomic data, which captures which genes are actively being expressed in a cell rather than the fixed genetic code itself, is just as tied to an individual.

Machine learning on this kind of data is valuable for precision medicine, tailoring treatment to a patient's own biomarkers and molecular profile. But to train a useful model you need data from many patients across many institutions, and traditionally that meant centralising raw genomic data in one place, a hospital network, a biobank, or a research consortium. The asymmetry between what we ask patients to share and what we can guarantee about how that data is protected is a problem I spent much of my PhD thinking about.

### Comparing federated learning frameworks on transcriptomic data

In a paper I wrote, published at ICCS 2024, my co-authors and I evaluated federated learning as an alternative. Instead of centralising raw data, each site keeps its transcriptomic data local, trains a model on it, and shares only model updates with a coordinating server, which aggregates them into a shared model. We compared two federated learning frameworks, TensorFlow Federated (TFF) and Flower (FLWR), on two real tasks: binary disease prognosis and multi-class cell type classification.

For disease prognosis, we used a dataset of 12,029 acute myeloid leukaemia (AML) samples pooled from 105 studies. For cell type classification, we used single-cell RNA-seq data from brain tissue, 6,931 cells across 5 dominant cell types. Both are realistic examples of the kind of large, multi-site transcriptomic datasets that precision medicine research depends on.

The headline result is encouraging: federated training barely cost us anything in model quality. Deep learning models reached an AUC of up to 0.98 on disease prognosis, and 0.99 (TFF) and 0.98 (FLWR) on cell type classification, both close to what centralised training would achieve. Logistic regression, a simpler baseline, topped out at an AUC of 0.90, so the choice of model mattered more than the choice of federated learning framework. With a realistic number of participating sites, 3 to 10 clients, accuracy stayed close to that baseline. It only degraded noticeably once we pushed to 50 clients, since each site then trains on much less data.

The two frameworks differ in ways that matter operationally, not just statistically. Flower gave much faster local training thanks to better parallelism, and used noticeably less memory than TensorFlow Federated. TensorFlow Federated, in turn, was more customisable, and importantly, more robust when we added privacy-preserving noise to the model updates before aggregation. Network traffic per round, with 10 clients, peaked at about 4 MB for logistic regression and 30 MB for the deep learning models, well within what a typical healthcare institution's internet connection can handle.

### What this does not solve

I want to be careful not to oversell this. Federated learning protects against a specific threat model, a curious-but-honest server that follows the protocol but would exploit raw data if it could see it. It does not, by itself, protect against an adversary that reconstructs individual records from the aggregated model updates.

Differential privacy, adding calibrated noise to the updates before they are shared, can close some of that gap. In our experiments, this trade-off was steep: once the noise level got high enough to offer meaningful protection, AUC collapsed to below 0.1, effectively destroying the model's usefulness. TensorFlow Federated tolerated noise better than Flower before falling apart, but neither framework made this trade-off disappear. Choosing a framework, in practice, is less about squeezing out marginal accuracy and more about matching its privacy support, ease of use, and resource demands to what your institution actually needs.

There is also the question of consent. Technical privacy guarantees are not a substitute for meaningful informed consent. Participants should understand not just that their data stays local, but what the study is trying to learn, how long their data is retained, and who benefits from the research. These are governance questions, not engineering ones.

### A step further: adding encryption

A related project I contributed to, PP-GWAS, led by my colleague Arjhun Swaminathan and published in *Nature Communications*, pushes this further for genome-wide association studies specifically. It combines federated computation with homomorphic encryption, so that even the intermediate values exchanged during the study stay encrypted, closing some of the gaps that plain federated learning leaves open, at a significant cost in computation.

### Why this matters beyond genomics

I think the genomics case is useful precisely because it is extreme. If we can build privacy-preserving infrastructure for data this sensitive, without giving up much model quality, we have a template for a much wider class of problems: medical imaging, electronic health records, financial data, location histories.

The pattern is the same in each case: valuable research requires data from many people, centralising that data creates risk, and federated approaches can decouple the scientific value from the privacy cost, at least against some threat models. The technical pieces exist. What we need now is the will to build systems around them, and the governance frameworks to make sure those systems are used responsibly.

That last part is what brought me to SCRAI. The engineering is, in some ways, the easier half of the problem.
