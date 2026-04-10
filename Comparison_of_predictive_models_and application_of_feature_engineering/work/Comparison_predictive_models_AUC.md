# Comparison of Predictive Models Through AUC Comparison

The predictive models attempted so far are:

- univariate model using logistic regression through LDL levels: **Construction of a Univariate Model to Predict Cardiac Risk**
- multivariate model using logistic regression with LDL levels together with other clinical parameters: **Construction of a Multivariate Model to Predict Cardiac Risk**
- multivariate models using more complex and less interpretable algorithms, namely Random Forest and XGBoost: **Random Forest and XGBoost to Predict Cardiac Risk**

In all these cases, we did not achieve good metrics on the test data.

As a final analysis step, we can evaluate the AUC value for multiple models.

The main issue with the published dataset is the imbalance between patients at risk and those not at risk.

Let us try to set up an undersampling operation and feed the resulting dataset into the models. The objective of the analysis is to see whether we can obtain an AUC value different from 0.5, which indicates random predictive behavior.

Now I want to try a large-scale comparative approach. That is, given several predictive models, I want to calculate the corresponding AUC. If in all models the AUC value is about 0.5, then some evaluations are necessary.

The models to be tested are the following:

- multivariate logistic regression with all variables in the dataset
- SVM model with linear kernel
- SVM model with radial kernel
- random forest model (already tested previously)

## THE SVM MODEL

The SVM model is an algorithm whose objective is to find a hyperplane that maximizes the separation of the data.

A hyperplane is the most general definition of a line.

We could define a line as a hyperplane in two dimensions (in one dimension it would be a point).

A hyperplane is described as follows:

```text
X1W1 + X2W2 + X3W3 + ... + XnWn + b = 0
```

- `n` is the number of dimensions
- `X` is any point in the space (therefore one of the variables)
- `W` is the coefficient determined by the algorithm and geometrically defines the slope of the hyperplane

The way the hyperplane is built is represented by the algorithm kernel, which can take several forms, but here we will test:

- `linear`: the hyperplane will be linear
- `radial`: the hyperplane will be curved

The linear kernel is used when the data are believed to be well linearly separable.

However, the current goal is not to go deeper into the SVM model itself in this specific case, but to apply it together with other algorithms to evaluate the resulting AUC value.

We will apply the SVM model to the entire database, which contains 23 variables.

In fact, this use of the SVM model is somewhat extreme, because it means that the hyperplane will operate in 23 dimensions.

## R IMPLEMENTATION OF THE MODEL COMPARISON

