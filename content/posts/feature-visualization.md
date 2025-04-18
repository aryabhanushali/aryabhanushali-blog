---
title: "Feature Visualization"
date: 2025-04-17T23:14:38-04:00
draft: false
---

# Feature Visualization

In this project, I explored **feature visualization**, which generates images that maximally activate specific layers and channels in deep networks. Using the **torch‑dreams** library with pretrained Inception v3 and ResNet‑18 models, I was able to:

- Load and configure models for activation maximization.  
- Visualize mid‑level textures and patterns.  
- Fine‑tune hyperparameters for clearer results.  
- Inspect individual channels with custom objectives.  
- Add a neuron‑by‑neuron montage to survey entire layers.  

Feature visualization shows us what neurons or layers in a neural network "see." We start with random noise and tweak the image so a chosen part of the network lights up. The final "dream" image highlights the patterns—like edges, textures, and shapes—that the network uses to make decisions.

---
The feature visualization script performs the following steps:

1. **Imports & Model Loading**  
   It imports `torch-dreams`, `torchvision.models`, and `matplotlib`. Pretrained Inception v3 and ResNet‑18 models are loaded and switched to evaluation mode on the GPU.

2. **Dreamer Setup**  
   A `Dreamer` object wraps the model (or multiple models) to provide a `.render` method for activation maximization.

3. **Layer & Channel Selection**  
   Specific layers (e.g., `Mixed_5b`) or single channels are chosen by passing layer references or custom loss functions to `.render`.

4. **Rendering Images**  
   The `.render` method starts from random noise and iteratively updates pixel values to maximize activations, with options for iteration count, learning rate, geometric transforms, and regularization.

5. **Custom Objectives**  
   A custom function can target one channel’s mean activation, returning `-activation` so the optimizer maximizes that channel’s response.

6. **Neuron Grid Montage**  
   Rendered images for each channel are saved and then laid out in a grid, enabling side‑by‑side comparison of individual neuron preferences.

---


## Neuron‑by‑Neuron Montage

For each neuron (i.e., each channel in the layer), I ran the dream algorithm separately to generate its ideal input pattern.

This produced a unique image for neuron 0, neuron 1, and so on.

I organized these images into rows and columns so that I could compare every neuron side by side.

Key point: Neuron‑by‑neuron visualization shows the individual “preferences” of each filter, and grid layouts scale up interpretability across an entire layer.

---


 
