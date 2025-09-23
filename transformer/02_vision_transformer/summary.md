# 📄 Paper Summary: Vision Transformer (ViT)

**Title**: An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale  
**Authors**: Dosovitskiy et al. (Google Research, Brain Team)  
**Published in**: ICLR 2021  
**Link**: https://arxiv.org/abs/2010.11929  

---

## ✅ Day 1 – Abstract & Introduction

### 📌 Background & Motivation
- **CNN dominance in CV**: For over a decade, convolutional neural networks (CNNs) have been the backbone of computer vision tasks (e.g., image classification, detection, segmentation).  
- **Limitations of CNNs**:  
  - Strong **inductive biases** (locality, translation equivariance) limit flexibility.  
  - Scaling CNNs to extremely large datasets shows diminishing returns.  
- **NLP breakthrough with Transformers**: In contrast, NLP models based on pure **Transformer architectures** scale extremely well, achieving state-of-the-art results without handcrafted inductive biases.  
- **Core Question**: Can the same Transformer architecture that revolutionized NLP be directly applied to vision tasks, without convolutions?  

---

### 📌 Core Idea
- Represent an image as a **sequence of patches** (like words in NLP).  
- Flatten each patch (e.g., 16×16 pixels), project it into an embedding space, and feed it into a standard Transformer encoder.  
- Introduce a special **[CLS] token** for classification tasks.  
- Add **positional embeddings** to retain spatial information.  

---

### 📌 Contributions
1. Proposed the **Vision Transformer (ViT)**: the first pure Transformer architecture for large-scale image recognition.  
2. Demonstrated that ViT, when trained on **large datasets** (JFT-300M), can **match or outperform CNNs** on ImageNet and other benchmarks.  
3. Showed that **scaling laws** of Transformers in NLP also apply to vision: performance improves predictably with model/data size.  

---
