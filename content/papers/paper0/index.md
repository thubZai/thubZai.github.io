---
title: "Pathological Truth Bias in Vision-Language Models" 
#date: 2023-02-01
tags: ["Multimodal Learning","Spatial Reasoning","Mechanistic Interpretability", "PyTorch"]
author: ["Yash Thube (2025)"]
description: "MATS, a behavioral audit for vision language models, identifies systematic failures in spatial consistency and suggests repair paths through activation patching." 
summary: "MATS, a behavioral audit for vision language models, identifies systematic failures in spatial consistency and suggests repair paths through activation patching." 
cover:
    #image: "paper (1).png"
    alt: ""
    relative: false
#editPost:
 #   URL: "https://doi.org/10.1073/pnas.1816454115"
 #   Text: "Other Journal Name"

---

##### Abstract

Vision-language models (VLMs) exhibit a concerning failure mode termed pathological truth bias: the systematic tendency to affirm visually contradicted statements rather than rejecting them. Using MATS (Multimodal Audit for Truthful Spatialization), we demonstrate that instruction-tuned generative VLMs (LLaVA-1.5, Qwen-VL-chat) show very low Spatial Consistency Scores (SCS ≈ 1–3%) and high Incorrect Agreement Rates (IAR ≈ 75–80%), while contrastive encoders (CLIP, SigLIP) remain substantially more robust (SCS ≈ 57–68%, IAR ≈ 8–12%). Through systematic activation patching across 420 trials, we causally localize these failures to mid-to-late cross-attention layers in generative models and pooled/projection components in contrastive encoders, achieving 23% patch success in restoring correct behavior. Results implicate current instruction-tuning practices that prioritize agreeableness over truthfulness, and identify specific neural loci as targets for intervention-based repairs.

---

##### Links

+ [You can find the preprint here.](https://arxiv.org/abs/2509.22674)
+ [Code and data](https://github.com/thubZ09/mats-vlm.git)

---
<!-- 
##### Citation

Author 1 and Author 2. Year. "Title." *Journal* Volume (Issue): First page–Last page. https://doi.org/paper_doi.

```BibTeX
@article{AAYY,
author = {Author 1 and Author 2},
doi = {paper_doi},
journal = {Journal},
number = {Issue},
pages = {XXX--YYY},
title ={Title},
volume = {Volume},
year = {Year}}
```

---

##### Related material

+ [Presentation slides](presentation2.pdf)
 -->
