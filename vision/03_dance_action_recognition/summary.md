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

- **Alignment o highlight which skeletal joints/movements influenced the model’s score the most.

---
Module**: Combines **Procrustes Analysis** and **DTW (Dynamic Time Warping)** to temporally and spatially align movement data.
- **Multi-model Evaluation**: Trains several ML models (e.g., Decision Trees, Logistic Regression, LSTM) to assess motion quality.
- **Explainable Output**: Uses **SHAP** (Shapley Additive Explanations) t
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

### 🧩 From Paper to Practice

I found this paper particularly compelling because it directly supports the kind of work I want to pursue: using **pose-based motion analysis not just for recognition, but for quantitative and explainable assessment**.

> That’s why I decided to turn this paper into a hands-on coding experiment.

I've started implementing a simplified version of the system described here — beginning with 2D pose alignment, DTW-based comparison, and basic score visualization.  
This is now being actively developed as a **personal project**, and I'm gradually adding more features such as:

- Real-time similarity tracking from webcam or pre-recorded video  
- Motion feedback system with visual overlays  
- Score interpretation with body-part relevance  
- Extension from martial arts to dance or exercise routines  

This paper essentially laid the foundation for what could become my first full **pose-based evaluation system**.

