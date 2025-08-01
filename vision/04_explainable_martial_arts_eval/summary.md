# 📄 Paper Summary: Explainable Quality Assessment of Effective Aligned Skeletal Representations for Martial Arts Movements by Multi-Machine Learning Decisions

**Title**: Explainable Quality Assessment of Effective Aligned Skeletal Representations for Martial Arts Movements by Multi-Machine Learning Decisions  
**Link**: [https://doi.org/10.1038/s41598-024-83475-4](https://doi.org/10.1038/s41598-024-83475-4)  
**Authors**: Yiqun Pang, Kaiqi Zhang, Fengmei Li  
**Published in**: *Scientific Reports* (Nature Portfolio), Jan 2025  

---

## ✅ Day 0 – Why I Chose This Paper

### 📌 Background & Motivation

In recent months, I've been exploring how **pose estimation and motion analysis** can be applied to classify or evaluate human movement — especially in the context of **dance or martial arts**.

What intrigued me about this paper was that it steps beyond just recognizing poses or genres. It proposes a **quantitative evaluation method** for martial arts movements using **machine learning**, while also providing **explainability**. That's exactly the kind of direction I'm aiming to take: using skeletal data not just for classification, but also for **assessment**, **feedback**, and even **training evaluation**.

Also:
- It was recommended by a teammate who shares my interest in skeletal-based recognition.
- It directly applies to my personal research goals of building **interpretable movement evaluation systems**.

---

## ✅ Day 1 – Introduction & Problem Statement

### 📌 Core Motivation

Traditional martial arts evaluation relies on **subjective judgment** by human experts, which leads to:
- Variability in scoring (bias or inconsistency),
- Difficulty in fair assessment,
- Lack of real-time feedback tools.

This paper proposes a solution that:
- Aligns human motion features across different performers,
- Evaluates motion *effectiveness* using multiple ML models,
- Provides **explainable scoring** through model interpretation.

---

### 📌 Key Questions Addressed

1. How can we **align** skeleton-based movement sequences effectively between performers and reference templates?  
2. Can **machine learning models** be trained to assess martial arts skill based on aligned motion data?  
3. Is it possible to **explain** which body parts or movement patterns affected the final score?

---

### 📌 Main Contributions

- **Alignment Module**: Combines **Procrustes Analysis** and **DTW (Dynamic Time Warping)** to temporally and spatially align movement data.  
- **Multi-model Evaluation**: Trains several ML models (e.g., Decision Trees, Logistic Regression, LSTM) to assess motion quality.  
- **Explainable Output**: Uses **SHAP** (Shapley Additive Explanations) to highlight which skeletal joints/movements influenced the model’s score the most.

---

### 📌 High-Level Architecture

1. **Data Collection**  
   - Skeleton sequences from both martial arts experts (reference) and trainees (to be evaluated)  
2. **Preprocessing**  
   - Interpolation → Normalization → Procrustes + DTW alignment  
3. **Model Training**  
   - Feed aligned features into ML models  
4. **Scoring**  
   - Output quantitative score (regression or classification)  
5. **Explainability**  
   - Visualize contribution of different joints using SHAP

---

### 📌 TL;DR Summary

This paper proposes an explainable evaluation framework that compares a trainee’s martial arts movements to expert templates using skeleton alignment and multiple ML models. The final score is interpretable and body-part-aware, offering a practical and fair assessment tool for skill training and feedback.

---

## ✅ Day 2 – Related Work & Method Overview

### 📚 Literature Review

#### 🔹 Motion Capture in Martial Arts  
Motion capture methods are divided into **wearable** and **markerless** types.  
- **Wearable methods**: Infrared-based (accurate but limited to indoor), IMU-based (more portable but limited sensors).  
- **Markerless methods**: RGB-D sensors (e.g., Kinect) and 2D/3D human pose estimation using RGB (e.g., OpenPose, HRNet, BlazePose GHUM).  

BlazePose GHUM allows monocular 3D pose estimation and is especially useful in sports scenarios.

---

#### 🔹 Martial Arts Movement Quality Assessment  
Three main approaches have been used in previous studies:

1. **Rule-Based**  
   - Predefined thresholds based on expert knowledge  
   - Easy to understand but hard to generalize  

2. **Similarity Metrics**  
   - Measures distance between user and reference movements (e.g., DTW, Euclidean, Cosine)  
   - Gives a numeric score but lacks interpretability  

3. **Model-Based**  
   - Machine learning regressors or classifiers  
   - Better performance but typically black-box  
   - Includes linear regression, SVM, Lasso, and ensemble models

---

### ⚙️ Method Overview

#### 🔹 Skeleton Feature Extraction  
- MediaPipe is used to extract 3D skeleton coordinates (33 joints).  
- 18 body angles are calculated to describe posture:  
  - Includes shoulder, elbow, hip, knee, and trunk angles  
- Each angle is computed using:

$$
A_i = \arccos \left( \frac{\vec{v_1} \cdot \vec{v_2}}{|\vec{v_1}||\vec{v_2}|} \right) \cdot \frac{180}{\pi}
$$


---

#### 🔹 Feature Alignment  
- Uses **Dynamic Time Warping (DTW)** to align each subject’s motion to a 32-frame action template (sampled from top scorer).  
- Handles differences in movement speed and rhythm through time series warping.

---

#### 🔹 Regression & Ensemble  
- Base models: Linear Regression, Lasso, SVM, Decision Tree, Random Forest, KNN, Bagging  
- Final output uses **adaptive weighted averaging**:  
  - Models with **lower RMSE** get **higher weight**  
  - Final score is a **weighted sum** of base model outputs

---

#### 🔹 Explainability (SHAP)  
- SHAP is used to interpret both **global** and **local** contributions:  
  - Global: Which features matter most overall  
  - Local: Why a specific motion got a particular score

---

### 🧪 Datasets Used

| Dataset         | Description |
|----------------|-------------|
| **XSQ**         | Xingshen Quan (Long Fist routine, filmed by authors)  
| **PBB**         | Plum Blossom Boxing (traditional martial art, heritage-listed)  
| **UMONS-TaiChi**| Public 3D Tai Chi dataset using Kinect (25 joints, depth-based)

---

### 📌 TL;DR Summary

The paper presents a novel framework that combines skeleton-based motion analysis, alignment techniques, multiple regression models, and SHAP explanations to evaluate martial arts skill objectively.  
It addresses limitations of subjective scoring and opens new directions for intelligent feedback in training and education.

---

## ✅ Day 3 – Implementation Details: Alignment, Regression Ensemble, and Explainability

### 🔧 1. Skeleton Feature Extraction

- The system extracts 33 3D joint coordinates using **MediaPipe**.
- From these, 18 joint angles are computed to represent body posture.
- Each angle is calculated as:

$$
A_i = \arccos \left( \frac{\vec{v_1} \cdot \vec{v_2}}{|\vec{v_1}||\vec{v_2}|} \right) \cdot \frac{180}{\pi}
$$

- These angles include joints from upper limbs (shoulder, elbow), lower limbs (hip, knee), and trunk (torso twists), capturing both local and global posture features.

---

### 📐 2. Feature Alignment

- To reduce variation caused by different body types, movement speeds, or execution styles, the authors apply **two alignment steps**:

#### 🔹 Procrustes Analysis (Spatial Alignment)
- Normalizes for scale, rotation, and translation.
- Aligns skeletons from different individuals into a shared coordinate space.

#### 🔹 Dynamic Time Warping (Temporal Alignment)
- Handles different motion speeds and sequence lengths.
- Each sequence is warped to a 32-frame reference template (sampled from top performers).
- This allows fair comparison regardless of timing differences.

---

### 🧠 3. Regression Models + Adaptive Ensemble

- Seven base regressors are used:  
  **Linear Regression**, **Lasso**, **SVM**, **Decision Tree**, **Random Forest**, **KNN**, **Bagging**

- Final prediction is obtained by **adaptive weighted averaging** based on model RMSEs:

Weights are computed as:

$$
w_i' = \left( e^{|\text{RMSE}_i - \text{RMSE}_{\max}|} \right)^k
$$

$$
w_i = \frac{w_i'}{\sum_j w_j'}
$$

Final score is:

$$
\hat{y}_{\text{final}} = \sum_i w_i \cdot \hat{y}_i
$$

- This ensures that models with lower RMSE contribute more to the final prediction.

---

### 💡 4. Explainability via SHAP

- To make the system interpretable, SHAP is applied to the final regression output.
- SHAP provides:
  - **Global explanations**: which joint angles contribute most across all predictions
  - **Local explanations**: which angles affected a specific person’s score

- This enables feedback such as:
  - “Left calf angle at frame 21 reduced your score”
  - “Right thigh angle improved the overall score”

---

### 📌 Summary

This stage of the paper builds a full end-to-end pipeline:

> **Skeleton angles → Alignment (space/time) → Multi-model regression → Weighted prediction → SHAP-based explainability**

The approach not only predicts accurate skill scores, but also explains *why* a specific movement received that score, making it useful for both learners and instructors.

