---
title: "Face Selective Neuron Experiment"
date: 2025-03-31T13:59:12-04:00
draft: false
---

### Face‑Selective Neuron Experiments

In this set of experiments I set out to determine whether the neurons in a TopoNet model that appear to “light up” for faces are truly **necessary** and **sufficient** for face discrimination. 
---

### 1. Activation Extraction  
- **Forward Hook**  
  I attached a PyTorch forward‑hook to the final convolutional block (`layer4`) of a pretrained TopoNet (ResNet‑18).  
- **Data Collection**  
  I ran **1,000** face images from the CelebA dataset through the model.  
- **Feature Vector**  
  For each image, I collapsed each channel’s H×W activation map to its mean—yielding a single scalar activation per channel. This produced a **1,000 × C** activation matrix.

---

### 2. PCA for Identifying Face‑Selective Neurons  
- **Fit PCA**  
  I performed Principal Component Analysis on the 1,000 × C matrix to understand which channels carried the most variance when processing faces.  
- **Channel Ranking**  
  I also computed each channel’s mean activation across all face images and selected the **top 5 %** of channels (above the 95th percentile) as candidate **face‑selective neurons**.

---

### 3. Necessity Test (Lesion Experiment)  
- **Lesion Hook**  
  I modified the forward hook to **zero out** only the face‑selective channels during inference.  
- **Re‑Inference**  
  Running the same 1,000 face images through the lesioned model, I captured the new activations.  
- **Metrics**  
  - **Recognition Accuracy**: I passed the lesioned activations through a simple classifier head (e.g., logistic regression) to measure drop in face‑vs‑non‑face accuracy.  
  - **PCA Explained Variance**: I refit PCA on the lesioned activations and plotted the cumulative variance curve.  
- **Conclusion**  
  A sharp drop in accuracy and explained variance confirmed these neurons are **necessary**—the network truly relies on them.

---

### 4. Sufficiency Test  
- **Inverse Lesion**  
  I zeroed out **all other** channels except the face‑selective ones, effectively isolating the candidate neurons.  
- **Re‑Inference & Metrics**  
  I measured classifier accuracy and PCA variance again on that reduced activation set.  
- **Conclusion**  
  If accuracy remained high and PCA variance curve closely matched the baseline, it demonstrated those channels alone are **sufficient** to drive face discrimination.

---

### 5. Control Lesion (Random Neurons)  
- **Random Subset**  
  To verify specificity, I repeated the lesion procedure on a **random** subset of channels (same size as the face‑selective group).  
- **Comparison**  
  The random‑lesioned PCA and accuracy curves performed much worse than the face‑lesioned curves, confirming the unique importance of the face‑selective neurons.

---

### 6. Spatial Visualization  
- **Heatmap**  
  For a single example image, I visualized the spatial activation map of one face‑selective channel—showing exactly where on the face it responds most strongly.

---

### 7. More Analysis
- **Scene & Object Datasets**  
  I collected activations on Places365 (scenes) and CIFAR10 (objects) using the same face‑selective hook.  
- **Activation Comparison**  
  I compared the mean activation of the face‑selective channels across faces, scenes, and objects—demonstrating they respond more to faces.

---

