# 📄 Paper Summary: Attention Is All You Need

**Title**: Attention Is All You Need  
**Link**: [https://arxiv.org/abs/1706.03762](https://arxiv.org/abs/1706.03762)  
**Published in**: NeurIPS 2017  

---

## ✅ Day 1 – Abstract & Introduction  

### 📌 Background & Motivation  
- Traditional **sequence transduction models** mainly relied on **RNNs/LSTMs/GRUs** or **CNNs**.  
- These models typically adopt an **encoder–decoder architecture**, often enhanced with **attention mechanisms**.  
- However, recurrent models suffer from:  
  - **Sequential computation** → no parallelization possible  
  - **Inefficiency** with long sequences (memory bottlenecks, slower training)  
  - High training cost and limited scalability  

---

### 📌 Proposed Approach  
- Introduces the **Transformer**, a new architecture:  
  - **Removes recurrence and convolution entirely**  
  - Relies **solely on attention mechanisms** for sequence modeling  
- Key benefits:  
  - Enables **parallelization across sequence elements**  
  - Reduces training time significantly  
  - Captures **long-range dependencies** more effectively  

---

### 📌 Key Contributions  
1. **Attention-only architecture** replacing RNNs and CNNs  
2. **Parallelizable computation**, boosting efficiency on GPUs  
3. **State-of-the-art translation performance**:  
   - WMT 2014 English→German: 28.4 BLEU (+2 BLEU improvement)  
   - WMT 2014 English→French: 41.8 BLEU (new single-model SOTA)  
4. **Generalization beyond translation**, e.g., English constituency parsing  

---

### 📌 High-Level Workflow  
1. **Traditional models**: sequential hidden state updates → bottlenecked by recurrence  
2. **Problem**: sequential nature blocks parallelization, slows training  
3. **Transformer**: uses attention to select relevant words from the entire input at once  
4. **Outcome**: faster training, better performance, improved generalization  

---

### 📌 Early Results  
- Achieved SOTA with only **12 hours of training on 8 P100 GPUs**  
- Training cost much lower than prior architectures  
- Proves that **attention-only design is both efficient and powerful**  

---

### 📌 TL;DR Summary  
The Transformer replaces recurrence and convolutions with **pure attention mechanisms**,  
achieving **parallelization, faster training, and superior accuracy** across translation and other NLP tasks.  
