# Feature Engineering and Correlogram to Predict Cardiac Risk

One of the issues with this dataset is the inability to capture relationships between the independent variables and the target variable. In these cases, there are several possibilities; one of them is to try to build a new variable derived from the combination of multiple variables currently present in the dataset.

## FEATURE ENGINEERING AND CORRELOGRAMS

Let us try to construct 2 new clinical indicators:

- **Blood Pressure Index**: the ratio between systolic and diastolic blood pressure
- **Lipid Ratio**: the ratio between bad cholesterol and good cholesterol

Once the two new variables have been created and added to the dataset, we will calculate a correlogram with the **Bravais-Pearson coefficient** to see whether there are linear correlations.

```r
# Load required libraries
library(dplyr)
library(ggplot2)
library(readr)

# Load the dataset
df <- heart_attack_prediction_india

# Feature Engineering
df <- df %>%
  mutate(
    Blood_Pressure_Index = Systolic_BP / Diastolic_BP,
    Lipid_Ratio = ifelse(HDL_Level == 0, NA, LDL_Level / HDL_Level),
    Gender = ifelse(Gender == "Male", 1, 0)
  )

# Select only numeric variables
numeric_vars <- df %>% select(where(is.numeric))
# Calculate the correlation with the target variable
correlations <- cor(numeric_vars, use = "complete.obs")

# Extract and sort correlations with Heart_Attack_Risk
cor_target <- sort(abs(correlations[, "Heart_Attack_Risk"]), decreasing = TRUE)
cor_target <- data.frame(Variable = names(cor_target), Correlation = cor_target)

# Print the most correlated variables
print(cor_target)
```

**Output:**

```text
                                       Variable  Correlation
Heart_Attack_Risk             Heart_Attack_Risk 1.0000000000
LDL_Level                             LDL_Level 0.0212282254
Patient_ID                           Patient_ID 0.0204554733
Alcohol_Consumption         Alcohol_Consumption 0.0172825848
Age                                         Age 0.0155171897
Emergency_Response_Time Emergency_Response_Time 0.0151488477
Smoking                                 Smoking 0.0125035448
Stress_Level                       Stress_Level 0.0121762296
HDL_Level                             HDL_Level 0.0118837716
Healthcare_Access             Healthcare_Access 0.0118608468
Diet_Score                           Diet_Score 0.0106228668
Diabetes                               Diabetes 0.0092777697
Gender                                   Gender 0.0080413230
Lipid_Ratio                         Lipid_Ratio 0.0077196047
Annual_Income                     Annual_Income 0.0073779947
Air_Pollution_Exposure   Air_Pollution_Exposure 0.0065006823
Physical_Activity             Physical_Activity 0.0059677377
Triglyceride_Level           Triglyceride_Level 0.0048145045
Diastolic_BP                       Diastolic_BP 0.0039511714
Blood_Pressure_Index       Blood_Pressure_Index 0.0032447533
Cholesterol_Level             Cholesterol_Level 0.0030543922
Systolic_BP                         Systolic_BP 0.0022929668
Obesity                                 Obesity 0.0020040112
Family_History                   Family_History 0.0013756679
Hypertension                       Hypertension 0.0012280527
Heart_Attack_History       Heart_Attack_History 0.0003442410
Health_Insurance               Health_Insurance 0.0002353388
```

All relationships are near zero, therefore there are no linear relationships.

It could also be useful to calculate additional correlograms and determine whether there are other types of relationships:

- **Distance Correlation**: captures any relationship (linear and non-linear). The algorithm creates a matrix of Euclidean distances for all X and Y pairs and then calculates the distance covariance and variance. This algorithm is computationally expensive, especially for a dataset with many variables.
- **MIC**: this algorithm searches for the best grid that maximizes mutual information between two variables (to better understand the concept: [Mutual information - Wikipedia](https://it.wikipedia.org/wiki/Informazione_mutua)). The advantage of MIC is that the type of relationship between the two variables does not matter (linear, logarithmic, ...).
- **Spearman**: the algorithm tries to capture the monotonic correlation between two variables (not necessarily linear).
- **Kendall**: computes the number of concordant and discordant pairs between X and Y.

However, considering the previous applications of the predictive models, processing all variables is most likely computationally expensive and also useless. If we are unable to determine a pool of variables more strongly linked to the target variable, we will not reach a more concrete result than the one obtained so far.

The problem is that this dataset does not seem to have a clear affinity between:

- the provided variables
- the calculated variables (e.g. **Lipid Ratio**)

There do not appear to be linearly correlated variables.

We could use PCA, but there are no linear relationships.

We can investigate which variables show a non-linear type of correlation according to the **Spearman** indicator:

```r
# Load packages
library(readr)
library(dplyr)
library(ggplot2)

# Read the data
df <-  heart_attack_prediction_india
# Select only valid numeric columns
df_numeric <- df %>%
  select_if(is.numeric) %>%
  select(where(~ length(unique(na.omit(.))) > 1))

# Remove rows with NA
df_complete <- na.omit(df_numeric)

# Compute Spearman correlation
cor_matrix <- cor(df_complete, method = "spearman")
# Extract all correlations with Heart_Attack_Risk, excluding itself
cor_target <- cor_matrix[, "Heart_Attack_Risk"]
cor_target <- cor_target[names(cor_target) != "Heart_Attack_Risk"]
# Create data frame with asterisks
cor_df <- data.frame(
  Variable = names(cor_target),
  Correlation = cor_target
) %>%
  mutate(Significance = case_when(
    abs(Correlation) >= 0.8 ~ "***",
    abs(Correlation) >= 0.5 ~ "**",
    abs(Correlation) >= 0.3 ~ "*",
    TRUE ~ ""
  )) %>%
  arrange(desc(abs(Correlation)))
# Create the plot
ggplot(cor_df, aes(x = reorder(Variable, Correlation), y = Correlation, fill = Correlation)) +
  geom_bar(stat = "identity") +
  geom_text(aes(label = Significance), hjust = -0.2, size = 6) +
  coord_flip() +
  scale_fill_gradient2(low = "blue", mid = "white", high = "red", midpoint = 0) +
  labs(title = "Correlations with Heart_Attack_Risk (with asterisks)",
       y = "Spearman Correlation", x = "Variable") +
  theme_minimal(base_size = 14)
```
<img width="998" height="401" alt="2594b05f-0582-4e6e-a742-9f3bac7c17ff" src="https://github.com/user-attachments/assets/6f16aaed-678b-49e1-aaba-803ce91b5900" />

*(click on the image)*

## ANALYSIS CONCLUSION

The dataset does not contain variables capable of predicting the target variable.
