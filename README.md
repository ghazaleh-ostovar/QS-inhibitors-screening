# QS-inhibitors-classification
ML-based classification of QS inhibitors and activators targeting LuxR-family receptors to identify potent candidates.

---

##  Project Overview

This project applies machine learning to classify small molecules based on their inhibitory effects on two key quorum sensing (QS) receptors from the LuxR family: LuxR and LasR. The goal is to distinguish potent inhibitors, weak inhibitors, and activators using molecular fingerprints and receptor-specific features.


---

##  Biological Motivation

Quorum sensing (QS) is a key communication system in bacteria, regulating collective behaviors such as biofilm formation and virulence. Disrupting this signaling through **quorum sensing inhibitors (QSIs)** offers a promising anti-virulence strategy, especially for tackling antibiotic-resistant infections.

Quorum sensing inhibitors (QSIs) show promise as therapeutics, but inconsistent effects across receptors and compound types remain a barrier to clinical translation. This project addresses this gap by distinguishing potent inhibitors, weak inhibitors, and activators—an essential step toward improving drug selectivity and minimizing unintended activation of QS pathways.

---

##  Data Cleaning & Preprocessing

-- **Source**: Bioactivity data for LuxR and LasR receptors from [ChEMBL](https://www.ebi.ac.uk/chembl/)

- **Cleaning**:
  - Removed missing values
  - Averaged inhibition values across experimental replicates
- **Feature Engineering**:
  - 512-bit Morgan fingerprints generated from SMILES using RDKit
  - Receptor type one-hot encoded
- **Labeling**:
  - `0 = Activator` (Inhibition < 0%)
  - `1 = Weak Inhibitor` (0–50%)
  - `2 = Potent Inhibitor` (>50%)

---

## 🔍 Feature Selection

- Used Random Forest feature importance to select top 20 features:
  - 18 fingerprint bits
  - 2 receptor-type features (LuxR and LasR)

---

##  Models Trained

Three classifiers were trained using GridSearchCV and 5-fold cross-validation:
- **Support Vector Machine (SVM)**
- **Random Forest (RF)**
- **XGBoost (XGB)**

---

##  Results

| Class               | Precision | Recall | F1-Score |
|--------------------|-----------|--------|----------|
| Activator (0)       | 0.80      | 0.89   | 0.84     |
| Weak Inhibitor (1)  | 0.93      | 0.81   | 0.87     |
| **Potent Inhibitor (2)** | **0.80** | **0.89** | **0.84**   |

- ✅ **Best model**: **SVM**, achieving **85% test accuracy** and **0.89 recall** on potent inhibitors
- 🔍 SVM showed the best balance in distinguishing weak vs potent inhibitors
- RF and XGBoost performed well but struggled to resolve ambiguous (intermediate) cases

---

##  Visualizations

- Confusion matrices and heatmaps revealed receptor-specific behavior:
  - Some molecules acted as inhibitors for LuxR but activators for LasR
  - Despite structural similarity, responses varied — highlighting the need for receptor-aware classification

---

## ⚙ Setup & Requirements

Install required dependencies with:

```bash
pip install -r requirements.txt
