# Graph-Enhanced BERT for Vulnerability Detection: A Structural-Semantic Fusion Approach

> **Course:** Secure Programming and Software Vulnerability Exploitation (NT521.Q12.ANTT)  
> **Institution:** University of Information Technology (UIT) - VNU-HCM

---

## 📌 Overview

This project proposes a **Structural-Semantic Fusion** approach combining **GraphCodeBERT** and **Graph Attention Network (GAT)** for fine-grained vulnerability detection in C/C++ source code at the function level.

### Problem Statement
- Traditional static analysis methods relying on pattern matching are often too rigid.
- Analyzing code at the function level can introduce significant noise because only a few statements within a function typically trigger the vulnerability.
- There is a need for an approach that effectively combines both the **semantics** and the **structure** of the source code.

---

## 🔬 Proposed Method

### Processing Pipeline

```
Code Input → Property Graph (CPG) → PrVC Slicing → Feature Extraction → Model → Prediction
```

### Key Steps

1.  **Identify Slicing Entry Points (SEPs)**
    - Library/API function calls
    - Sensitive variables
    - Arithmetic operators
    - If-condition statements

2.  **Property Graph-based Program Slicing**
    - **Forward Slicing:** Traverse forward to find nodes affected by SEPs.
    - **Backward Slicing:** Traverse backward to find nodes affecting SEPs.
    - **Combination:** Aggregate nodes from both processes to form the Property Vulnerability Code (PrVC).

3.  **PrVC Graph Construction**
    - Built from Code Property Graph (CPG) including AST, CFG, and PDG edges.
    - Adds **Code Sequence Edges** to connect adjacent nodes based on their natural order in the source code.

4.  **Hybrid Structural-Semantic Model**
    - **Semantic Branch:** GraphCodeBERT encodes source code and data flow (768d).
    - **Structural Branch:** GAT encodes the code graph structure (128d × 2 layers).
    - **Fusion:** Concatenate (896d) → Fusion Layer (512d) → Classifier.

---

## � Project Structure & Usage

The repository contains the following key notebooks for reproduction and experimentation:

- **[train_custom_bert.py.ipynb](train_custom_bert.py.ipynb)**: Implementation of our proposed **GraphCodeBERT + GAT** fusion model.
- **[train_bert.py.ipynb](train_bert.py.ipynb)**: Baseline implementation using standard **GraphCodeBERT** / BERT fine-tuning.
- **[graphfvd.ipynb](graphfvd.ipynb)**: Implementation of the **GraphFVD** baseline model for comparison.
- **[README.md](README.md)**: Main documentation file.

---

## �📊 Dataset

Data was collected from the **National Vulnerability Database (NVD)** (NIST):

| Statistic | Count |
|:---|---:|
| Vulnerable Functions | 937 |
| Total Functions | 2,011 |
| Vulnerable PrVCs (Label 1) | 9,683 |
| Total PrVCs | 36,571 |
| Benign Samples | 26,888 |
| Vulnerable Samples | 9,683 |

### Common Vulnerability Types

| Group | Description | Common CWEs |
|:---|:---|:---|
| **controlstmt** | Control flow errors | CWE-362, CWE-670, CWE-427, CWE-691 |
| **sensivar** | Sensitive variable/memory errors | CWE-476, CWE-119, CWE-125, CWE-787, CWE-416 |
| **sensifunc** | Sensitive function/API errors | CWE-120, CWE-78, CWE-134, CWE-252 |
| **expression** | Arithmetic expression errors | CWE-190, CWE-191, CWE-369, CWE-197 |

---

## 📈 Experimental Results

### Performance Comparison

| Metric | GraphCodeBERT | GraphCodeBERT + GAT | GraphFVD |
|:---|:---:|:---:|:---:|
| **Accuracy** | - | **0.845** | - |
| **F1 Score** | - | **0.757** | - |
| **AUC** | - | **0.934** | - |
| **Precision** | - | **0.658** | - |
| **Recall** | **0.880** | - | - |

> **Result:** The GraphCodeBERT + GAT model achieved the best overall performance with a True Positive Rate involved of 89.9% and a True Negative Rate of 83.6%.

---

## 🎯 Conclusion

### Advantages
- High performance, capable of capturing better semantic representation compared to traditional methods.
- Utilizing PrVC (Property Vulnerability Code) slicing helps reduce noise and increases accuracy in vulnerability identification.

### Limitations & Future Work
- Currently limited to function-level analysis.
- **Future Goal:** Expand to program-level context analysis.
- Validate on more diverse datasets.

---

## 🔗 References & Resources

- **GraphFVD:** [Kaggle Notebook](https://www.kaggle.com/code/jvn3jean/graphfvd)
- **Paper:** Shao et al. "GraphFVD: Property graph-based fine-grained vulnerability detection." Computers & Security 151 (2025)
- **GraphCodeBERT:** Guo et al. "Graphcodebert: Pre-training code representations with data flow." arXiv:2009.08366

1. X. Chen et al., "VulChecker: Achieving More Effective Taint Analysis by Identifying Sanitizers Automatically," IEEE TrustCom, 2021.
2. Shao, Miaomiao, et al. "GraphFVD: Property graph-based fine-grained vulnerability detection." Computers & Security 151 (2025): 104350.
3. Guo, Daya, et al. "Graphcodebert: Pre-training code representations with data flow." arXiv preprint arXiv:2009.08366 (2020).