```r
# Required libraries
library(dplyr)
library(e1071)
library(randomForest)
library(pROC)

# Dataset already loaded
df <- heart_attack_prediction_india

# Remove non-useful columns
df <- df %>% select(-Patient_ID, -State_Name)

# Train/test split (70/30)
set.seed(123)
idx <- sample(1:nrow(df), 0.7 * nrow(df))
train <- df[idx, ]
test  <- df[-idx, ]

# Undersampling on the training set
n1 <- sum(train$Heart_Attack_Risk == 1)
train_0 <- train %>% filter(Heart_Attack_Risk == 0) %>% sample_n(n1)
train_1 <- train %>% filter(Heart_Attack_Risk == 1)
train_bal <- bind_rows(train_0, train_1) %>% sample_frac(1)

# Dataset with target as factor
train_bal$Heart_Attack_Risk <- as.factor(train_bal$Heart_Attack_Risk)
test$Heart_Attack_Risk <- as.factor(test$Heart_Attack_Risk)

# Model training
model_log <- glm(Heart_Attack_Risk ~ ., data = train_bal, family = "binomial")
model_svm <- svm(Heart_Attack_Risk ~ ., data = train_bal, kernel = "linear", probability = TRUE)
model_svm_radial <- svm(Heart_Attack_Risk ~ ., data = train_bal, kernel = "radial", probability = TRUE)
model_rf  <- randomForest(Heart_Attack_Risk ~ ., data = train_bal)

# Function for predicted probabilities
get_probs <- function(model, test_set, tipo) {
  if (tipo == "svm") {
    p <- attr(predict(model, test_set, probability = TRUE), "probabilities")[, "1"]
  } else {
    p <- predict(model, test_set, type = "response")
  }
  p <- as.numeric(p)
  r <- roc(as.numeric(as.character(test_set$Heart_Attack_Risk)), p)
  if (auc(r) < 0.5) p <- 1 - p
  return(p)
}

# Probability computation
probs_log <- get_probs(model_log, test, "glm")
probs_svm <- get_probs(model_svm, test, "svm")
probs_svm_radial <- get_probs(model_svm_radial, test, 'svm_radial')
probs_rf  <- get_probs(model_rf, test, "glm")

# ROC and AUC computation
roc_log <- roc(test$Heart_Attack_Risk, probs_log)
roc_svm <- roc(test$Heart_Attack_Risk, probs_svm)
roc_svm_radial <- roc(test$Heart_Attack_Risk, probs_svm_radial)
roc_rf  <- roc(test$Heart_Attack_Risk, probs_rf)
auc_log <- auc(roc_log)
auc_svm <- auc(roc_svm)
auc_svm_radial <- auc(roc_svm_radial)
auc_rf  <- auc(roc_rf)

# Print descriptive AUC values
cat("\n AUC for each model:\n")
cat(sprintf("✅ Logistic Regression: AUC = %.3f\n", auc_log))
cat(sprintf("✅ SVM (Linear):        AUC = %.3f\n", auc_svm))
cat(sprintf("✅ SVM (Radial:        AUC = %.3f\n", auc_svm_radial))
cat(sprintf("✅ Random Forest:       AUC = %.3f\n", auc_rf))
```

**Output**

```text
 AUC for each model:
✅ Logistic Regression: AUC = 0.509
✅ SVM (Linear):        AUC = 0.496
✅ SVM (Radial:        AUC = 0.500
✅ Random Forest:       AUC = 0.504
```

Therefore, these are AUC values close to 0.5.

## Linear SVM Model

As mentioned earlier, the model found a hyperplane in multiple dimensions. Just to mathematically visualize the complexity of the model, we may ask R to print the full formula of the identified hyperplane:

```r
# Support vectors
SV <- model_svm$SV

# Coefficients associated with support vectors
coefs <- model_svm$coefs

# Intercept (b)
bias <- -model_svm$rho
w <- t(coefs) %*% as.matrix(SV)

# Get weights and bias
w <- t(model_svm$coefs) %*% model_svm$SV
b <- -model_svm$rho

# Print the hyperplane formula
print(w)
print(b)
```

What we obtain is the following equation:

```text
0.0579⋅Age−0.0157⋅GenderFemale+0.0157⋅GenderMale−0.0424⋅Diabetes
−0.0124⋅Hypertension+0.0194⋅Obesity+0.0729⋅Smoking+0.1175⋅Alcohol_Consumption
−0.0570⋅Physical_Activity−0.0987⋅Diet_Score+0.0147⋅Cholesterol_Level
+0.0241⋅Triglyceride_Level−0.1404⋅LDL_Level−0.0320⋅HDL_Level
+0.0558⋅Systolic_BP+0.1126⋅Diastolic_BP+0.1281⋅Air_Pollution_Exposure
+0.0069⋅Family_History+0.4368⋅Stress_Level+0.0500⋅Healthcare_Access
+0.0308⋅Heart_Attack_History−0.0266⋅Emergency_Response_Time
+0.0251⋅Annual_Income+0.0141⋅Health_Insurance−0.0306=0
```

The SVM model uses numbers for all variables; it does not distinguish between categorical and continuous numeric variables.

It encoded sex as 0/1 using an internal encoding procedure (called hot encoding).

## FINAL REMARKS

So, the following models did not work:

- logistic regression, both univariate and multivariate
- random forest
- XGBoost
- SVM with radial and linear kernel

The issue is not attributable to the algorithms, but rather to the data. Most likely, the data are not very informative.

One possible direction would be to perform feature engineering, that is, constructing new variables that may better predict cardiac risk.

`R`
