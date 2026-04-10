# Random Forest and XGBoost to Predict Cardiac Risk

## THE RANDOM FOREST ALGORITHM

We can divide the process into 4 phases:

1. **Bootstrap**

Random samples with replacement (bootstrapping) are created from the original dataset.

Each sample feeds a decision tree.

The number of trees (*n*, e.g. 100) is chosen manually.

2. **Building the trees**

For each sample, a decision tree is built.

At each node, a random subset of variables is selected to decide how to split the data.

3. **Different nodes for each tree**

Because both the data and the variables used in the nodes change, each tree is different from the others.

This increases model diversity and reduces overfitting.

4. **Prediction and aggregation**

Each tree makes a prediction for a new observation (e.g. risk = 0 or 1).

Random Forest aggregates the answers of the trees:

- when the target variable is categorical (our case), classification uses the majority vote (in this case we must set the 0/1 variable as a factor, otherwise it will be treated as numeric)

- when the target variable is continuous, regression is used through the average of the predictions

```r
# Trasforma la variabile target in una variabile categorica
heart_attack_prediction_india$Heart_Attack_Risk <- as.factor(heart_attack_prediction_india$Heart_Attack_Risk)
# Ora crea il modello correttamente come classificazione
rf_model <- randomForest(heart_attack_prediction_india$Heart_Attack_Risk ~ ., data = heart_attack_prediction_india, ntree = 100)
```

The `"rf_model"` model was built considering all the variables in the dataset (including the socio-economic ones).

We can use `VarImpPlot` to determine which variables were the most influential in splitting the trees in the model:



For this model, the three most important variables are:

- Annual Income
- Emergency Response Level
- Triglyceride Level

But now let’s move to performance.

```text
> print(rf_model)

Call:
 randomForest(formula = heart_attack_prediction_india$Heart_Attack_Risk ~      ., data = heart_attack_prediction_india, ntree = 100)
               Type of random forest: classification
                     Number of trees: 100
No. of variables tried at each split: 4
        OOB estimate of  error rate: 31.07%
Confusion matrix:
     0   1 class.error
0 6837 156  0.02230802
1 2951  56  0.98137679
```

The results can be explained as follows:

- number of trees = 100 (manually set by the model)

- at each node the model used 4 random variables — our model is a classification model, because the predictive variable is binary 0/1

- OOB (Out of Bag) of 31% (therefore the model made mistakes 31% of the time when predicting patients at risk)

The confusion matrix results are:

- 6837 patients were not at risk and the model predicted them correctly

- 156 patients were not at risk but the model classified them as at risk (false positives); the model made mistakes for `(156/(156+6837))*100% = 2.2%`

So the model predicted very well those who are not at risk.

- 2951 patients are at risk but the model classified them as not at risk (false negatives)

- 56 at-risk patients were correctly classified as at risk; the model made mistakes for `(2951/(2951+56))*100% = 98%`

So the model is practically unable to understand who is at risk.

The problem with this model is its inability to identify subjects at risk. Most likely this is because the number of patients with zero (therefore not at risk) is much higher than the number of subjects at risk.

Indeed, in MySQL:

```sql
SELECT count(*) as totale, Heart_Attack_Risk
FROM database_1.heart_attack_prediction_india_ag

group by Heart_Attack_Risk
```

```text
totale Heart_Attack_Risk
6993 0
3007 1
```

The dataset needs to be balanced.

We have two alternatives:

- increase the number of patients at risk

- decrease the number of patients not at risk

We can try to reduce the number of non-risk patients, so as to collect two random samples of 3000 patients with and without cardiac risk. This way we will have balancing.

**Input code:**

```r
# Downsampling: crea un dataset bilanciato
set.seed(123)  # Per riproducibilità

# Separa le due classi
class_0 <- heart_attack_prediction_india %>% filter(Heart_Attack_Risk == 0)
class_1 <- heart_attack_prediction_india %>% filter(Heart_Attack_Risk == 1)

# Prende un campione casuale classe0 con uguale dimensione della classe1
class_0_downsampled <- class_0 %>% sample_n(nrow(class_1))
# Combina le due classi per creare un nuovo dataset bilanciato
balanced_data <- bind_rows(class_0_downsampled, class_1)

# Allena il modello Random Forest sul dataset bilanciato
rf_model <- randomForest(Heart_Attack_Risk ~ ., data = balanced_data, ntree = 100)

# Valutazione: stampa la matrice di confusione OOB
print(rf_model)
```

**Output:**

```text
Call:
 randomForest(formula = Heart_Attack_Risk ~ ., data = balanced_data,      ntree = 100)
               Type of random forest: classification
                     Number of trees: 100
No. of variables tried at each split: 5

        OOB estimate of  error rate: 49.55%
Confusion matrix:
     0    1 class.error
0 1527 1480   0.4921849
1 1500 1507   0.4988360
```

