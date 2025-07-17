# 📄 Paper Summary: Unsupervised 3D Pose Estimation for Hierarchical Dance Video Recognition

**Title**: Unsupervised 3D Pose Estimation for Hierarchical Dance Video Recognition
**Link**: [https://arxiv.org/abs/2109.09166](https://arxiv.org/abs/2109.09166)
**Authors**: Xiaodan Hu, Narendra Ahuja
**Published by**: ICCV 2021 (University of Illinois Urbana-Champaign)

---

## ✅ Day 0 – Why I Chose This Paper

### 📌 Background & Motivation

After exploring pose estimation and action recognition methods, I became interested in how skeletal motion can be used to classify **dance genres** like hip-hop, waacking, and more.
I wanted a paper that offered a clean pipeline from pose extraction to temporal modeling, especially one that works well with custom-collected videos.

This paper stood out because it:

* Proposes an **unsupervised 3D pose estimation** pipeline starting from 2D keypoints
* Introduces a **hierarchical framework** for understanding body motion and classifying dance genre
* Uses an **LSTM model** over pose sequences to model spatiotemporal movement patterns
* Works with **multi-person dance videos**, without relying on 3D ground truth annotations

Perfect for building an explainable, lightweight pipeline for human-centric vision tasks.

---

## ✅ Day 1 – Abstract, Introduction & Motivation

### 📌 Abstract Summary

This paper presents **HDVR** (Hierarchical Dance Video Recognition), a framework for recognizing dance genre using **unsupervised 3D pose estimation**.
From 2D keypoint sequences, the method tracks dancers and lifts poses to 3D without needing ground-truth annotations.
An LSTM is trained on these 3D pose sequences to extract 154 types of body part movements across 16 joints, and classify dance genres.
The approach is shown to outperform existing 3D pose estimation methods on the University of Illinois dance dataset.

---

### 📌 Introduction Insights

* Existing dance recognition models often rely on **RGB video features**, **wearable sensors**, or **supervised 3D annotations**
* 2D pose data is ambiguous, especially for self-occlusion and camera viewpoint
* This paper proposes a fully **unsupervised lifting** from 2D to 3D poses
* Temporal modeling is done via an **LSTM**, which interprets 3D motion patterns and enables genre classification
* The approach is **hierarchical**:

  * **Low-level**: raw images or pose sequences
  * **Mid-level**: segmented movement patterns
  * **High-level**: dance genre labels

---

### 📌 Problem Statement

How can we recognize complex dance genres using only pose sequences, without 3D ground truth or appearance features?
Traditional methods either rely on heavy annotations or focus on video texture rather than motion.
This paper solves this by using **unsupervised 3D pose estimation** and modeling motion via **hierarchical temporal reasoning**.

---

### 📌 Core Design Principles

1. Extract 2D keypoints from video using OpenPose or BlazePose
2. Track dancers and lift 2D poses to 3D in an unsupervised manner
3. Model 3D pose sequences with LSTM to capture temporal dynamics
4. Classify genre based on movement types across 16 body parts

---

### 📌 3-Line Summary

* This paper proposes HDVR, a pipeline for dance genre classification via unsupervised 3D pose estimation
* It avoids reliance on RGB frames or 3D ground truth, making it ideal for custom datasets
* LSTM-based temporal modeling interprets complex motion and enables hierarchical understanding

---

### 📌 Unfamiliar Terms

* **HDVR**: Hierarchical Dance Video Recognition – the proposed framework
* **Unsupervised 3D pose estimation**: Lifting 2D keypoints to 3D pose sequences without ground-truth 3D labels
* **Hierarchical modeling**: Representing data at multiple semantic levels (e.g., keypoints → motion → genre)

---

## ✅ TL;DR

This paper builds a clean, lightweight pipeline to classify dance genres from raw video using only 2D pose sequences.
By lifting them to 3D and using LSTM-based temporal modeling, the method delivers strong recognition performance without appearance-based features or labels.
A great starting point for building explainable and customizable motion-based recognition systems.
