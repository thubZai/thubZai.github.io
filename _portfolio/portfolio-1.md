---
title: "Vision Transformers (ViTs)"
excerpt: "And what I learned while implementing them!"
collection: Projects & Papers💡
---

Transformers have revolutionized natural language processing (NLP), powering models like GPT and BERT. But recently, they’ve also been making waves in computer vision. Enter Vision Transformers (ViTs), an approach to image classification that treats images like sequences of patches, similar to tokens in text.

Here’s what I learned while implementing a ViT in PyTorch for classifying handwritten digits and how it changed my perspective on image classification.

![Alt Text](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*hesI2FjNGPyffEOj75-M4A.jpeg)


## ✅What I Did
I set out to build a Vision Transformer (ViT) model in PyTorch and train it to classify handwritten digits (0–9) from the MNIST dataset. The goal was to see if I could accurately identify these digits using a Transformer-based approach, which feels quite different from the traditional Convolutional Neural Network (CNN) methods I was more familiar with.

## ✅How I Got There
### 📌Data preparation dataset
 I started with the MNIST dataset, loading it from CSV files and splitting it into training, validation, and test sets.

✔️Custom Datasets: I created custom PyTorch dataset classes to handle loading, preprocessing, and applying augmentations like random rotations. This step was new for me and helped me understand how to make models more robust.

✔️Normalization: I scaled pixel values between -1 and 1, which I learned helps stabilize training.

✔️Data Loaders: I used (PyTorch DataLoader) to manage batching and shuffling, which made the training process smoother.

### 📌Implementing the ViT Model:
This was the most exciting part!

✔️Patch Embedding Layer: I divided each image into small patches (e.g., 16x16 pixels) using a convolutional layer. Flattening these patches into vectors and adding positional embeddings felt like turning pieces of a puzzle into something the model could process.

✔️Transformer Encoder: I built the encoder with alternating layers of:

👉Multi-Headed Self-Attention (MSA): This allowed each patch to “look” at all other patches. Now, what is MSA? It allows each part of an image (or patch) to focus on other parts of the image, understanding relationships across the entire picture. Think of it like analyzing all the pieces of a puzzle at once to figure out how they fit together. For example, in a cat image, one patch might focus on the ear while considering the position of the eyes for context. By using multiple “heads,” MSA captures different perspectives simultaneously.

👉Feedforward Neural Network (MLP): These layers added complexity and non-linearity. MLP? After understanding relationships through attention, the MLP refines this information by applying transformations and adding non-linearity. For example, if MSA identifies key features like “eyes” or “ears” in a cat image, the MLP combines and weighs these features to make better predictions. It’s like a decision-making layer that ensures the model can capture complex patterns.
Classification Head: The class token aggregates information from all patches to predict the image’s label.

### 📌Training the Model