We now obtain a balanced model, but one that makes mistakes 49% of the time (therefore for every two patients, it predicts one correctly and gets the other wrong almost regardless of whether the patient is at risk or not).

We can try the other approach, namely balancing the minority class by increasing the number of patients at risk (therefore the opposite operation of downsampling).

```r
#  Dividi il dataset in classi
class_0 <- heart_attack_prediction_india %>% filter(Heart_Attack_Risk == 0)
class_1 <- heart_attack_prediction_india %>% filter(Heart_Attack_Risk == 1)

#  Esegui l'upsampling della classe 1 (a rischio)
set.seed(123)
class_1_upsampled <- class_1 %>% sample_n(nrow(class_0), replace = TRUE)

# Combina le classi in un dataset bilanciato
balanced_data <- bind_rows(class_0, class_1_upsampled)
#  Shuffle delle righe (opzionale ma utile)
balanced_data <- balanced_data %>% sample_frac(1)

#  Controlla che le classi siano ora bilanciate
table(balanced_data$Heart_Attack_Risk)

#  Allena il modello Random Forest sul dataset bilanciato
rf_model <- randomForest(Heart_Attack_Risk ~ ., data = balanced_data, ntree = 100)

#  Visualizza le performance OOB
print(rf_model)
```

In this way the model generated a random sample of at-risk patients and repeated them until we reached parity with the majority class.

We obtain these performance metrics:

```text
Call:
 randomForest(formula = Heart_Attack_Risk ~ ., data = balanced_data,      ntree = 100)
               Type of random forest: classification
                     Number of trees: 100
No. of variables tried at each split: 5
        OOB estimate of  error rate: 6.89%
Confusion matrix:
     0    1 class.error
0 6688  305  0.04361504
1  658 6335  0.09409409
```

OOB of 6.89%, that is, the model is making mistakes 6.89% of the time, namely:

- 4.37% were not at-risk patients and the model classified them as at risk

- 9.41% were at risk and the model classified them as not at risk

therefore OOB of `(4.37 + 9.41) / 2 = 6.89%`, very good!

Before proceeding to model optimization (or rather seeing whether it is possible), it makes me think that the model is working well because it learned the dataset well. Using upsampling means forcing things a bit; we deliberately created a random sample to balance the number of non-risk patients, so I may have obtained good performance but also substantial overfitting.

To be really sure about the model’s performance, we should feed the model fresh data and see how well it discriminates between patients at risk and those not at risk.

We can use this process:

1. start again from the unbalanced dataset and split it into two portions: we could think of a 70/30 split, therefore:

   - 70% of the dataset will serve as the training dataset  
   - 30% of the dataset will be used for testing, so we can see how it predicts cardiac risk on patients it has not seen

Since we needed to perform the upsampling operation, we must use the dataset on which we build the model (training) to balance the classes.

```r
# Suddividi il dataset (prima dell'upsampling) con la regola 70/30
set.seed(42)
train_idx <- sample(nrow(heart_attack_prediction_india), 0.7 * nrow(heart_attack_prediction_india))
train_data <- heart_attack_prediction_india[train_idx, ]
test_data  <- heart_attack_prediction_india[-train_idx, ]
```

Now we have the two datasets: `train_data` and `test_data`.

We can now proceed with the upsampling operation on the training dataset, create the model, and finally evaluate performance:

```r
# Esegui upsampling su train_data
class_0 <- train_data %>% filter(Heart_Attack_Risk == 0)
class_1 <- train_data %>% filter(Heart_Attack_Risk == 1)
class_1_upsampled <- class_1 %>% sample_n(nrow(class_0), replace = TRUE)
balanced_train <- bind_rows(class_0, class_1_upsampled) %>% sample_frac(1)

# Allena il modello
rf_model <- randomForest(Heart_Attack_Risk ~ ., data = balanced_train, ntree = 100)
# Valutazione su test set "pulito"
pred <- predict(rf_model, newdata = test_data)
confusionMatrix(pred, test_data$Heart_Attack_Risk, positive = "1")
```

We arrive at these performance metrics:

```text
Confusion Matrix and Statistics

          Reference
Prediction    0    1
         0 2083  875
         1   29   13
               Accuracy : 0.6987
                 95% CI : (0.6819, 0.7151)
    No Information Rate : 0.704
    P-Value [Acc > NIR] : 0.7458

                  Kappa : 0.0013

 Mcnemar's Test P-Value : <2e-16
            Sensitivity : 0.014640
            Specificity : 0.986269
         Pos Pred Value : 0.309524
         Neg Pred Value : 0.704192
             Prevalence : 0.296000
         Detection Rate : 0.004333
   Detection Prevalence : 0.014000
      Balanced Accuracy : 0.500454

       'Positive' Class : 1
```

We can calculate several metrics, but I would focus on **RECALL**, which is essentially the ratio between:

