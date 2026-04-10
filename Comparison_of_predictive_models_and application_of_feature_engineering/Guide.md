## Comparison of Predictive Models and Application of Feature Engineering

## Project Overview

This repository presents a complete data analysis and predictive modeling workflow focused on heart attack risk prediction. The project starts with dataset exploration and proceeds through several stages of analysis, from descriptive statistics and simple models to more advanced machine learning algorithms such as **Random Forest** and **XGBoost**.

The goal of the project is not only to compare different predictive models, but also to understand whether the dataset contains variables that are truly informative with respect to the target. For this reason, in addition to predictive modeling, the project also includes a **feature engineering** phase aimed at creating new variables and evaluating whether they can improve predictive performance.

The repository follows a progressive logic: first the dataset is introduced, then the models are built and compared, and finally the results are critically interpreted.

---

## Project Goals

The project has three main objectives:

1. Understand the structure of the dataset and the meaning of its variables.
2. Build and compare different predictive models to classify heart attack risk.
3. Evaluate whether feature engineering can improve model performance and reveal stronger relationships between predictors and the target variable.

More broadly, the project aims to highlight a key principle of data science: **model complexity does not automatically guarantee better results if the available features are only weakly informative with respect to the target**.

---

## Repository Structure

The repository is organized into two main sections: one dedicated to the dataset and one dedicated to the analytical and modeling work.

### Folder Structure

    Comparison_of_predictive_models_and_application_of_feature_engineering/
    │
    ├── data/
    │   └── Dataset.md
    │
    └── work/
        ├── Comparison_predictive_models.md
        ├── Feature_Engineering.md
        ├── Multivariate_model_to_predict_heart_attack_risk.md
        ├── Random_Forest_and_XGBoost.md
        ├── Results.md
        ├── Univariate_Model_to_Predict_Heart_Attack_Risk.md
        └── Guide.md

---

## Description of Folders and Files

### `data/`

This folder contains the documentation related to the dataset used in the project.

- **`Dataset.md`**  
  Contains the description of the dataset, the available variables, the target variable, and the general context of the classification problem.

This section is essential because it provides the foundation for all subsequent analysis.

### `work/`

This folder contains the core of the project: the full analytical and predictive workflow.

- **`Univariate_Model_to_Predict_Heart_Attack_Risk.md`**  
  Presents the first phase of the work, based on univariate analysis and an initial simple model. In this file, variables are studied individually to determine whether they show any relationship with the target.

- **`Multivariate_model_to_predict_heart_attack_risk.md`**  
  Extends the analysis to a multivariate model, where several predictors are considered together. This phase makes it possible to observe the combined effect of variables on heart attack risk.

- **`Random_Forest_and_XGBoost.md`**  
  Introduces more advanced machine learning models capable of capturing nonlinear relationships and interactions between variables.

- **`Comparison_predictive_models.md`**  
  Collects the comparison between the different models developed in the project and evaluates their performance using classification metrics.

- **`Feature_Engineering.md`**  
  Describes the phase in which new derived variables are created and their possible relationship with the target is analyzed.

- **`Results.md`**  
  Summarizes the main results of the project and provides an interpretative synthesis of the work.

---

## Analytical Workflow

The project follows a clear and progressive workflow that can be divided into several stages.

### 1. Dataset Understanding

The first stage consists of studying the dataset: the source of the data, the meaning of the variables, the nature of the target variable, and the possible informational limitations of the dataset.

This step is essential because every predictive analysis depends directly on the quality, representation, and interpretability of the available data.

### 2. Univariate Analysis

The project begins with a simple approach in which variables are analyzed individually to verify whether they are associated with the target.

This phase is useful for:

- identifying potentially relevant features;
- building an initial intuition about the data;
- setting up an initial and easily interpretable model;
- defining a baseline for comparison.

Univariate analysis represents the first level of understanding of the problem.

### 3. Multivariate Modeling

The project then moves to a multivariate model, in which multiple variables are considered simultaneously.

This phase allows the analysis to examine:

- the joint effect of several predictors;
- possible changes in significance compared with the univariate model;
- a more realistic representation of the phenomenon under study.

In clinical and risk classification problems, a multivariate approach is often more appropriate than analyzing one variable at a time.

### 4. Machine Learning Models

After the more interpretable models, the project explores more flexible and complex methods:

- **Random Forest**, useful for identifying nonlinear patterns and interactions between variables;
- **XGBoost**, a boosting algorithm that is often highly effective in classification tasks involving tabular data.

These models are used to test whether greater algorithmic complexity can lead to better predictive performance.

### 5. Model Comparison

Once the models are built, the project compares their performance using metrics suitable for classification tasks.

This phase is central because it helps determine:

- whether more complex models truly outperform simpler ones;
- whether any improvements are substantial or only marginal;
- whether the dataset contains enough informative signal to support reliable prediction.

Model comparison is not only meant to identify “the best” model, but also to critically evaluate the quality of the overall predictive process.

### 6. Feature Engineering

After observing the limitations of models built with the original features, the project introduces a feature engineering phase.

The objectives of this stage are:

- to construct new derived variables from the original ones;
- to determine whether combinations or transformations of features can improve the informative signal;
- to explore new relationships between variables and the target.

This part of the project is methodologically very important because it addresses a common issue in real-world problems: often the limitation lies not in the model itself, but in the way the data represents the phenomenon.

---

## Recommended Reading Order

To understand the repository in a logical and progressive way, the following reading order is recommended:

1. `data/Dataset.md`
2. `work/Univariate_Model_to_Predict_Heart_Attack_Risk.md`
3. `work/Multivariate_model_to_predict_heart_attack_risk.md`
4. `work/Random_Forest_and_XGBoost.md`
5. `work/Comparison_predictive_models.md`
6. `work/Feature_Engineering.md`
7. `work/Results.md`
8. `work/Guide.md`

This order follows the natural development of the project, from understanding the data to the final interpretation of results.

---

## Methodological Value of the Project

This project is useful because it demonstrates several essential aspects of a real data science workflow:

- data understanding comes before modeling;
- simple models are an important starting point;
- complex models must be tested, not assumed to be better;
- feature engineering is often necessary;
- final evaluation should be critical and not limited to metrics alone.

In this sense, the repository is not just a collection of files, but a structured example of analytical reasoning in a classification problem.

---

## Techniques and Tools Used

The project makes use of common techniques in data analysis and machine learning, including:

- exploratory data analysis;
- univariate statistical analysis;
- logistic regression;
- multivariate modeling;
- Random Forest;
- XGBoost;
- feature engineering;
- model comparison;
- evaluation metrics for classification problems.

Depending on the contents of the files, the project may also include plots, correlation analysis, summary tables, and interpretations of coefficients or feature importance.

