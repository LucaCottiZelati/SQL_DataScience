# Results

## Overview

This file summarizes the main results of the project **Comparison of Predictive Models and Application of Feature Engineering**, which focuses on heart attack risk classification using multiple statistical and machine learning approaches.

The project was designed not only to compare model performance, but also to understand whether the available dataset contains features that are sufficiently informative to support reliable prediction.

---

## Summary of the Analytical Process

The project followed a progressive workflow:

1. Initial exploration of the dataset and understanding of the target variable.
2. Development of a **univariate model** to test whether individual predictors were associated with heart attack risk.
3. Construction of a **multivariate model** to evaluate the combined contribution of multiple predictors.
4. Application of more advanced machine learning methods, including **Random Forest** and **XGBoost**.
5. Comparison of predictive performance across models.
6. Implementation of **feature engineering** to test whether new derived variables could improve the predictive signal.

---

## Main Findings

### 1. Univariate Analysis

The univariate stage showed that some variables appeared to have a statistical relationship with the target when analyzed individually. However, these relationships were generally weak and did not translate into strong predictive power.

This suggests that although some predictors may show isolated associations with heart attack risk, they are not sufficient on their own to produce accurate classification results.

### 2. Multivariate Modeling

The multivariate model improved the analytical framework by considering several predictors simultaneously. This allowed a more realistic representation of the problem compared with the univariate approach.

However, the overall predictive performance remained limited. Even when variables were analyzed jointly, the model did not achieve a strong separation between classes, suggesting that the available predictors do not provide a robust signal for the target.

### 3. Random Forest and XGBoost

More advanced machine learning models were then tested to verify whether nonlinear methods could capture hidden patterns or interactions not detected by simpler models.

Although these algorithms are generally powerful for tabular classification tasks, they did not produce a substantial improvement in this project. Their performance remained close to the baseline and did not indicate a strong predictive structure in the dataset.

This is an important result: increasing algorithmic complexity did not meaningfully improve the quality of prediction.

### 4. Model Comparison

The comparison across models showed that no approach clearly outperformed the others in a significant way. The predictive metrics remained weak overall, and the models tended to perform close to the level of random discrimination.

This indicates that the limitation of the project is likely not the choice of algorithm itself, but the limited informativeness of the available features.

### 5. Feature Engineering

The feature engineering phase was introduced to determine whether new variables derived from the original ones could reveal stronger relationships with the target.

Although this step is methodologically important and represents a valid attempt to improve the dataset representation, the engineered features did not lead to a major improvement in predictive performance.

The analysis suggests that the original dataset may not contain enough useful structure to support reliable classification, even after feature transformation.

---

## Interpretation of Results

The results of this project suggest a broader methodological conclusion:

> Poor predictive performance is not always a consequence of using the wrong model. In many cases, the main limitation lies in the quality and informativeness of the available data.

In this repository, several different modeling strategies were tested, ranging from simple and interpretable methods to more advanced machine learning algorithms. Since performance remained weak across all of them, the most reasonable conclusion is that the dataset does not contain features that are strongly related to heart attack risk.

This makes the project particularly useful from a learning perspective, because it demonstrates that:

- not all classification problems can be solved effectively with more complex models;
- weak features limit performance regardless of the algorithm used;
- feature engineering can help, but it cannot always compensate for a lack of informative data;
- model evaluation should always be accompanied by critical interpretation.

---

## General Conclusion

The project shows that, within the context of this dataset, none of the tested predictive approaches achieved clearly strong classification performance.

The main contribution of the work is therefore not the identification of a single best-performing model, but the demonstration of an important practical lesson in data science: **the predictive value of a model depends heavily on the informational value of the features available in the dataset**.

For this reason, the results should be interpreted not as a failure of the modeling process, but as a meaningful outcome that highlights the importance of data quality, feature relevance, and critical model assessment.

---

## Limitations

Some possible limitations of the project include:

- limited predictive signal in the available variables;
- possible class imbalance in the dataset;
- lack of stronger domain-specific features related to cardiovascular risk;
- limited benefit from engineered variables;
- possible need for more extensive preprocessing, validation, or external data.

These limitations help explain why performance remained modest even after testing multiple approaches.

---

## Future Directions

Possible next steps for improving the project include:

- testing additional feature engineering strategies;
- exploring more advanced preprocessing techniques;
- applying cross-validation more extensively;
- investigating class balancing methods in greater detail;
- incorporating external or richer clinical variables;
- evaluating the models on a different or larger dataset.

These extensions could help determine whether the low predictive performance is due specifically to this dataset or whether the problem is inherently difficult with the available information.

---

## Final Remark

Overall, this project provides a complete and instructive workflow for predictive modeling. It combines statistical reasoning, machine learning experimentation, and feature engineering within a coherent structure.

Even though the predictive results were limited, the project remains valuable because it illustrates a realistic and important scenario in applied machine learning: sometimes the key challenge is not selecting a better model, but understanding the limits of the data itself.
