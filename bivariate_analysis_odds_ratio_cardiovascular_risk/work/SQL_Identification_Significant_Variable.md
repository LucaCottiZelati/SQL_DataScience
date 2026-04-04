## Identification of the Most Significant Variable

We divide the cases into two parts:

- analysis of the **Over_50** population
- analysis of the **Under_50** population

### 1. Analysis of the Over_50 Population

```sql
SELECT 'Diabetes' as 'Variabile_considerata', 
Gender,  
COUNT(*) AS totale,
SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS numero_soggetti,
ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 
ELSE 0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio 
FROM database_1.heart_attack_prediction_india_ag 
where age_group='Over_50' and Diabetes=1 
GROUP BY Diabetes,Gender 
UNION
SELECT 'Hypertension' as 'Variabile_considerata',
Gender,  
COUNT(*) AS totale,
SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS numero_soggetti,
ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 
ELSE 0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio 
FROM database_1.heart_attack_prediction_india_ag 
where age_group='Over_50' and Hypertension=1 
GROUP BY Hypertension,Gender 
UNION 
SELECT 'Obesity' as 'Variabile_considerata',
Gender, COUNT(*) AS totale,
SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS numero_soggetti, 
ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
 0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio 
FROM database_1.heart_attack_prediction_india_ag 
where age_group='Over_50' and Obesity=1 
GROUP BY obesity,Gender 
UNION 
SELECT 'Smoking' as 'Variabile_considerata',  
Gender, COUNT(*) AS totale,
SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
END) AS numero_soggetti,
ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio
FROM database_1.heart_attack_prediction_india_ag 
where age_group='Over_50' and Smoking=1
GROUP BY  Smoking,Gender  
UNION 
SELECT 'Alcohol_Consumption' as
'Variabile_considerata',  
Gender,  COUNT(*) AS totale,  
SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
END) AS numero_soggetti,   
ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio
FROM  database_1.heart_attack_prediction_india_ag 
where age_group='Over_50' and Alcohol_Consumption=1 
GROUP BY  Alcohol_Consumption ,Gender 
UNION
SELECT  'Physical_Activity' as 'Variabile_considerata',  
Gender,  
COUNT(*) AS totale, 
SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
END) AS numero_soggetti,  
ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio 
FROM database_1.heart_attack_prediction_india_ag 
where age_group='Over_50' and Physical_Activity=0 
GROUP BY  Physical_Activity, Gender ; 
UNION
SELECT 'Air_Pollution_Exposure' as
'Variabile_considerata',  
Gender,  COUNT(*) AS totale, 
SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
END) AS numero_soggetti,  
ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio 
FROM database_1.heart_attack_prediction_india_ag 
where age_group='Over_50' and Air_Pollution_Exposure=1 
GROUP BY Air_Pollution_Exposure, Gender 
UNION
SELECT 'Family_History' as 'Variabile_considerata',
Gender,  COUNT(*) AS totale,
SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
END) AS numero_soggetti, 
ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio 
FROM database_1.heart_attack_prediction_india_ag 
where age_group='Over_50' and Family_History=1 
GROUP BY Family_History ,Gender 
UNION
SELECT  'Healthcare_Access' as 'Variabile_considerata',
Gender, 
COUNT(*) AS totale,  
SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
END) AS numero_soggetti, 
ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio
FROM database_1.heart_attack_prediction_india_ag 
where age_group='Over_50' and Healthcare_Access=1 
GROUP BY Healthcare_Access, Gender 
```

| Variable Considered        | Gender | Number of Subjects | Total at Risk | Risk Percentage |
|---------------------------|--------|-------------------:|--------------:|----------------:|
| Diabetes                  | Male   | 277                | 88            | 31.77           |
| Obesity                   | Male   | 867                | 270           | 31.14           |
| Hypertension              | Male   | 681                | 209           | 30.69           |
| Family_History            | Female | 686                | 208           | 30.32           |
| Healthcare_Access         | Female | 686                | 203           | 29.59           |
| Physical_Activity         | Male   | 1098               | 321           | 29.23           |
| Family_History            | Male   | 883                | 257           | 29.11           |
| Air_Pollution_Exposure    | Male   | 1149               | 334           | 29.07           |
| Air_Pollution_Exposure    | Female | 868                | 252           | 29.03           |
| Obesity                   | Female | 717                | 208           | 29.01           |
| Alcohol_Consumption       | Male   | 988                | 285           | 28.85           |
| Smoking                   | Male   | 862                | 245           | 28.42           |
| Smoking                   | Female | 688                | 194           | 28.20           |
| Diabetes                  | Female | 178                | 50            | 28.09           |
| Healthcare_Access         | Male   | 897                | 252           | 28.09           |
| Alcohol_Consumption       | Female | 807                | 226           | 28.00           |
| Hypertension              | Female | 573                | 160           | 27.92           |
| Physical_Activity         | Female | 946                | 255           | 26.96           |

This table highlights the variables most strongly associated with heart attack risk in the **Over_50** population.  
The highest observed risk percentage is associated with **Diabetes in males (31.77%)**, followed by **Obesity in males (31.14%)** and **Hypertension in males (30.69%)**.

