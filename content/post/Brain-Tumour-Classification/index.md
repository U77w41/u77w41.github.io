---
title: Brain Tumour Classification
slug: brain-tumour-classification
description: Fine-tuning a VGG16 deep convolutional neural network in PyTorch to classify brain MRI scans across four diagnostic classes.
aliases:
- /p/brain-tumour-classificaion/
image: Anaplastic_astrocytoma.jpg
date: 2022-05-02T00:00:00+00:00
weight: 3
categories:
- Deep Learning
tags:
- Machine Learning
- Deep Learning
- Computer Vision
- Image Classification
- PyTorch
---

**[GitHub Repository](https://github.com/U77w41/Brain-Tumour-Classificaion/) · [Jupyter Notebook](https://github.com/U77w41/Brain-Tumour-Classificaion/blob/main/notebooks/Brain%20Tumor%20Classification%20CDS%20Project.ipynb)**

## Overview

Magnetic resonance imaging (MRI) is the primary non-invasive modality for diagnosing intracranial tumors. However, differentiating between tumor subtypes on axial MRI slices is challenging due to variable lesion margins, necrotic tissue regions, and subtle intensity differences between abnormal and healthy neural tissue.

This project implements a transfer learning pipeline in PyTorch, fine-tuning a VGG16 convolutional neural network to categorize brain MRI scans into four distinct diagnostic categories.

## The Diagnostic Classes

The classification model evaluates MRI slices across three major intracranial tumor categories alongside healthy control scans:

| Class | Pathological Profile |
| :--- | :--- |
| **Glioma** | Primary brain tumors arising from glial cells, typically presenting irregular, infiltrative boundaries. |
| **Meningioma** | Usually slow-growing tumors arising from the meningeal membranes enveloping the brain and spinal cord. |
| **Pituitary** | Tumors originating in the pituitary gland at the base of the skull, frequently affecting hormonal pathways. |
| **No Tumor** | Control scans exhibiting normal cerebral anatomy without focal space-occupying lesions. |

## Preprocessing & Image Augmentation

MRI scans vary significantly in intensity, resolution, and skull orientation across clinical imaging centers. Using OpenCV and PyTorch torchvision transforms, each image undergoes standardized preprocessing:

* **Contour-Based Brain Cropping:** Detects the largest contour in the grayscale image to isolate the cranial region, removing black border margins and non-informative background pixels.
* **Standardization:** Resizes images to $224 \times 224$ pixels and normalizes pixel values against standard ImageNet channel means and standard deviations.
* **Data Augmentation:** To prevent overfitting on small clinical sample cohorts, the training pipeline applies:
  * Random horizontal and vertical flips
  * Random affine rotations ($\pm 15^\circ$)
  * Random scaling and perspective shifts
  * Brightness and contrast adjustments

## Model Architecture & Transfer Learning

Instead of training a deep convolutional network from scratch on a limited medical dataset, the pipeline adapts **VGG16** pre-trained on ImageNet:

1. **Feature Extractor:** Frozen early convolutional blocks retain low-level visual primitives (edges, gradients, textures).
2. **Fine-Tuning:** The deeper convolutional layers are unfrozen with a reduced learning rate to learn domain-specific neuroimaging patterns.
3. **Custom Classification Head:** Replaces VGG16's original 1000-class classifier with:
   * Adaptive Average Pooling
   * Dense Layer (512 units) with ReLU activation
   * Dropout ($p = 0.5$) for regularization
   * Final Linear Layer (4 output units) mapped through Softmax

## Optimization & Experimentation

We benchmarked four optimization algorithms to assess convergence stability and loss reduction:

* **Adam:** Provided the fastest initial convergence and stable training progression using adaptive moment estimation.
* **SGD with Momentum ($0.9$):** Exhibited smoother gradient trajectories and achieved competitive final test accuracy when paired with a cosine annealing learning rate scheduler.
* **RMSprop & AdaGrad:** Evaluated as adaptive baselines, though RMSprop showed occasional loss oscillations on smaller batch sizes.

## Results & Diagnostic Evaluation

Model performance was evaluated on a held-out test split using multi-class metrics:

* **Confusion Matrix Analysis:** Evaluates class-specific misclassifications, prioritizing high sensitivity on glioma scans to avoid missed high-grade lesions.
* **Per-Class Precision, Recall, and F1-Score:** Ensures consistent diagnostic performance across both common and rarer tumor presentations without class bias.

The complete training scripts, hyperparameter configurations, and evaluation graphs are documented in the [project notebook](https://github.com/U77w41/Brain-Tumour-Classificaion/blob/main/notebooks/Brain%20Tumor%20Classification%20CDS%20Project.ipynb).

---

> Photo credit: [Wikipedia / Wikimedia Commons](https://upload.wikimedia.org/wikipedia/commons/d/d8/Anaplastic_astrocytoma.jpg)
