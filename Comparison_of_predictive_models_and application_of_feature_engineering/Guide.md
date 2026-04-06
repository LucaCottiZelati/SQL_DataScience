# Comparison of Predictive Models and Application of Feature Engineering

## Project Overview
This repository presents an **R**-based analytical workflow focused on predicting **cardiac risk** using a dataset related to Indian patients.

The project was developed with two main objectives:
1. **to compare multiple predictive models** using the ROC curve and the AUC metric;
2. **to apply feature engineering techniques** in order to assess whether new variables can improve predictive performance.

The analysis starts from the observation that the tested models show performance levels very close to random chance. For this reason, in addition to comparing algorithms, a second step was introduced based on the creation of new features and correlation analysis.

## Project Sources
This repository is based on the following in-depth articles:

- **Comparison of Predictive Models through AUC Analysis**  
  Reference article on comparing logistic regression, SVM, and Random Forest.
- **Feature Engineering and Correlogram for Predicting Cardiac Risk**  
  Reference article on the construction of new variables and correlation analysis.

## Analytical Objectives
The aim of this work is to answer three questions:

- can supervised models reliably predict cardiac risk?
- does the issue depend on the algorithm, or on the informational quality of the data?
- can the creation of new features make the relationship with the target variable easier to interpret?

## Methodology

### 1. Data Preparation
The analytical pipeline includes:

- removal of columns not useful for training, such as identifiers and geographic variables;
- splitting the dataset into a **training set (70%)** and a **test set (30%)**;
- handling **class imbalance** through **undersampling** on the training set;
- converting the target variable into a format suitable for classification models.

### 2. Comparison of Predictive Models
The models compared are:

- **Multivariate logistic regression**;
- **SVM with a linear kernel**;
- **SVM with a radial kernel**;
- **Random Forest**.

The comparison is carried out through:
- predicted probabilities on the test data;
- construction of **ROC** curves;
- calculation of the **AUC** for each model.

### 3. Feature Engineering
To evaluate whether new transformations improve the interpretability of the phenomenon, two additional features are created:

- **Blood_Pressure_Index** = `Systolic_BP / Diastolic_BP`
- **Lipid_Ratio** = `LDL_Level / HDL_Level`

### 4. Correlation Analysis
After creating the new features, two approaches are applied:
- **Pearson correlation (Bravais-Pearson)**, to identify linear relationships;
- **Spearman correlation**, to detect possible monotonic or non-strictly linear relationships.
