---
title: "Feature Visualization"
date: 2025-04-17T23:14:38-04:00
draft: false
---

# Feature Visualization

In this project, I dove into **feature visualization**—the practice of generating images that maximally activate specific neurons or layers in a deep network. By using the **torch‑dreams** library together with pretrained models (Inception v3 and ResNet‑18), I was able to “peek under the hood” and see exactly what each filter is looking for.

---

## Installing and Setting Up

First, I installed the core library:

- **`torch‑dreams`** brings a simple interface for activation maximization.
- I loaded PyTorch’s pretrained **Inception v3** and **ResNet‑18** models, switching them to evaluation mode on the GPU.
- Basic image preprocessing (resize, normalization) was applied so the generated “dreams” would respect ImageNet conventions.

---

## 1. Mid‑Level Texture Dreams

I began by asking the Dreamer to visualize the **Mixed_5b** block of Inception v3—an intermediate layer known to detect textures and simple shape combinations:

1. The Dreamer starts from random noise.
2. It repeatedly perturbs the pixels to maximize the average activation of Mixed_5b’s filters.
3. After a few dozen iterations, the output coalesced into swirling color gradients, edge junctions, and repeating motifs.

**What I learned:**  
Mid‑level filters act like texture and pattern detectors—when you amplify their responses, you get artwork that resembles woven fabrics, rippling water, or feather‑like structures.

---

## 2. Tuning the Dream

Next, I experimented with optimization parameters:

- **Iteration count** and **learning rate** determine how far the image can evolve.
- **Rotations, scaling, and translation** prevent the network from “cheating” by focusing on a single spot.
- **Weight decay** and **gradient clipping** keep the dream from exploding into noise.

By gently adjusting these settings, the resulting images became sharper, more coherent, and richer in semantic detail.

---

## 3. Channel‑Specific Visualization

Rather than visualizing an entire layer, I targeted a **single channel** within a convolutional block:

1. I wrote a custom loss that returns the *negative* mean activation of channel Y, forcing the optimizer to boost that channel’s response.
2. After optimization, the dream revealed the unique “signature” pattern to which that channel is most sensitive—whether it was circular blobs, striped edges, or high‑frequency textures.

**What I learned:**  
Each convolutional filter is its own mini‑detector. By visualizing one at a time, you can assign intuitive labels (“this neuron is a corner detector,” “that neuron fires on yellow gradients,” etc.).

---

## 4. Multi‑Model, Multi‑Layer Dreams

To push things further, I combined two architectures:

- I wrapped Inception v3 and ResNet‑18 into a single **ModelBunch**.
- I asked the Dreamer to maximize both a mid‑level Inception block and a specific ResNet channel simultaneously.
- The resulting images fused characteristics from both networks—creating hybrid patterns that both models found highly activating.

**What I learned:**  
Joint visualization lets you explore the intersection of features across architectures, hinting at shared representational motifs and subtle differences in how networks process visual information.

---

## 5. Building a Montage

Finally, I saved individual channel dreams to disk and arranged them into a grid:

- By plotting a mosaic of 64 channel images, I could compare dozens of filters at once.
- Patterns ranged from floral‑like swirls to grid textures and radial spokes.

**What I learned:**  
Montages are a powerful way to survey an entire layer’s diversity at a glance—great for spotting redundant filters or identifying intriguing outliers.

---

## Takeaways

1. **Textures to Semantics:** Early and mid‑level layers capture color gradients, textures, and simple shapes.  
2. **Custom Loss Functions:** Targeted objectives unlock neuron‑specific insights.  
3. **Cross‑Model Exploration:** Visualizing multiple networks together reveals both commonalities and unique patterns.  
4. **Interpretability at Scale:** Montage displays help scale up from a handful of neurons to entire layers.





