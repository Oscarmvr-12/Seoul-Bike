---
title: Paper Summary
---

# Paper Summary: Explainable Cluster Analysis

[← Back to project overview](./)

## Assigned papers

1. Alvarez-Garcia, M., Ibar-Alonso, R., & Arenas-Parra, M. (2024). A comprehensive framework for explainable cluster analysis. *Information Sciences, 663*, 120282. <https://doi.org/10.1016/j.ins.2024.120282>
2. Alvarez-Garcia, M., Arenas-Parra, M., & Ibar-Alonso, R. (2024). Uncovering student profiles. An explainable cluster analysis approach to PISA 2022. *Computers & Education, 223*, 105166. <https://doi.org/10.1016/j.compedu.2024.105166>

Paper 1 proposes the method. Paper 2, by the same authors, applies it to a new, larger dataset.

## The problem

Clustering is **unsupervised**. It groups similar observations, but it doesn't say what the groups mean, and its results depend on the quality of the input data. The authors argue that earlier explainable-clustering methods focused on interpretation alone, often ignored data-quality problems such as missing values, outliers, high dimensionality and mixed numeric/categorical data, and were usually tied to one specific algorithm. Paper 1 proposes a single end-to-end workflow and implements it in an open-source Python package, **Clust-learn** (<https://github.com/malgar/clust-learn>).

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

Once clusters exist, their labels become the **target** of a supervised model. Both papers train the classifier on the **original** variables rather than the reduced components, so the explanation is in terms people understand. SHAP values from the classifier then show which original variables most strongly separate each cluster from the others, and the authors use the top variables to **name** each cluster. The trained model can also assign new observations to existing clusters.

This step **characterizes** the clustering: it explains how the clusters differ. It is **not** independent proof that the clusters are real. The classifier's inputs are the same information the clusters were built from, so high accuracy mainly shows that the clusters are well separated and describable.

## Case studies

| | Paper 1 (framework) | Paper 2 (application) |
|---|---|---|
| Data | PISA 2018, Spanish students: random sample of 5,000; 80 variables | PISA 2022, all 30,800 Spanish students; 74 variables |
| Preprocessing | Imputation; 444 records with >1/3 missing removed; outliers removed → 4,241 students | Imputation; **no** outlier removal (every student kept) |
| Reduced to | 14 derived variables | 16 derived variables |
| Clustering | k-means, agglomerative and Gaussian mixture compared → k-means, *k* = 6 | Weighted k-means (survey sample weights) → *k* = 7 |
| Classifier | XGBoost; test accuracy 0.83 | XGBoost with SMOTE class balancing; test accuracy 0.86 |

In paper 2, gender, test scores and educational expectations were **not** used to form clusters. They were examined afterwards, and the clusters still differed by up to about 90 points in average test performance. Socio-economic status and ICT availability at home were the most important factors separating the student profiles.

## Relevance to our project

- **Mostly numeric features.** Station-level features would be almost all numeric (shares, counts, medians), so MCA is likely unnecessary. With roughly ten features, dimensionality reduction may be optional; paper 1 notes that clustering can run directly on normalized variables.
- **External variables for interpretation.** As paper 2 did with test scores, we can keep weather sensitivity and spatial context (such as distance to a subway station) out of the clustering and use them afterwards to interpret the clusters.
- **Honest framing of classification.** We will present the classifier as describing our clusters, not as validating them.
