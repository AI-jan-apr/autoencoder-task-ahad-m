[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/jYmqdKSO)
# Autoencoder Image Retrieval - MNIST

## Overview
A simple image retrieval system using an autoencoder trained on the MNIST dataset (70,000 handwritten digit images). Given a query image, the system returns the top 5 most similar images from the training set.

## How It Works
1. **Load & preprocess** MNIST data (normalize pixels to 0-1, split into 60k train / 10k test)
2. **Build an autoencoder** with encoder (784 → 256 → bottleneck) and decoder (bottleneck → 256 → 784)
3. **Train** the autoencoder to reconstruct input images using binary crossentropy loss and Adam optimizer
4. **Encode** all training images using the encoder only to get compressed representations
5. **Retrieve** similar images by computing cosine similarity between a query image and all encoded training images

## Bottleneck Size Comparison

I experimented with three different bottleneck sizes to see how compression level affects performance:

| Metric | 16 | 32 | 64 |
|---|---|---|---|
| Final Train Loss | ~0.092 | ~0.084 | ~0.073 |
| t-SNE Clusters | Clear with some overlap | Clear and well-separated | Clear but more overlap |
| Retrieval | Works correctly | Works correctly | Works correctly |

**Key takeaways:**
- Larger bottleneck = lower loss (better reconstruction) but the model doesn't need to separate digits as strongly
- Smaller bottleneck = higher loss but forces the encoder to learn the most important features
- 32 is a good balance between reconstruction quality and meaningful compression

