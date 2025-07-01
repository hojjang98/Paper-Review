# 📄 Paper Summary: MobileNetV2

**Title**: MobileNetV2: Inverted Residuals and Linear Bottlenecks  
**Link**: [https://arxiv.org/abs/1801.04381](https://arxiv.org/abs/1801.04381)  
**Authors**: Mark Sandler, Andrew Howard, Menglong Zhu, Andrey Zhmoginov, Liang-Chieh Chen  
**Published by**: Google Research (2018)

---

## ✅ Day 1 – Overview & Purpose

### 📌 Abstract Summary
MobileNetV2 introduces a lightweight CNN architecture tailored for mobile and resource-constrained environments.  
Its core ideas—**Inverted Residuals** and **Linear Bottlenecks**—help reduce computation while maintaining representational power.  
It achieves strong performance on tasks like image classification, object detection, and semantic segmentation, with a balanced trade-off in accuracy, latency, and model size.

---

### 📌 Introduction Insights
The paper addresses the inefficiency of existing deep CNNs in mobile applications due to high computation and memory costs.  
To solve this, MobileNetV2 proposes a structure that maintains high accuracy while being computationally efficient.  
Its design expands low-dimensional features to high-dimensional ones, filters them using depthwise convolution, and projects them back linearly—avoiding large intermediate tensors during inference.

---

### 📌 Problem Statement
While modern deep networks achieve superhuman accuracy in tasks like image recognition, their resource demands exceed what mobile and embedded systems can handle.  
This paper proposes a new architecture optimized for such environments.  
The key module—**Inverted Residual with Linear Bottleneck**—expands inputs to high dimensions, filters them, and compresses back to low dimensions without incurring large memory costs, making it highly efficient for mobile inference.

---

### 📌 3-Line Summary
- Modern deep networks are accurate but unsuitable for mobile devices due to computation and memory constraints.  
- This paper introduces an efficient architecture that retains accuracy while minimizing resource usage.  
- The core design leverages Inverted Residuals and Linear Bottlenecks to reduce intermediate tensor size and computation.

---

### 📌 Unfamiliar Terms (None Today)

---

> 📝 Notes:  
> This summary is part of a structured review routine.  
> More in-depth architecture and experiments will be covered in Day 2.

