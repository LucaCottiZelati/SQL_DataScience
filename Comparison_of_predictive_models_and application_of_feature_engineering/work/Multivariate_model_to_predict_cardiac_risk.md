# Construction of a multivariate model to predict cardiac risk
#### See this logic different In formulas:
Univariate logistic function:

```text
p = 1 / (1 + e^-(β₀ + β₁ * LDL))
```

Multivariate model:

```text
p = 1 / (1 + e^-(β₀ + β₁*x₁ + β₂*x₂ + β₃*x₃ + ... + βₙ*xₙ))
```

The objective of the model will be to determine the beta coefficients that identify the relationship between the variables and the probability of risk.

- positive β → positive relationship
- negative β → negative relationship

#### Analysis strategy

We can manually enter the variables or automatically evaluate the possible combinations. We will proceed with the second strategy using AIC.

## Automatic selection of continuous variables

The AIC index is calculated as follows:

```text
AIC = -2 * log-likelihood + 2 * k
```

where `k` is the number of estimated parameters and the log-likelihood indicates how well the model fits the data.

A lower AIC indicates a better model (greater likelihood with fewer parameters).

#### In R

Input code:

```r
modello_completo <- glm(Heart_Attack_Risk ~ LDL_Level + HDL_Level + Systolic_BP +
                       Diastolic_BP + Cholesterol_Level + Triglyceride_Level,
                       data = heart_attack_prediction_india, family = binomial)
modello_finale <- step(modello_completo, direction = "both")
```

Output:

```text
Start:  AIC=12236.72
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Systolic_BP +
    heart_attack_prediction_india$Diastolic_BP + heart_attack_prediction_india$Cholesterol_Level +
    heart_attack_prediction_india$Triglyceride_Level
                                                   Df Deviance   AIC
- heart_attack_prediction_india$Systolic_BP         1    12223 12235
- heart_attack_prediction_india$Cholesterol_Level   1    12223 12235
- heart_attack_prediction_india$Diastolic_BP        1    12223 12235
- heart_attack_prediction_india$Triglyceride_Level  1    12223 12235
- heart_attack_prediction_india$HDL_Level           1    12224 12236
<none>                                                   12223 12237
- heart_attack_prediction_india$LDL_Level           1    12227 12239
Step:  AIC=12234.77
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Diastolic_BP +
    heart_attack_prediction_india$Cholesterol_Level + heart_attack_prediction_india$Triglyceride_Level
                                                   Df Deviance   AIC
- heart_attack_prediction_india$Cholesterol_Level   1    12223 12233
- heart_attack_prediction_india$Diastolic_BP        1    12223 12233
- heart_attack_prediction_india$Triglyceride_Level  1    12223 12233
- heart_attack_prediction_india$HDL_Level           1    12224 12234
<none>                                                   12223 12235
+ heart_attack_prediction_india$Systolic_BP         1    12223 12237
- heart_attack_prediction_india$LDL_Level           1    12227 12237
Step:  AIC=12232.84
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Diastolic_BP +
    heart_attack_prediction_india$Triglyceride_Level
                                                   Df Deviance   AIC
- heart_attack_prediction_india$Diastolic_BP        1    12223 12231
- heart_attack_prediction_india$Triglyceride_Level  1    12223 12231
- heart_attack_prediction_india$HDL_Level           1    12224 12232
<none>                                                   12223 12233
+ heart_attack_prediction_india$Cholesterol_Level   1    12223 12235
+ heart_attack_prediction_india$Systolic_BP         1    12223 12235
- heart_attack_prediction_india$LDL_Level           1    12227 12235
Step:  AIC=12231.03
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Triglyceride_Level
                                                   Df Deviance   AIC
- heart_attack_prediction_india$Triglyceride_Level  1    12223 12229
- heart_attack_prediction_india$HDL_Level           1    12224 12230
<none>                                                   12223 12231
+ heart_attack_prediction_india$Diastolic_BP        1    12223 12233
+ heart_attack_prediction_india$Cholesterol_Level   1    12223 12233
+ heart_attack_prediction_india$Systolic_BP         1    12223 12233
- heart_attack_prediction_india$LDL_Level           1    12228 12234
Step:  AIC=12229.26
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level
                                                   Df Deviance   AIC
- heart_attack_prediction_india$HDL_Level           1    12225 12229
<none>                                                   12223 12229
+ heart_attack_prediction_india$Triglyceride_Level  1    12223 12231
+ heart_attack_prediction_india$Diastolic_BP        1    12223 12231
+ heart_attack_prediction_india$Cholesterol_Level   1    12223 12231
+ heart_attack_prediction_india$Systolic_BP         1    12223 12231
- heart_attack_prediction_india$LDL_Level           1    12228 12232
Step:  AIC=12228.62
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level
                                                   Df Deviance   AIC
<none>                                                   12225 12229
+ heart_attack_prediction_india$HDL_Level           1    12223 12229
+ heart_attack_prediction_india$Triglyceride_Level  1    12224 12230
+ heart_attack_prediction_india$Diastolic_BP        1    12224 12230
+ heart_attack_prediction_india$Cholesterol_Level   1    12225 12231
+ heart_attack_prediction_india$Systolic_BP         1    12225 12231
- heart_attack_prediction_india$LDL_Level           1    12229 12231
```

