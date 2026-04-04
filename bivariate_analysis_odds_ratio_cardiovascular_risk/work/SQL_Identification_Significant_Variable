## Identification of the Most Significant Variable

We divide the cases into two parts:

- analysis of the **Over_50** population
- analysis of the **Under_50** population

### 2.1 Analysis of the Over_50 Population

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
