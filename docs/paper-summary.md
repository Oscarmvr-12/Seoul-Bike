---
title: Paper Summary
---

# Paper Summary: Explainable Cluster Analysis

[← Back to project overview](./)

## Assigned papers

1. Alvarez-Garcia, M., Ibar-Alonso, R., & Arenas-Parra, M. (2024). A comprehensive framework for explainable cluster analysis. *Information Sciences, 663*, 120282. <https://doi.org/10.1016/j.ins.2024.120282>
2. Alvarez-Garcia, M., Arenas-Parra, M., & Ibar-Alonso, R. (2024). Uncovering student profiles. An explainable cluster analysis approach to PISA 2022. *Computers & Education, 223*, 105166. <https://doi.org/10.1016/j.compedu.2024.105166>

## The four-step framework

**1. Data preprocessing: make the data trustworthy**
- **Missing values:**
  - Variables, and then records, with too many missing values are removed.
  - Strongly related pairs of variables are used to impute each other: linear regression for numeric pairs, and hot-deck imputation (copying a value from similar records) for categorical or mixed pairs.
  - Groups of related variables are found with a graph, and imputation is done within each group.
  - Remaining gaps are filled with hot-deck imputation using k-nearest neighbours.
- **Outliers:** removed with the Isolation Forest algorithm.

**2. Dimensionality reduction: fewer, interpretable variables**
- Numeric variables are standardized and reduced with **sparse PCA**. Each component depends on only a few original variables, so it can be given a meaningful name.
- Categorical variables are reduced with **multiple correspondence analysis (MCA)**.
- The number of components is chosen with the elbow method.
- Each component is named after the original variables most strongly related to it (|correlation| > 0.5 for numeric; partial η² > 0.14 for categorical).
- This step also reduces the "curse of dimensionality", where distances become less meaningful as the number of variables grows.

**3. Clustering: find the groups**
- Any algorithm that follows the scikit-learn interface can be used. k-means with k-means++ initialization is the default.
- Several algorithms and values of *k* are compared using validity metrics (within-cluster sum of squares, Davies–Bouldin, silhouette, Calinski–Harabasz) and the elbow method.

**4. Classification: explain the groups**
- A supervised classifier is trained with the **cluster labels as the target**.
- The pipeline uses an 80/20 train/test split, feature selection, and a cross-validated grid search for hyperparameters. XGBoost is the default model.
- **SHAP values** show which variables push an observation toward each cluster: globally (across all clusters) and locally (for one cluster).

## Why clusters need interpretation

A clustering algorithm only returns arbitrary labels (0, 1, 2, …). The authors describe understanding and labelling clusters as the most important step, because it means putting results back into the context of the study. A statistically valid clustering is not useful until people can say *what kind* of observation each cluster contains. The papers interpret clusters in two complementary ways:

- **Comparing cluster means with global means** for each variable, usually shown as a heatmap.
- **Describing clusters with "external" variables** that were *not* used to form them.

## How classification characterizes the cluster labels




