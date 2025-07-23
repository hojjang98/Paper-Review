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


---

## ✅ Day 4 – Experiments, Results, Conclusion & Future Work

### 📌 3.1 Data & Experimental Setup

#### 🔹 UID (University of Illinois Dance) Dataset

- A curated in-the-wild dance video dataset covering **9 genres**: Ballet, Belly dance, Flamenco, Hip Hop, Rumba, Swing, Tango, Tap, Waltz.
- Contains both clean tutorial videos and challenging cases with occlusion, background clutter, and lighting variation.

| Attribute | Value |
|----------|-------|
| Total Genres | 9 |
| Total Clips | 1,143 |
| Total Frames | 2,788,157 |
| Total Duration | 108,089 seconds |
| Min / Max Clip Length | 4s / 824s |
| Min / Max Clips per Class | 30 / 304 |

#### 🔹 AIST++ Dataset

- Large-scale multiview dance dataset with 1,408 sequences from 10 dance genres.
- Includes 3D keypoint ground-truth and camera parameters.
- Used for benchmarking 3D pose estimation with MPJPE evaluation.

#### 🔹 Evaluation Metrics

- **MPJPE (Mean Per Joint Position Error)** – measures 3D pose accuracy.
- **F-score** – used for both movement recognition and genre classification.

---

### 📌 3.2 3D Pose Estimation Results

#### 🔹 MPJPE on AIST++

| Method | Supervision | Extra Data | MPJPE ↓ |
|--------|-------------|------------|---------|
| Martinez [ICCV'17] | Supervised | – | 110.0 |
| Wandt [CVPR'19] | Supervised | – | 323.7 |
| Pavllo [CVPR'19] | Supervised | – | 77.6 |
| Pavllo (semi-sup.) | Semi-supervised | ✖ | 446.1 |
| **Ours (semi-sup.)** | Semi-supervised | ✖ | **73.7** |
| Zhou [ICCV'17] | Weakly-supervised | ✔ | 93.1 |
| Kocabas [CVPR'19] | Self-supervised | Multiview | 87.4 |
| **Ours (unsupervised)** | Unsupervised | ✖ | 246.4 |

➡️ Our **semi-supervised** variant achieves the best performance among all methods.  
➡️ Even the **unsupervised** version is competitive against state-of-the-art models.

#### 🔹 MPJPE on Human3.6M

| Method | Supervision | Extra Data | MPJPE ↓ |
|--------|-------------|------------|---------|
| Pavllo [CVPR'19] | Supervised | – | **46.8** |
| Martinez [ICCV'17] | Supervised | – | 87.3 |
| Zanfir [CVPR'18] | Supervised | – | 69.0 |
| **Ours (semi-sup.)** | Semi-supervised | ✖ | **47.3** |
| Kocabas [CVPR'19] | Self-supervised | Multiview | 60.6 |
| Chen [CVPR'19] | Unsupervised | ✔ | 68.0 |
| Kundu [ECCV'20] | Unsupervised | ✔ | 67.9 |
| **Ours (unsupervised)** | Unsupervised | ✖ | 82.1 |

➡️ Our semi-supervised method nearly matches the best fully supervised result.

---

### 📌 3.3 Movement & Genre Recognition (on UID Dataset)

#### 🔹 Body Part Movement Recognition (F-score)

| Body Part | 2D Pose | 3D Pose |
|-----------|---------|---------|
| Head | 0.93 | **0.97** |
| Left Shoulder | **0.95** | 0.93 |
| Right Shoulder | 0.96 | 0.96 |
| Left Arm | 0.96 | 0.96 |
| Right Arm | 0.89 | **0.94** |
| Torso | 0.91 | **0.93** |
| Hips | 0.81 | **1.00** |
| Left Leg | 0.96 | 0.98 |
| Right Leg | 0.94 | 0.95 |
| Left Foot | 0.85 | **0.98** |
| Right Foot | **1.00** | 0.99 |

➡️ 3D poses generally improve motion recognition, especially for lower-body parts.

#### 🔹 Dance Genre Classification (F-score)

| Input Type | F-score |
|------------|---------|
| 2D Pose Only | 0.44 |
| 3D Pose Only | 0.47 |
| Movements (from 2D Pose) | 0.50 |
| Movements (from 3D Pose) | 0.55 |
| 2D Pose + Movements (2D) | 0.73 |
| **3D Pose + Movements (3D)** | **0.86** |

➡️ Best performance is achieved when both **3D pose features and inferred movements** are used together.

---

### 📌 Conclusion & Future Work

#### 🔹 Key Insights

- The proposed pipeline mirrors expert reasoning: from video → pose → part movements → genre.
- It performs well even under **unsupervised** or **semi-supervised** settings.
- The hierarchical decomposition offers both strong **recognition performance** and **explainability**.

#### 🔹 Limitations

- The pipeline is **not fully unsupervised** – genre classification still requires labeled data.
- Performance may degrade when dancers are small in the frame or heavily occluded.

#### 🔹 Future Directions

- Develop a **fully unsupervised end-to-end framework** for pose estimation and genre recognition.
- Use the extracted motion representations to **synthesize new dance sequences**.
- Evaluate synthesized dances via **expert judge ratings** to assess quality and realism.

---

### ✅ 3-Line Summary

- The method achieves strong 3D pose estimation and genre classification results without relying on ground-truth labels or RGB features.
- It benefits greatly from hierarchical modeling and body-part decomposition.
- The framework is highly applicable to real-world, custom-collected, and label-scarce dance video data.

---

## ✅ Day 5 – From Paper to Practice: First Implementation

### 🛠️ My Goal

Inspired by the HDVR paper, I wanted to begin building my **own version** of the pipeline, starting with feature extraction from pose keypoints.  
Since I don’t have 3D labels or clean multiview datasets, I focus on the **2D-to-feature** part as a starting point.

### 📂 File: `2D_Pose_Feature_Builder.ipynb`

This notebook implements the **first stage** of the HDVR pipeline:
- Input: pre-extracted 2D pose keypoints (from MediaPipe or OpenPose)
- Output: frame-level features like joint angles, velocities, and distances
- Also includes: simple dancer ID tracking logic and motion plotting tools

### 🧩 What I Learned

- Even basic joint distances/angles encode a lot of motion information
- Temporal smoothing is crucial for noisy keypoints (especially feet/hands)
- Some joints are more stable than others; torso vs. wrists behave very differently
- OpenPose/BlazePose formats need standardization for consistent feature pipelines

### 🔜 Next Steps

- Segment motion into clips and test LSTM or TCN for genre prediction
- Consider extending to 3D lifting with unsupervised constraints
- Add visualization tools for debugging movement classes

---