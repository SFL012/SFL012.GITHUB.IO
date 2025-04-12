---
layout: post
title: "Deep Learning - TensorFlow and PyTorch Implementation"
description: "Produced word embeddings by using Neural Network."
categories: [DataScience]
tags: [Deep Learning, Python]
redirect_from:
  - /2018/05/01/
---

## Deep Learning - TensorFlow and PyTorch Implementation


### Introduction
Word embeddings and feature extraction are critical in machine learning, particularly in image recognition and natural language processing. This blog examines how autoencoders can extract meaningful features and compares TensorFlow and PyTorch in terms of performance and usability, leveraging CUDA for GPU acceleration.

### Methodology
#### Word Embedding and Feature Extraction
I used neural network-based autoencoders to compress high-dimensional data into a latent space and reconstruct it efficiently, enabling feature extraction critical for downstream tasks.

#### Comparing TensorFlow and PyTorch
Key evaluation areas included:
- Performance Optimization: Training efficiency using CUDA acceleration.
- Code Readability: Ease of implementation and maintainability, comparing both frameworks' syntax and design.

#### TensorFlow and PyTorch Implementation
- TF1 and TF2: Explored legacy and updated TensorFlow features for training autoencoders.

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/76184559/108605851-b5542500-7384-11eb-8504-39ef838ba115.jpg" alt="TF1" width="600"/>
  <p style="font-style: italic; font-size: 14px;">Figure 1: TF1</p>
</div>

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/76184559/108605871-d7e63e00-7384-11eb-8c20-af94da4bcdc4.jpg" alt="TF2" width="600"/>
  <p style="font-style: italic; font-size: 14px;">Figure 2: TF2</p>
</div>

- PyTorch: Focused on its dynamic computation graph and flexible implementation.

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/76184559/108605883-e2083c80-7384-11eb-877a-79f81506d65f.jpg" alt="Autoencoder" width="600"/>
  <p style="font-style: italic; font-size: 14px;">Figure 3: Autoencoder Architecture</p>
</div>

### Experimental Setup
#### Dataset
I used the MNIST dataset (60,000 training images and 10,000 test images), a benchmark for image recognition tasks.

#### Training Configuration
- Learning Rate: 0.001
- Batch Size: 100
- Epochs: 45 (initial training), 20 (fine-tuning).

#### Hardware
All experiments were conducted on a CUDA-enabled GPU for efficient parallel processing.

### Results
- The initial accuracy is **97.9600%** after 45 epoch.

```python
   Extracting MNIST_data/train-images-idx3-ubyte.gz
   Extracting MNIST_data/train-labels-idx1-ubyte.gz
   Extracting MNIST_data/t10k-images-idx3-ubyte.gz
   Extracting MNIST_data/t10k-labels-idx1-ubyte.gz
   The accuracy is 0.9796000123023987 after 45 epoch with learning rate 0.001 and batch size 100.
```

- The final accuracy is **99.0100%** after 20 epoch.

```python
Extracting MNIST_data/train-images-idx3-ubyte.gz
Extracting MNIST_data/train-labels-idx1-ubyte.gz
Extracting MNIST_data/t10k-images-idx3-ubyte.gz
Extracting MNIST_data/t10k-labels-idx1-ubyte.gz
The final accuracy on test set is 99.0100%.
```

### Conclusion
Both frameworks achieved high accuracy with GPU acceleration, demonstrating their capability for feature extraction and data processing. Future explorations could focus on scaling these frameworks to more complex datasets and applications.
