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

---

## ✅ Day 2 – Computational Approach (Method)

### 📌 Overall Pipeline Steps

1. **2D Pose Estimation**: For each frame \( I(t) \), predict 2D keypoints \( \hat{p}^i(t) \) for each person \( i \)
2. **Bounding Box Tracking**: Track the rough position of each dancer as bounding boxes \( B^i(t) = (x, y, w, l) \)
3. **3D Pose Lifting**: Lift the 2D poses into 3D poses \( \hat{P}^i(t) \) using an unsupervised method
4. **Body Part Motion Modeling**: For each body part \( e \in E \), compute motion sequences \( \hat{y}^e_t \)
5. **Dance Genre Classification**: Concatenate all body part motion sequences and feed into LSTM to predict genre label \( \hat{g} \)

---

### 📌 Object Tracking (Section 2.1)

- **Problem in prior methods**: Struggle with multi-person occlusion, limited pose diversity, and dependence on single-person datasets
- **Key contribution**: Propose a 3-stage tracking algorithm to handle dancer overlap robustly

**3-Stage Tracking Process:**
1. **When no overlap**: Use LDES tracker to follow dancer \( i \), maintain color histogram \( h^i_t \) and bounding box \( B^i_t \)
2. **When overlap occurs**: Detect it via sharp change in motion direction (tracker failure)
3. **When overlap ends**: Predict end time and location using velocity before overlap; reassign tracked dancer using best histogram match

→ This allows **robust re-identification and continuous tracking**, even through occlusions

---

### 📌 Tracking-Based 2D Pose Estimation

- Initial 2D poses are extracted using **OpenPose**
- After overlap, a bounding box may contain multiple 2D pose candidates
- Select the pose \( p^i_t \) whose histogram most closely matches that of \( p^i_{t-1} \)

---

### ✅ 1-Line Summary

> A robust object tracking strategy that resolves dancer overlap using color histograms, motion prediction, and re-identification after occlusion.

---

### 🤔 Questions / Unclear Parts

- Exact mechanics of the **LDES tracker** (refer to citation [16])
- How "difference in motion direction" is formally computed to detect tracking failure

---

## ✅ Day 3 – 3D Pose Estimation & Movement Recognition

---

### 📌 Section 2.2 – 3D Pose Estimation

This section addresses the problem of lifting 2D keypoints $p_t$ to 3D poses $\hat{P}_t$ without using ground-truth 3D annotations. Since this is an ill-posed inverse problem, the authors generate multiple 3D pose **seeds** and select the best one using optimization.

#### 🔹 Multi-Seed Strategy & Selection

- Generate $K$ candidate 3D poses $\{ P_t^K, w_t^K \}_{K=1}^{K}$
- Optimize each seed over the time window
- Select the seed with the lowest cumulative error:

$$
k^* = \arg\min_k \sum_t e_t^k
$$

- Final 3D output and parameters:

$$
\hat{P}_t = P_t^{k^*}, \quad w_t = w_t^{k^*}
$$

#### 🔹 Loss Functions

- **2D projection loss**:

$$
L_{2D} = \| \hat{p}_t - p_t \|
$$

- **2D temporal smoothness**:

$$
L_{\text{smooth2D}} = \| \hat{p}_t - \hat{p}_{t-1} \|
$$

- **3D temporal smoothness**:

$$
L_{\text{smooth3D}} = \| \hat{P}_t - \hat{P}_{t-1} \|
$$

- **3D consistency with initial pose**:

$$
L_{3D} = \| \hat{P}_t - P_t^* \|
$$

---

### 📌 Section 2.3 – Body Part Movement Recognition

Each body part $e \in E$ is modeled with an LSTM to predict basic motion types over time.

#### 🔹 Input and Output

- **Input**: 3D pose sequences of joints related to part $e$

$$
\left\{ \left\{ \hat{p}_t^j \right\}_{j \in J_e} \right\}_{t=0}^{T-1}
$$

- **Output**: Multi-label movement predictions

$$
\left\{ \hat{y}_t^e \right\}_{t=0}^{T-1}
$$

#### 🔹 Loss Function (Binary Cross Entropy)

$$
L_{\text{BCE}}^e = \text{BCE}\left( \left\{ \hat{y}_t^e \right\}_{t=0}^{T-1}, \left\{ y_t^e \right\}_{t=0}^{T-1} \right)
$$

Each part's movement can belong to multiple motion classes simultaneously, hence the use of multi-label classification.

---

### 📌 Section 2.4 – Dance Genre Recognition

Finally, another LSTM model takes movement predictions from all body parts to classify the overall dance genre.

#### 🔹 Input

$$
\left\{ \left\{ \hat{y}_t^e \right\}_{t=0}^{T-1} \right\}_{e \in E}
$$

#### 🔹 Output

- Final genre prediction from last hidden state:

$$
\hat{g} = \text{Softmax}(W h_T + b)
$$

#### 🔹 Loss Function (Cross Entropy)

$$
L_{\text{genre}} = -\sum_{k=1}^K g_k \log(\hat{g}_k)
$$

Where $g$ is the ground-truth one-hot encoded genre label.

---

### ✅ 3-Line Summary

- 3D pose is estimated via multi-seed optimization without 3D labels.  
- Each body part's motion is modeled as a multi-label sequence using 3D joint trajectories.  
- All part-level motions are fused via LSTM to predict the final dance genre.