`(number of at-risk patients correctly predicted) / (total number of at-risk patients)`

in this case: `13 / (13 + 875) * 100% = 1.46%`, a terrible result: it means the model is able to predict little more than 1% of at-risk patients.

The **KAPPA** value indicates how much the model predicts the result better than chance; in this metric it is close to zero.

- The closer Kappa is to 1, the better the model works

- The closer Kappa is to zero, the more the model performs like randomness

So these metrics are disastrous compared with before.

This could be due to an imbalance in the test dataset; in fact we focused a lot on training:

```text
> table(test_data$Heart_Attack_Risk)

   0    1
2112  888
```

The intuition seems correct.

However, it is not good practice to create upsampling on the test dataset, because those should be fresh (production) data.

In this case, Random Forest does not allow me to obtain good metrics and, in general, a usable model.

We could use a model that also exploits trees but with a different algorithm (reasoning sequentially), namely **XGBoost**:

```r
# Carica le librerie necessarie
library(xgboost)
library(dplyr)
library(caret)
library(MLmetrics)
library(pROC)
# Suddividi il dataset in training e test
set.seed(42)
train_idx <- sample(nrow(heart_attack_prediction_india), 0.7 * nrow(heart_attack_prediction_india))
train_data <- heart_attack_prediction_india[train_idx, ]
test_data  <- heart_attack_prediction_india[-train_idx, ]
# Prepara le matrici (XGBoost richiede input in formato numerico)
# Rimuove la colonna target dai dati e crea la matrice delle feature
train_matrix <- model.matrix(Heart_Attack_Risk ~ . -1, data = train_data)
test_matrix  <- model.matrix(Heart_Attack_Risk ~ . -1, data = test_data)

# Target (deve essere numerico 0 o 1)
train_label <- as.numeric(train_data$Heart_Attack_Risk)
test_label  <- as.numeric(test_data$Heart_Attack_Risk)
# Calcola scale_pos_weight = numero classe 0 / numero classe 1 (dal training set)
pos_weight <- sum(train_label == 0) / sum(train_label == 1)

# Crea il DMatrix (formato speciale per XGBoost)
dtrain <- xgb.DMatrix(data = train_matrix, label = train_label)
dtest  <- xgb.DMatrix(data = test_matrix, label = test_label)
# Imposta i parametri di XGBoost
params <- list(
  objective = "binary:logistic",   # classificazione binaria
  eval_metric = "auc",             # metrica da ottimizzare
  scale_pos_weight = pos_weight,  # bilancia le classi
  eta = 0.1,                       # learning rate
  max_depth = 6                   # profondità massima degli alberi
)
# Addestra il modello
xgb_model <- xgb.train(
  params = params,
  data = dtrain,
  nrounds = 100,
  watchlist = list(train = dtrain),
  verbose = 0
)

# Previsioni (probabilità)
pred_prob <- predict(xgb_model, newdata = dtest)

# Previsioni (classi 0/1 con soglia 0.5)
pred_class <- ifelse(pred_prob > 0.5, 1, 0)
# Confronto e valutazione
confusion <- confusionMatrix(
  factor(pred_class, levels = c(0, 1)),
  factor(test_label, levels = c(0, 1)),
  positive = "1"
)

# Mostra confusion matrix
print(confusion)

# Calcola F1-score
f1 <- F1_Score(y_pred = pred_class, y_true = test_label, positive = "1")
cat("\nF1-Score:", round(f1, 4), "\n")

# Calcola AUC
auc <- auc(test_label, pred_prob)
cat("ROC-AUC:", round(auc, 4), "\n")
```

**Output with the metrics:**

```text
Confusion Matrix and Statistics

          Reference
Prediction    0    1
         0 1416  610
         1  696  278

               Accuracy : 0.5647
                 95% CI : (0.5467, 0.5825)
    No Information Rate : 0.704
    P-Value [Acc > NIR] : 1.00000

                  Kappa : -0.016

 Mcnemar's Test P-Value : 0.01867
            Sensitivity : 0.31306
            Specificity : 0.67045
         Pos Pred Value : 0.28542
         Neg Pred Value : 0.69891
             Prevalence : 0.29600
         Detection Rate : 0.09267
   Detection Prevalence : 0.32467
      Balanced Accuracy : 0.49176

       'Positive' Class : 1

F1-Score: 0.2986

ROC-AUC: 0.5138
```

The ROC-AUC value tells me how well the model can correctly separate patients at risk from those not at risk, and it should be close to 1 to be a good metric.

Here it is 0.51, so the model behaves not according to prediction but according to randomness.

The F1 value is a mean between model precision and recall. This value too is better the closer it gets to 1, because it means the model has a good balance between precision and recall. `F1 = 0.29` is very low, especially in a medical context.

Perhaps the solution is to abandon the tree-based approach and think about another algorithm.
