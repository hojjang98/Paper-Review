# 📄 Paper Summary: DoWhy

**Title**: DoWhy: An End-to-End Library for Causal Inference  
**Link**: [https://arxiv.org/abs/2011.04216](https://arxiv.org/abs/2011.04216)  
**Authors**: Amit Sharma, Emre Kiciman  
**Published by**: Microsoft Research, NeurIPS 2020 Workshop

---

## ✅ Day 1 – Motivation & Philosophical Foundations

### 📌 Why I Chose This Paper

In machine learning and data science, **explainable models** are increasingly crucial—not just for trust and transparency, but for real-world decision-making.  
However, most standard tools like linear regression, tree models, or neural networks only give us **correlations**, not **causal conclusions**.  
This paper addresses that exact gap by introducing **DoWhy**, a Python-based framework that provides a clear and testable structure for causal inference.

DoWhy appeals to me because:
- It doesn’t stop at statistical association—it pushes toward **true cause-effect understanding**  
- Its 4-step framework is intuitive yet rigorous  
- It bridges theory with practice through well-documented Python APIs  
- I can use it not just in toy examples, but in real-world evaluations (like treatment effect, policy modeling, behavior analysis)

---

### 📌 Abstract Summary

The authors present **DoWhy**, a library designed to support end-to-end **causal inference**, separating it into four steps:  
1) Modeling assumptions using causal graphs  
2) Identification using criteria like backdoor  
3) Estimation via statistical or ML models  
4) Refutation using falsifiability tests

This clear separation of responsibilities ensures more **robust and explainable causal estimates** than traditional methods.

---

### 📌 Introduction Insights

The paper opens with a powerful argument:  
> *"Correlation is not causation. Yet most data analyses stop at correlation."*

It identifies a core problem in data science:  
- Statistical tools often compute associations between variables  
- But rarely question **whether those relationships are causal**  
- And even when they do, the **assumptions behind those claims are implicit**, unverifiable, or forgotten

DoWhy tackles this by making all assumptions **explicit**, **testable**, and **reproducible** through a structured workflow.

---

### 📌 DoWhy's 4-Stage Framework

| Step | Purpose | Key Question |
|------|---------|--------------|
| **Model** | Define a causal graph | What assumptions do I believe about the world? |
| **Identify** | Check if the causal effect is mathematically identifiable | Can the effect be computed given the graph? |
| **Estimate** | Use data to estimate the effect | What’s the estimated magnitude of the effect? |
| **Refute** | Check robustness of the result | Could this result be due to bias or chance? |

---

### 📌 Confounders & Backdoor Criteria (Key Concepts)

- **Confounder**: A variable that affects both treatment and outcome  
  → e.g., age affecting both smoking and lung cancer  
- **Backdoor Criterion**: A graphical condition to block "spurious" paths in the causal graph  
  → Controlling for confounders ensures that only **true causal effects** are estimated

---

### 📌 3-Line Summary

- Most data analysis tools measure correlation, but DoWhy helps estimate true **causal relationships**  
- It achieves this through a 4-step pipeline: modeling, identification, estimation, and refutation  
- The framework encourages **explicit reasoning**, making results more robust and explainable

---

### 📌 Unfamiliar Terms

- **Identifiability**: Whether a causal quantity can be computed from the observed data  
- **Counterfactual**: A “what if” scenario used to test causal validity  
- **Refuter**: A test that checks whether the estimated effect holds under synthetic manipulation

---

> 📝 Notes:  
> This paper introduced a powerful mental model for thinking about causality.  
> Tomorrow I’ll dive into the implementation logic and real-world applications.

