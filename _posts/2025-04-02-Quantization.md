---
title: 'Understanding Quantization!'
date: 2024-11-12
permalink: /posts/2025-04-02-Quantization/

---

Massive models contain billions of parameters. These models are incredibly powerful, but their size comes with significant challenges. This is where **Quantization** steps in as a crucial optimization technique.

## Why Large Models Are a Problem
- **Big Storage Needs** - Models often use 32-bit floats (float32) for weights. For example, a 7-billion-parameter model may need around 28GB of disk space.  

- **High Memory Demands** - Running the model requires loading these huge numbers into RAM or GPU memory, which many devices simply can't handle.  
- **Slow Computations** - Floating-point math is slower than integer math. Think about doing 1.21 * 2.897 versus 3 * 6.
- **More Energy Use** - More data and heavier math operations consume extra power, a concern for edge devices.

## The Core Idea
Quantization reduces the precision of numbers in a model, usually converting from 32-bit floats to 8-bit integers (int8). This process isn't just rounding off numbers; it involves a smart mapping that keeps the most important information intact.

## How Does it Work? (Mapping Floats to Ints)
- **Understand the Goal** - We want to perform the core operations of a neural network layer (like Output = Weight * Input + Bias) using fast integer math instead of slower floating-point math.  
- **The Mapping** - To do this, we need a way to map the range of floating-point values found in the weights and activations to the much smaller range representable by integers (e.g., int8 can represent 256 distinct values, typically from -128 to 127).  

- **Scale and Zero-Point** - This mapping usually involves two key parameters:
   -  Scale (S): A floating-point number determining the ratio between the float range and the integer range. It tells us how much "real value" each step in the integer range represents.
   - Zero-Point (Z): An integer value that indicates which integer corresponds to the real number zero (0.0) in the original floating-point representation. (Note: In symmetric quantization, the zero-point is often fixed at 0).  
- **The Conversion** - A floating-point value X is quantized to an integer (X_q) using a formula like: (X_q) = round(X / S + Z). The result is then clamped to stay within the valid integer range (e.g., [-128, 127]).  
- **Dequantization** - To get back an approximate float value (needed for layers that expect floats or to understand the result), we reverse the process: (X_dq) = (X_q - Z) * S.  
- **Quantization Error** - Because we're squeezing a large range of possibilities into fewer bits, some precision is inevitably lost. (X_dq) will be close to, but not exactly the same as, the original X. A major goal of quantization techniques is to minimize this error so the model's overall accuracy doesn't suffer significantly.

### Types of Mapping
- **Asymmetric** - Uses both scale and zero-point to fully cover the range of the data.  
- **Symmetric** - Assumes data is centered around zero and typically fixes the zero-point at 0.

## Why Quantize?  
- **Smaller Models** - Reduces storage and memory needs (e.g., 28GB can shrink to around 7GB).  
- **Faster Inference** - Integer math is faster than floating-point math.  
- **Lower Energy Consumption** - Less data and simpler math reduce power use.  
- **Wider Deployability** - Makes it possible to run large models on devices with limited resources.

## Putting it into Practice - PTQ vs QAT
How do we actually quantize a model?

- **Post-Training Quantization (PTQ)** - Take an already trained float32 model and convert its weights to a lower precision (like int8). Often involves a "calibration" step where you run sample data through the model to determine the typical range of activations and calculate the best scale/zero-point values. PTQ is simpler as it doesn't require retraining, but might lead to a noticeable drop in accuracy for some models.  

- **Quantization-Aware Training (QAT)** - Simulate the effects of quantization during the model training process. The model learns to adapt to the lower precision, effectively becoming robust to quantization errors. QAT is more complex and requires retraining but usually results in better accuracy compared to PTQ.

### More things to look for
- **Dynamic Quantization** - Converts weights to lower precision "on the fly" during inference, which is especially useful when activation ranges vary a lot. 

- **Per-Channel Quantization** - Instead of using a single scale and zero-point for an entire layer, assign separate parameters to each channel (especially in convolutional layers). This often improves accuracy.  
- **Mixed Precision Approaches** - Sometimes, a combination of different precisions (e.g., int8 for some layers and float16 for others) can balance speed and precision better.  
- **Hardware Support** - Different devices (CPUs, GPUs, and specialized accelerators) have varying levels of native support for low-precision math. Choosing the right quantization technique may depend on your target hardware.  
- **Calibration Techniques** - For PTQ, selecting the right calibration data (a set of sample inputs) is critical to accurately estimate the range of activations and minimize quantization error.  
- **Fine-Tuning After Quantization** - In some cases, a short retraining phase after quantization can help recover any lost accuracy.  
- **Quantization During Training** - Beyond QAT, some research explores incorporating quantization even earlier in the training process to build models that are inherently robust to lower precision.

## Handling Challenges (Like Outliers)
Naive quantization (just using the absolute min/max values to set the range) can be sensitive to outliers, which might stretch the required range unnecessarily, causing most values to lose precision. Techniques using percentiles to determine the range can often yield better results by ignoring extreme outliers.

### Stay Curious☺️….See you in the next one!