Therefore, the lowest AIC value can be attributed to this univariate model:

```text
Heart_Attack_Risk ~ LDL_Level
```

So we return to the model discussed in this post:

[Construction of a univariate model to predict cardiac risk in Indian patients](https://lucacottizelati.blogspot.com/2025/05/costruzione-di-un-modello-univariato.html)

## Automatic selection of mixed variables

We can proceed by creating a macro-model with a large number of variables, both continuous and categorical, and then choose the model with the lowest AIC:

Input code:

```r
>modello_completo <- glm(
  heart_attack_prediction_india$Heart_Attack_Risk ~
    heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level +
    heart_attack_prediction_india$Systolic_BP +
    heart_attack_prediction_india$Diastolic_BP +
    heart_attack_prediction_india$Cholesterol_Level +
    heart_attack_prediction_india$Triglyceride_Level +
    heart_attack_prediction_india$Age +
    heart_attack_prediction_india$Diabetes +
    heart_attack_prediction_india$Hypertension +
   heart_attack_prediction_india$Obesity +
    heart_attack_prediction_india$Smoking +
    heart_attack_prediction_india$Alcohol_Consumption +
    heart_attack_prediction_india$Air_Pollution_Exposure,
  data = heart_attack_prediction_india,
  family = binomial
)
>modello_finale <- step(modello_completo, direction = "both")
```

Output:

```text
Start:  AIC=12242.16
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Systolic_BP +
    heart_attack_prediction_india$Diastolic_BP + heart_attack_prediction_india$Cholesterol_Level +
    heart_attack_prediction_india$Triglyceride_Level + heart_attack_prediction_india$Age +
    heart_attack_prediction_india$Diabetes + heart_attack_prediction_india$Hypertension +
    heart_attack_prediction_india$Obesity + heart_attack_prediction_india$Smoking +
    heart_attack_prediction_india$Alcohol_Consumption + heart_attack_prediction_india$Air_Pollution_Exposure
                                                       Df Deviance   AIC
- heart_attack_prediction_india$Hypertension            1    12214 12240
- heart_attack_prediction_india$Obesity                 1    12214 12240
- heart_attack_prediction_india$Cholesterol_Level       1    12214 12240
- heart_attack_prediction_india$Systolic_BP             1    12214 12240
- heart_attack_prediction_india$Triglyceride_Level      1    12214 12240
- heart_attack_prediction_india$Diastolic_BP            1    12214 12240
- heart_attack_prediction_india$Air_Pollution_Exposure  1    12215 12241
- heart_attack_prediction_india$Diabetes                1    12215 12241
- heart_attack_prediction_india$HDL_Level               1    12216 12242
- heart_attack_prediction_india$Smoking                 1    12216 12242
<none>                                                       12214 12242
- heart_attack_prediction_india$Age                     1    12217 12243
- heart_attack_prediction_india$Alcohol_Consumption     1    12217 12243
- heart_attack_prediction_india$LDL_Level               1    12219 12245
Step:  AIC=12240.18
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Systolic_BP +
    heart_attack_prediction_india$Diastolic_BP + heart_attack_prediction_india$Cholesterol_Level +
    heart_attack_prediction_india$Triglyceride_Level + heart_attack_prediction_india$Age +
    heart_attack_prediction_india$Diabetes + heart_attack_prediction_india$Obesity +
    heart_attack_prediction_india$Smoking + heart_attack_prediction_india$Alcohol_Consumption +
    heart_attack_prediction_india$Air_Pollution_Exposure
                                                       Df Deviance   AIC
- heart_attack_prediction_india$Obesity                 1    12214 12238
- heart_attack_prediction_india$Cholesterol_Level       1    12214 12238
- heart_attack_prediction_india$Systolic_BP             1    12214 12238
- heart_attack_prediction_india$Triglyceride_Level      1    12214 12238
- heart_attack_prediction_india$Diastolic_BP            1    12214 12238
- heart_attack_prediction_india$Air_Pollution_Exposure  1    12215 12239
- heart_attack_prediction_india$Diabetes                1    12215 12239
- heart_attack_prediction_india$HDL_Level               1    12216 12240
- heart_attack_prediction_india$Smoking                 1    12216 12240
<none>                                                       12214 12240
- heart_attack_prediction_india$Age                     1    12217 12241
- heart_attack_prediction_india$Alcohol_Consumption     1    12217 12241
+ heart_attack_prediction_india$Hypertension            1    12214 12242
- heart_attack_prediction_india$LDL_Level               1    12219 12243
Step:  AIC=12238.22
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Systolic_BP +
    heart_attack_prediction_india$Diastolic_BP + heart_attack_prediction_india$Cholesterol_Level +
    heart_attack_prediction_india$Triglyceride_Level + heart_attack_prediction_india$Age +
    heart_attack_prediction_india$Diabetes + heart_attack_prediction_india$Smoking +
    heart_attack_prediction_india$Alcohol_Consumption + heart_attack_prediction_india$Air_Pollution_Exposure
                                                       Df Deviance   AIC
- heart_attack_prediction_india$Cholesterol_Level       1    12214 12236
- heart_attack_prediction_india$Systolic_BP             1    12214 12236
- heart_attack_prediction_india$Triglyceride_Level      1    12214 12236
- heart_attack_prediction_india$Diastolic_BP            1    12214 12236
- heart_attack_prediction_india$Air_Pollution_Exposure  1    12215 12237
- heart_attack_prediction_india$Diabetes                1    12215 12237
- heart_attack_prediction_india$HDL_Level               1    12216 12238
- heart_attack_prediction_india$Smoking                 1    12216 12238
<none>                                                       12214 12238
- heart_attack_prediction_india$Age                     1    12217 12239
- heart_attack_prediction_india$Alcohol_Consumption     1    12217 12239
+ heart_attack_prediction_india$Obesity                 1    12214 12240
+ heart_attack_prediction_india$Hypertension            1    12214 12240
- heart_attack_prediction_india$LDL_Level               1    12219 12241
Step:  AIC=12236.27
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Systolic_BP +
    heart_attack_prediction_india$Diastolic_BP + heart_attack_prediction_india$Triglyceride_Level +
    heart_attack_prediction_india$Age + heart_attack_prediction_india$Diabetes +
    heart_attack_prediction_india$Smoking + heart_attack_prediction_india$Alcohol_Consumption +
    heart_attack_prediction_india$Air_Pollution_Exposure
                                                       Df Deviance   AIC
- heart_attack_prediction_india$Systolic_BP             1    12214 12234
- heart_attack_prediction_india$Triglyceride_Level      1    12214 12234
- heart_attack_prediction_india$Diastolic_BP            1    12214 12234
- heart_attack_prediction_india$Air_Pollution_Exposure  1    12215 12235
- heart_attack_prediction_india$Diabetes                1    12215 12235
- heart_attack_prediction_india$HDL_Level               1    12216 12236
- heart_attack_prediction_india$Smoking                 1    12216 12236
<none>                                                       12214 12236
- heart_attack_prediction_india$Age                     1    12217 12237
- heart_attack_prediction_india$Alcohol_Consumption     1    12217 12237
+ heart_attack_prediction_india$Cholesterol_Level       1    12214 12238
+ heart_attack_prediction_india$Obesity                 1    12214 12238
+ heart_attack_prediction_india$Hypertension            1    12214 12238
- heart_attack_prediction_india$LDL_Level               1    12219 12239
Step:  AIC=12234.33
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Diastolic_BP +
    heart_attack_prediction_india$Triglyceride_Level + heart_attack_prediction_india$Age +
    heart_attack_prediction_india$Diabetes + heart_attack_prediction_india$Smoking +
    heart_attack_prediction_india$Alcohol_Consumption + heart_attack_prediction_india$Air_Pollution_Exposure
                                                       Df Deviance   AIC
- heart_attack_prediction_india$Triglyceride_Level      1    12214 12232
- heart_attack_prediction_india$Diastolic_BP            1    12215 12233
- heart_attack_prediction_india$Air_Pollution_Exposure  1    12215 12233
- heart_attack_prediction_india$Diabetes                1    12215 12233
- heart_attack_prediction_india$HDL_Level               1    12216 12234
- heart_attack_prediction_india$Smoking                 1    12216 12234
<none>                                                       12214 12234
- heart_attack_prediction_india$Age                     1    12217 12235
- heart_attack_prediction_india$Alcohol_Consumption     1    12218 12236
+ heart_attack_prediction_india$Systolic_BP             1    12214 12236
+ heart_attack_prediction_india$Cholesterol_Level       1    12214 12236
+ heart_attack_prediction_india$Obesity                 1    12214 12236
+ heart_attack_prediction_india$Hypertension            1    12214 12236
- heart_attack_prediction_india$LDL_Level               1    12219 12237
Step:  AIC=12232.54
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Diastolic_BP +
    heart_attack_prediction_india$Age + heart_attack_prediction_india$Diabetes +
    heart_attack_prediction_india$Smoking + heart_attack_prediction_india$Alcohol_Consumption +
    heart_attack_prediction_india$Air_Pollution_Exposure
                                                       Df Deviance   AIC
- heart_attack_prediction_india$Diastolic_BP            1    12215 12231
- heart_attack_prediction_india$Air_Pollution_Exposure  1    12215 12231
- heart_attack_prediction_india$Diabetes                1    12216 12232
- heart_attack_prediction_india$HDL_Level               1    12216 12232
- heart_attack_prediction_india$Smoking                 1    12216 12232
<none>                                                       12214 12232
- heart_attack_prediction_india$Age                     1    12217 12233
- heart_attack_prediction_india$Alcohol_Consumption     1    12218 12234
+ heart_attack_prediction_india$Triglyceride_Level      1    12214 12234
+ heart_attack_prediction_india$Systolic_BP             1    12214 12234
+ heart_attack_prediction_india$Cholesterol_Level       1    12214 12234
+ heart_attack_prediction_india$Obesity                 1    12214 12234
+ heart_attack_prediction_india$Hypertension            1    12214 12234
- heart_attack_prediction_india$LDL_Level               1    12219 12235
Step:  AIC=12230.77
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Age +
    heart_attack_prediction_india$Diabetes + heart_attack_prediction_india$Smoking +
    heart_attack_prediction_india$Alcohol_Consumption + heart_attack_prediction_india$Air_Pollution_Exposure
                                                       Df Deviance   AIC
- heart_attack_prediction_india$Air_Pollution_Exposure  1    12215 12229
- heart_attack_prediction_india$Diabetes                1    12216 12230
- heart_attack_prediction_india$HDL_Level               1    12216 12230
- heart_attack_prediction_india$Smoking                 1    12216 12230
<none>                                                       12215 12231
- heart_attack_prediction_india$Age                     1    12217 12231
- heart_attack_prediction_india$Alcohol_Consumption     1    12218 12232
+ heart_attack_prediction_india$Diastolic_BP            1    12214 12232
+ heart_attack_prediction_india$Triglyceride_Level      1    12215 12233
+ heart_attack_prediction_india$Systolic_BP             1    12215 12233
+ heart_attack_prediction_india$Cholesterol_Level       1    12215 12233
+ heart_attack_prediction_india$Obesity                 1    12215 12233
+ heart_attack_prediction_india$Hypertension            1    12215 12233
- heart_attack_prediction_india$LDL_Level               1    12219 12233
Step:  AIC=12229.25
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Age +
    heart_attack_prediction_india$Diabetes + heart_attack_prediction_india$Smoking +
    heart_attack_prediction_india$Alcohol_Consumption
                                                       Df Deviance   AIC
- heart_attack_prediction_india$Diabetes                1    12216 12228
- heart_attack_prediction_india$HDL_Level               1    12217 12229
- heart_attack_prediction_india$Smoking                 1    12217 12229
<none>                                                       12215 12229
- heart_attack_prediction_india$Age                     1    12218 12230
- heart_attack_prediction_india$Alcohol_Consumption     1    12218 12230
+ heart_attack_prediction_india$Air_Pollution_Exposure  1    12215 12231
+ heart_attack_prediction_india$Diastolic_BP            1    12215 12231
+ heart_attack_prediction_india$Triglyceride_Level      1    12215 12231
+ heart_attack_prediction_india$Systolic_BP             1    12215 12231
+ heart_attack_prediction_india$Cholesterol_Level       1    12215 12231
+ heart_attack_prediction_india$Obesity                 1    12215 12231
+ heart_attack_prediction_india$Hypertension            1    12215 12231
- heart_attack_prediction_india$LDL_Level               1    12220 12232
Step:  AIC=12228.18
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$HDL_Level + heart_attack_prediction_india$Age +
    heart_attack_prediction_india$Smoking + heart_attack_prediction_india$Alcohol_Consumption
                                                       Df Deviance   AIC
- heart_attack_prediction_india$HDL_Level               1    12218 12228
- heart_attack_prediction_india$Smoking                 1    12218 12228
<none>                                                       12216 12228
- heart_attack_prediction_india$Age                     1    12219 12229
+ heart_attack_prediction_india$Diabetes                1    12215 12229
- heart_attack_prediction_india$Alcohol_Consumption     1    12219 12229
+ heart_attack_prediction_india$Air_Pollution_Exposure  1    12216 12230
+ heart_attack_prediction_india$Diastolic_BP            1    12216 12230
+ heart_attack_prediction_india$Triglyceride_Level      1    12216 12230
+ heart_attack_prediction_india$Systolic_BP             1    12216 12230
+ heart_attack_prediction_india$Cholesterol_Level       1    12216 12230
+ heart_attack_prediction_india$Obesity                 1    12216 12230
+ heart_attack_prediction_india$Hypertension            1    12216 12230
- heart_attack_prediction_india$LDL_Level               1    12221 12231
Step:  AIC=12227.59
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$Age + heart_attack_prediction_india$Smoking +
    heart_attack_prediction_india$Alcohol_Consumption
                                                       Df Deviance   AIC
- heart_attack_prediction_india$Smoking                 1    12219 12227
<none>                                                       12218 12228
- heart_attack_prediction_india$Age                     1    12220 12228
+ heart_attack_prediction_india$HDL_Level               1    12216 12228
+ heart_attack_prediction_india$Diabetes                1    12217 12229
- heart_attack_prediction_india$Alcohol_Consumption     1    12221 12229
+ heart_attack_prediction_india$Air_Pollution_Exposure  1    12217 12229
+ heart_attack_prediction_india$Diastolic_BP            1    12217 12229
+ heart_attack_prediction_india$Triglyceride_Level      1    12217 12229
+ heart_attack_prediction_india$Systolic_BP             1    12218 12230
+ heart_attack_prediction_india$Cholesterol_Level       1    12218 12230
+ heart_attack_prediction_india$Obesity                 1    12218 12230
+ heart_attack_prediction_india$Hypertension            1    12218 12230
- heart_attack_prediction_india$LDL_Level               1    12222 12230
Step:  AIC=12227.21
heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level +
    heart_attack_prediction_india$Age + heart_attack_prediction_india$Alcohol_Consumption
                                                       Df Deviance   AIC
<none>                                                       12219 12227
+ heart_attack_prediction_india$Smoking                 1    12218 12228
- heart_attack_prediction_india$Age                     1    12222 12228
+ heart_attack_prediction_india$HDL_Level               1    12218 12228
- heart_attack_prediction_india$Alcohol_Consumption     1    12222 12228
+ heart_attack_prediction_india$Diabetes                1    12218 12228
+ heart_attack_prediction_india$Air_Pollution_Exposure  1    12219 12229
+ heart_attack_prediction_india$Diastolic_BP            1    12219 12229
+ heart_attack_prediction_india$Triglyceride_Level      1    12219 12229
+ heart_attack_prediction_india$Cholesterol_Level       1    12219 12229
+ heart_attack_prediction_india$Systolic_BP             1    12219 12229
+ heart_attack_prediction_india$Obesity                 1    12219 12229
+ heart_attack_prediction_india$Hypertension            1    12219 12229
- heart_attack_prediction_india$LDL_Level               1    12224 12230
```

So the best model seems to be the one that correlates risk with:

- age
- alcohol
- LDL

with the R summary we can determine the various coefficients

```text
Coefficients:
                                                       Estimate Std. Error z value Pr(>|z|)
(Intercept)                                          -0.9450585  0.2344420  -4.031 5.55e-05 ***
heart_attack_prediction_india$LDL_Level               0.0010545  0.0005033   2.095   0.0361 *
heart_attack_prediction_india$HDL_Level               0.0015107  0.0012549   1.204   0.2287
heart_attack_prediction_india$Systolic_BP            -0.0002175  0.0008446  -0.257   0.7968
heart_attack_prediction_india$Diastolic_BP           -0.0006006  0.0012553  -0.478   0.6324
heart_attack_prediction_india$Cholesterol_Level      -0.0001126  0.0005037  -0.224   0.8231
heart_attack_prediction_india$Triglyceride_Level     -0.0001412  0.0003069  -0.460   0.6453
heart_attack_prediction_india$Age                     0.0019954  0.0012642   1.578   0.1145
heart_attack_prediction_india$Diabetes               -0.0721312  0.0761090  -0.948   0.3433
heart_attack_prediction_india$Hypertension           -0.0069743  0.0506858  -0.138   0.8906
heart_attack_prediction_india$Obesity                -0.0092853  0.0475258  -0.195   0.8451
heart_attack_prediction_india$Smoking                -0.0614164  0.0478272  -1.284   0.1991
heart_attack_prediction_india$Alcohol_Consumption    -0.0816325  0.0459177  -1.778   0.0754 .
heart_attack_prediction_india$Air_Pollution_Exposure -0.0311534  0.0445562  -0.699   0.4844
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
(Dispersion parameter for binomial family taken to be 1)
```

Now we have a multivariate model available.

We can proceed to evaluate how performant the model is.

First of all, now that we have completed the logistic function, we can proceed to calculate the probability value of risk for each patient as follows:

```r
> heart_attack_prediction_india$prob_rischio <- predict(modello_completo, type = "response")
```

In this way we have added `prob_rischio` to the dataset, calculated for each patient.

Now we need to set a threshold that will allow us to say whether the patient falls into cardiac risk or not.

We could set the threshold to 0.5 so that:

- if a patient has `prob_rischio > 0.5` then we consider them at risk, therefore `1`
- if a patient has `prob_rischio <= 0.5` then we consider them not at risk, therefore `0`

The applicable code is the following:

```r
heart_attack_prediction_india$predicted_class <- ifelse(heart_attack_prediction_india$prob_rischio > 0.5, 1, 0)
```

However, before setting a threshold we should study the probabilities (that is, what the values of the `prob_rischio` field are):

<img width="864" height="550" alt="Rplot02" src="https://github.com/user-attachments/assets/9cdfe3d1-67f2-46c6-98b9-c74ced2b1ab9" />

Given the distribution, we can observe that:

- 0.5 is out of reach, so it is an inapplicable threshold (they would all be zeros in the predicted class)
- we could set it to 0.3, but that is not very significant

Therefore I would not proceed with the calculation of the model performance because we are unable to assign high probabilities of risk to patients.

A new multivariate model is needed.
