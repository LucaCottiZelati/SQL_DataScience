# Building a Univariate Model to Predict Cardiac Risk

Let us begin by listing the variables that contain the measurements:

- cholesterol level

- triglyceride level

- LDL level

- HDL level

- systolic blood pressure level

- diastolic blood pressure level

The categorical variable, instead, is cardiac risk (0 or 1).

Important premise: in the two-sample t-test we have a series of important outputs, namely:

- the t value

- degrees of freedom

- p-value

- confidence interval

- means of the two groups

What is interesting for our analysis is the p-value: the lower it is, the greater the probability that the association between the observed variable and cardiac risk is not due to chance.

#### RUNNING THE T-TEST

From an "algorithmic" point of view, the action of the t-test is focused on a few stages:

- STAGE 1: Splitting the dataset

Let us start, for example, from cholesterol levels, which are associated with 0/1 values of the cardiac risk variable. The first activity of the t-test is to split people into:

    - people with cardiac risk

    - people without cardiac risk

- STAGE 2: Calculation of means and standard deviations for the groups

- STAGE 3: Application of the statistical formula (Student's test, Welch's test)

In our case we apply Welch's test because Student's test, to be applied without problems, requires the variances of the two groups to be similar. To avoid additional effort we use Welch's test (therefore we will have `var.equal = FALSE`, so we assume it is false that the variances of the two groups are equal).

#### APPLICATION

```text
> t.test(heart_attack_prediction_india$Cholesterol_Level ~ heart_attack_prediction_india$Heart_Attack_Risk, data = heart_attack_prediction_india, var.equal = FALSE)

    Welch Two Sample t-test
data:  heart_attack_prediction_india$Cholesterol_Level by heart_attack_prediction_india$Heart_Attack_Risk
t = 0.30477, df = 5667, p-value = 0.7606
alternative hypothesis: true difference in means between group 0 and group 1 is not equal to 0
95 percent confidence interval:
 -1.568841  2.146426
sample estimates:
mean in group 0 mean in group 1
       224.8398        224.5510

> t.test(heart_attack_prediction_india$Triglyceride_Level ~ heart_attack_prediction_india$Heart_Attack_Risk, data = heart_attack_prediction_india, var.equal = FALSE)

    Welch Two Sample t-test
data:  heart_attack_prediction_india$Triglyceride_Level by heart_attack_prediction_india$Heart_Attack_Risk
t = 0.4813, df = 5691.5, p-value = 0.6303
alternative hypothesis: true difference in means between group 0 and group 1 is not equal to 0
95 percent confidence interval:
 -2.295938  3.790171
sample estimates:
mean in group 0 mean in group 1
       174.9580        174.2108

> t.test(heart_attack_prediction_india$LDL_Level ~ heart_attack_prediction_india$Heart_Attack_Risk, data = heart_attack_prediction_india, var.equal = FALSE)

    Welch Two Sample t-test
data:  heart_attack_prediction_india$LDL_Level by heart_attack_prediction_india$Heart_Attack_Risk
t = -2.1085, df = 5606.1, p-value = 0.03503
alternative hypothesis: true difference in means between group 0 and group 1 is not equal to 0
95 percent confidence interval:
 -3.8778640 -0.1411631
sample estimates:
mean in group 0 mean in group 1
       123.2678        125.2774

> t.test(heart_attack_prediction_india$HDL_Level ~ heart_attack_prediction_india$Heart_Attack_Risk, data = heart_attack_prediction_india, var.equal = FALSE)

    Welch Two Sample t-test
data:  heart_attack_prediction_india$HDL_Level by heart_attack_prediction_india$Heart_Attack_Risk
t = -1.1798, df = 5601.8, p-value = 0.2381
alternative hypothesis: true difference in means between group 0 and group 1 is not equal to 0
95 percent confidence interval:
 -1.2001445  0.2983434
sample estimates:
mean in group 0 mean in group 1
       49.19991        49.65081

> t.test(heart_attack_prediction_india$Systolic_BP ~ heart_attack_prediction_india$Heart_Attack_Risk, data = heart_attack_prediction_india, var.equal = FALSE)

    Welch Two Sample t-test
data:  heart_attack_prediction_india$Systolic_BP by heart_attack_prediction_india$Heart_Attack_Risk
t = 0.23004, df = 5738.1, p-value = 0.8181
alternative hypothesis: true difference in means between group 0 and group 1 is not equal to 0
95 percent confidence interval:
 -0.9721677  1.2306629
sample estimates:
mean in group 0 mean in group 1
       134.7648        134.6355

> t.test(heart_attack_prediction_india$Diastolic_BP ~ heart_attack_prediction_india$Heart_Attack_Risk, data = heart_attack_prediction_india, var.equal = FALSE)

    Welch Two Sample t-test
data:  heart_attack_prediction_india$Diastolic_BP by heart_attack_prediction_india$Heart_Attack_Risk
t = 0.39704, df = 5759.1, p-value = 0.6914
alternative hypothesis: true difference in means between group 0 and group 1 is not equal to 0
95 percent confidence interval:
 -0.5901805  0.8899568
sample estimates:
mean in group 0 mean in group 1
       89.35707        89.20718
```

