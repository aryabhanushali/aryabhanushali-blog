---
title: "Face Selective Neuron Experiment"
date: 2025-03-31T13:59:12-04:00
draft: false
---

Welcome to my first post on Arya Bhanushali's blog! 🎉

---

## Face Selective Neuron Experiment:

In this experiment, our goal was to test whether the “face‑selective” neurons that  
appear in TopoNet models (ResNet‑18) are both necessary and sufficient  
to identify faces. We used the CelebA dataset as our face stimulus.

---

### 1. Activation

We “hooked” into the TopoNet’s last convolutional layer (layer4) so that every time an image passed through the network, we captured the raw activation values of all channels at that layer.

We fed in 1,000 face images from CelebA and, for each image, collapsed each channel’s height×width map down to its mean value—giving one activation number per channel per image.

These channel activations form the signal that tells us how strongly each neuron responds to faces.

---

### 2. PCA

We ran Principal Component Analysis (PCA) on the 1,000 × C activation matrix.

PCA finds the directions (linear combinations of channels) along which the activations vary the most across the face images.

We looked at the top components’ loadings and also ranked each channel by its mean activation—then picked the top 5 % as our candidate “face‑selective” neurons.

PCA helps us reduce noise and complexity, spotlighting the small subset of channels that carry the bulk of “face information.” Those high‑variance, high‑activation channels are our prime suspects for being critical to face discrimination.

---

### 3. Lesion

We built a “lesion hook” that, during inference, zeroes out the activation of just the selected face‑neurons.

We re‑ran the same 1,000 face images and measured the drop in (a) recognition accuracy and (b) PCA’s explained variance.  
If silencing those channels breaks the network’s ability to recognize faces, then those neurons aren’t merely correlated with faces—they are necessary: the network relies on them to succeed.

We zeroed out every other channel except our face‑selective ones.

Again, we measured recognition accuracy on the same face set.

If those handful of channels alone can still drive good face performance, then they are sufficient: they carry enough information, by themselves, to solve the task.
