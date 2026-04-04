# Project Guide

## Overview

This project analyzes the **Heart Attack Risk & Prediction Dataset in India** with the aim of studying heart attack risk through a workflow that starts with **bivariate analysis** and ends with the calculation of the **odds ratio**.

According to the publication referenced in the post, **cardiovascular diseases are the leading cause of death in India**.

## Dataset

The dataset, downloaded from **Kaggle**, collects health information on individuals across various Indian states and focuses on risk factors that may contribute to the occurrence of heart attacks.

For full details about the data source, please refer to the original publication associated with the dataset.

## Variables

The dataset variables can be organized into three main groups:

- **Identification data**, such as `Patient_ID` and `State_Name`
- **Binary variables**, such as `Diabetes`, `Hypertension`, `Obesity`, `Smoking`, `Alcohol_Consumption`, `Physical_Activity`, `Air_Pollution_Exposure`, `Family_History`, `Healthcare_Access`, `Health_Insurance`, and `Heart_Attack_Risk`
- **Continuous variables**, such as `Age`, `Diet_Score`, `Cholesterol_Level`, `Triglyceride_Level`, `LDL_Level`, `HDL_Level`, `Systolic_BP`, `Diastolic_BP`, `Stress_Level`, `Emergency_Response_Time`, and `Annual_Income`

## Analytical Objective

The goal of the project is to verify whether certain categorical variables are associated with the target variable `Heart_Attack_Risk`.

To do this, the analysis is carried out in several stages:

1. First, the population in the dataset is examined
2. Then, the main risk factors are compared across subpopulations defined by age
3. Finally, the strength of the association is measured using the **odds ratio**

## Data Preparation

To make the analysis easier to read, the project creates a new variable called `age_group`, which divides subjects into two classes:

- `Under_50`
- `Over_50`

This transformation makes it possible to compare risk factors separately across the two age groups.

The post also shows the creation of a derived table containing this new variable.

## Analysis Method

The first part of the project uses **SQL queries** to:

- describe the population
- count subjects by gender and age
- create the `age_group` variable
- compare categorical risk factors with `Heart_Attack_Risk`

In particular, the bivariate analysis compares variables such as:

- diabetes
- hypertension
- obesity
- smoking
- alcohol consumption
- physical activity
- air pollution exposure
- family history
- access to healthcare

## From Bivariate Analysis to Odds Ratio

After the descriptive comparison, the project introduces the **odds ratio (OR)** as a measure of the strength of association between each categorical variable and heart attack risk.

The post defines the four elements of the **2x2 contingency table** as follows:

- **a**: subjects with the condition and with heart attack risk
- **b**: subjects with the condition and without heart attack risk
- **c**: subjects without the condition and with heart attack risk
- **d**: subjects without the condition and without heart attack risk

The OR is then calculated using the following formula: (a × d) / (b × c)