#### T-TEST RESULT

The significant value is certainly the LDL value.

Therefore, we have now found the most statistically significant variable. The next step is to associate a probability of cardiac risk when the LDL value increases or decreases.

#### BUILDING THE MODEL WITH LOGISTIC REGRESSION

To obtain the logistic regression model (which fits well in this case because we have one continuous variable and one binary variable) we can carry out these stages:

- STAGE 1: Take the data and build a table with: LDL level and cardiac risk 0 or 1

- STAGE 2: application of the logistic function:

p=1/(1+e−(β 0+β 1∗L D L))

p: is the probability of cardiac risk

beta0 and beta1: are the parameters to be estimated in order to obtain maximum likelihood with respect to the dataset values.

#### APPLICATION

```text
> modello <- glm(heart_attack_prediction_india$Heart_Attack_Risk ~ heart_attack_prediction_india$LDL_Level, data = heart_attack_prediction_india, family = binomial)

> summary(modello)

Call:
glm(formula = heart_attack_prediction_india$Heart_Attack_Risk ~
    heart_attack_prediction_india$LDL_Level, family = binomial,
    data = heart_attack_prediction_india)

Coefficients:
                                          Estimate Std. Error z value Pr(>|z|)
(Intercept)                             -0.9765241  0.0663438 -14.719   <2e-16 ***
heart_attack_prediction_india$LDL_Level  0.0010667  0.0005026   2.122   0.0338 *
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 12229  on 9999  degrees of freedom
Residual deviance: 12225  on 9998  degrees of freedom
AIC: 12229

Number of Fisher Scoring iterations: 4
```

#### MODEL RESULT

The results can be interpreted as follows:

- estimate (that is beta1): 0.0010667

- intercept estimate (that is beta zero): -0.9765241

This means that for an increase of one unit of LDL, cardiac risk increases by 0.0010667 units (therefore very little).

So now the model formula becomes:

p=1/(1+e−(-0.9765241+0.0010667​∗L D L))

Therefore, for example, for an LDL level equal to 250:

p=1/(1+e−(-0.9765241+0.0010667∗250))= 33% (approximately)

By mapping the function over a range that lets us see the classic sigmoid, let us use a very wide range of values:

```text
> # Grafico
> plot(ldl_values, prob_values, type = "l", lwd = 2, col = "blue",
+      main = "Probabilità di rischio cardiaco in base a LDL",
+      xlab = "LDL (mg/dL)", ylab = "Probabilità di rischio")
> abline(h = 0.5, col = "red", lty = 2)  # Soglia del 50%

> # Funzione logistica per il rischio cardiaco
> prob_rischio <- function(ldl) {
+   1 / (1 + exp(-(-0.9765241 + 0.0010667 * ldl)))
+ }
>
>
> # Vettore di valori LDL
> ldl_values <- seq(0, 9000, by = 1)
>
> # Calcolo delle probabilità
> prob_values <- prob_rischio(ldl_values)
>
> # Grafico
> plot(ldl_values, prob_values, type = "l", lwd = 2, col = "blue",
+      main = "Probabilità di rischio cardiaco in base a LDL",
+      xlab = "LDL (mg/dL)", ylab = "Probabilità di rischio")
> abline(h = 0.5, col = "red", lty = 2)  # Soglia del 50%
```


(click on the image to expand it in high definition)

Therefore:

- the LDL value is statistically significant in predicting cardiac risk.

- to obtain a considerable increase in cardiac risk, LDL must be really very high.

(it should be considered that usually LDL values in humans are between 250 and 500)

Therefore, LDL value alone is certainly not a strong predictor of cardiac risk.