### 2 Analysis of the Under_50 Population
```sql
SELECT 'Diabetes' as 'Variabile_considerata',  Gender,  COUNT(*) AS totale, SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS numero_soggetti,  ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
            0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio FROM database_1.heart_attack_prediction_india_ag where age_group='Under_50' and Diabetes=1 GROUP BY Diabetes,Gender union SELECT 'Hypertension' as 'Variabile_considerata',  Gender, COUNT(*) AS totale,  SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS numero_soggetti,  ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
            0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio FROM database_1.heart_attack_prediction_india_ag where age_group='Under_50' and Hypertension=1 GROUP BY Hypertension,Gender UNION SELECT 'Obesity' as 'Variabile_considerata',  Gender, COUNT(*) AS totale,  SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS numero_soggetti, ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
            0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio FROM database_1.heart_attack_prediction_india_ag where age_group='Under_50' and Obesity=1 GROUP BY  obesity,Gender  UNION SELECT  'Smoking' as 'Variabile_considerata',  Gender, 
         SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS numero_soggetti,  ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
            0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio FROM database_1.heart_attack_prediction_india_ag where age_group='Under_50' and Smoking=1 GROUP BY Smoking,Gender union SELECT 'Alcohol_Consumption' as
            'Variabile_considerata',  Gender,  COUNT(*) AS totale, SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS numero_soggetti,  ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
            0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio FROM database_1.heart_attack_prediction_india_ag where age_group='Under_50' and Alcohol_Consumption=1 GROUP BY  Alcohol_Consumption ,Gender  union  SELECT 'Physical_Activity' as 'Variabile_considerata',  Gender,  COUNT(*) AS totale,  SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS numero_soggetti,  ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
            0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio FROM database_1.heart_attack_prediction_india_ag where age_group='Under_50' and Physical_Activity=0 GROUP BY  Physical_Activity, Gender union SELECT 'Air_Pollution_Exposure' as
            'Variabile_considerata',  Gender,  COUNT(*) AS totale,  SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS numero_soggetti,  ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
            0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio FROM database_1.heart_attack_prediction_india_ag where age_group='Under_50' and Air_Pollution_Exposure=1 GROUP BY  Air_Pollution_Exposure, Gender union SELECT 'Family_History' as 'Variabile_considerata',  Gender,  COUNT(*) AS totale, SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS numero_soggetti,  ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
            0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio FROM database_1.heart_attack_prediction_india_ag where age_group='Under_50' and Family_History=1 GROUP BY  Family_History ,Gender union SELECT 'Healthcare_Access' as 'Variabile_considerata', Gender, COUNT(*) AS totale, SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS numero_soggetti,  ROUND(SUM(CASE WHEN Heart_Attack_Risk = 1 THEN 1 ELSE
            0 END) * 100.0 / COUNT(*), 2) AS percentuale_rischio FROM database_1.heart_attack_prediction_india_ag where age_group='Under_50' and Healthcare_Access=1 GROUP BY  Healthcare_Access, Gender
```          
 
### Most Significant Variables in the Under_50 Population

The following table reports the variables with the highest percentage of heart attack risk in the **Under_50** population, broken down by gender.

| Variable Considered     | Gender | Total | Number of Subjects at Risk | Risk Percentage |
|-------------------------|--------|------:|---------------------------:|----------------:|
| Hypertension            | Male   | 1362  | 429                        | 31.50           |
| Physical_Activity       | Male   | 2211  | 687                        | 31.07           |
| Family_History          | Female | 1378  | 422                        | 30.62           |
| Obesity                 | Male   | 1679  | 511                        | 30.43           |
| Alcohol_Consumption     | Male   | 1926  | 577                        | 29.96           |
| Air_Pollution_Exposure  | Male   | 2258  | 675                        | 29.89           |
| Diabetes                | Male   | 539   | 161                        | 29.87           |
| Family_History          | Male   | 1735  | 517                        | 29.80           |
| Smoking                 | Male   | 1678  | 498                        | 29.68           |
| Air_Pollution_Exposure  | Female | 1778  | 524                        | 29.47           |
| Healthcare_Access       | Female | 1382  | 406                        | 29.38           |
| Obesity                 | Female | 1358  | 398                        | 29.31           |
| Healthcare_Access       | Male   | 1728  | 504                        | 29.17           |
| Smoking                 | Female | 1336  | 382                        | 28.59           |
| Physical_Activity       | Female | 1831  | 515                        | 28.13           |
| Hypertension            | Female | 1107  | 311                        | 28.09           |
| Alcohol_Consumption     | Female | 1602  | 446                        | 27.84           |
| Diabetes                | Female | 390   | 106                        | 27.18           |

Based on the provided dataset, the highest heart attack risk appears to be associated with the following factors:

- for the **Over_50** population: **diabetes, obesity, and hypertension** are the main risk factors, and they seem to have a stronger negative impact on men than on women;

- for the **Under_50** population: **hypertension, lack of physical activity, and family history** represent the top three risk factors, and once again men appear to be more at risk than women.

However, the analysis of the ratios does not show one association that is clearly much stronger than the others.

A possible next step would be to use the **odds ratio (OR)** in order to determine whether one of these variables is more strongly associated with heart attack risk. View SQL_Odds_Ratio file.