![Alt Text](https://miro.medium.com/v2/resize:fit:828/format:webp/1*sgIzF9za45x84HB51TX2DA.png)


✔️Loss Function: I used cross-entropy loss to evaluate predictions. 

✔️Optimizer: The Adam optimizer adjusted model weights during training.

✔️Training Loop: Over 40 epochs, I processed batches of images, calculated losses, and updated weights.
I monitored validation metrics after each epoch, which helped me track progress.

✔️Tracking Progress: Plotting training and validation losses and accuracies was a great way to visualize the learning process.

### 📌Evaluation and Prediction

✔️Testing: After training, I evaluated the model on the test set.

✔️Submission File: I saved predictions in a CSV file, which felt rewarding after all the effort.

✔️Visualization: Seeing sample predictions alongside their labels made the results tangible.

![Alt Text](https://miro.medium.com/v2/resize:fit:828/format:webp/1*ep975C27fmEWFCsaVMihFA.png)


## ✅Challenges I Faced and How I Overcame Them
### 📌Computational Limitations

Initially, my hardware struggled with training the ViT due to its high resource demands. I addressed this by reducing the patch size, which lowered the computational complexity.
Using mixed precision training with PyTorch’s torch.cuda.amp package to speed up computations and reduce memory usage.

### 📌Overfitting

During early training, I noticed that the model performed well on the training set but poorly on validation data. To tackle this, I applied data augmentations like random rotations and flips to make the model generalize better. I used dropout layers in the Transformer encoder to prevent overfitting.

### 📌Understanding Self-Attention

Initially, self-attention felt abstract and challenging to grasp, but overcame this by,
Studying visualizations of attention maps to see how the model focuses on different patches.
Resources like “The Illustrated Transformer” broke down the concept intuitively.

## ✅What I Achieved
By the end of 40 epochs, the ViT reached over 92% validation accuracy on MNIST. Watching the training accuracy climb from 10% to 90%, while validation accuracy grew from 16% to 93%, was satisfying. Implementing ViTs helped me grasp ideas like patch embedding, self-attention, and class token-based classification.

![Distribution](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*z_RTunus_0H98UeB9szc_w.jpeg)

![Time Series](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*0e8kGDDgyvniqr4Yiayoaw.jpeg)


## ✅Simplified Explanation
Here’s how I understand it now: Think of an image as a jigsaw puzzle. Each piece (patch) contains part of the image, but only by understanding how all the pieces fit together can you see the full picture. ViTs break images into patches, treat them like puzzle pieces, and use a Transformer to figure out their relationships. This global view sets them apart from CNNs, which focus more on local details.

## ✅Key Concepts I Learned from the ViT Paper
[https://arxiv.org/abs/2010.11929](https://arxiv.org/abs/2010.11929)

✔️Image as Patches: ViTs break it into fixed-size patches like 16x16 instead of processing an image pixel by pixel.

✔️Patch Embeddings: Each patch is flattened and turned into a vector, like word embeddings in NLP.

✔️Positional Embeddings: These embeddings tell the model where each patch belongs in the image.

✔️Self-Attention: This mechanism helps each patch “attend” to every other patch, capturing global context.

✔️Classification Token: A special token gathers information from all patches to predict the final label.

✔️Large-Scale Training: ViTs need large datasets (e.g. ImageNet-21k) to perform well because they lack the image-specific biases built into CNNs.

## ✅Why am I excited about ViTs?
Flexibility: They can adapt to various tasks, from classification to object detection.

Global Context: Unlike CNNs, which focus on local features, they capture the big picture.

Scalability: With enough data, ViTs achieve state-of-the-art results.

## ✅Additional Resources I Found Useful
### 📌Vision Transformer (ViT) Fundamentals

💡[Transformers in Vision: A Survey
A Survey of Visual Transformers](https://arxiv.org/abs/2101.01169)

💡[An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale](https://arxiv.org/abs/2010.11929)

💡[Training data-efficient image transformers & distillation through attention (DeiT)](https://arxiv.org/abs/2012.12877)

💡Attention Is All You Need ( Do I have to provide a link to this paper😅)

💡[Vision Transformer Explained (Blog Post)](https://theaisummer.com/vision-transformer/)

💡[The Illustrated Transformers (Blog Post)](https://jalammar.github.io/illustrated-transformer/)

### 📌Convolutional ViT and Hybrid Models

💡🔉[CvT: Introducing Convolutions to Vision Transformers (Paper)](https://arxiv.org/abs/2103.15808)

💡[CoAtNet: Marrying Convolution and Attention for All Data Sizes (Paper)](https://arxiv.org/abs/2106.04803)

### 📌Video Vision Transformer (ViViT)
💡[ViViT: A Video Vision Transformer (Paper)](https://arxiv.org/abs/2103.15691)

💡[Video Transformer Network (Paper)](https://arxiv.org/abs/2102.00719)

Understanding ViT’s helped me see how they bridge the gap between NLP and vision tasks while offering a fresh perspective on computer vision. If you’re curious about them, I highly recommend diving in!

### [📌Project Code](https://github.com/thubZ09/Vision-Transformer-Implementation)

### Stay Curious☺️….see you in the next one!