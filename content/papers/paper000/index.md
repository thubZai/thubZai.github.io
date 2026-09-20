---
title: "Pathological Truth Bias in Vision-Language Models"
date: 2025-01-01
domain: "3D Vision & Sports Biomechanics"
tags: ["Multimodal Learning", "Spatial Reasoning", "Mechanistic Interpretability", "PyTorch"]
author: ["Yash Thube (2025)"]
description: "MATS, a behavioral audit for vision language models, identifies systematic failures in spatial consistency and suggests repair paths through activation patching."
summary: "MATS, a behavioral audit for vision language models, identifies systematic failures in spatial consistency and suggests repair paths through activation patching."
cover:
    #image: "paper (1).png"
    alt: ""
    relative: false
---

##### Abstract

Vision-language models (VLMs) exhibit a failure mode termed pathological truth bias: the systematic tendency to affirm visually contradicted statements rather than rejecting them. Using MATS (Multimodal Audit for Truthful Spatialization), we demonstrate that instruction-tuned generative VLMs (LLaVA-1.5, Qwen-VL-chat) show very low Spatial Consistency Scores (SCS ≈ 1–3%) and high Incorrect Agreement Rates (IAR ≈ 75–80%), while contrastive encoders (CLIP, SigLIP) remain substantially more robust (SCS ≈ 57–68%, IAR ≈ 8–12%). Through systematic activation patching across 420 trials, we causally localize these failures to mid-to-late cross-attention layers in generative models and pooled/projection components in contrastive encoders, achieving 23% patch success in restoring correct behavior. Results implicate current instruction-tuning practices that prioritize agreeableness over truthfulness, and identify specific neural loci as targets for intervention-based repairs.
