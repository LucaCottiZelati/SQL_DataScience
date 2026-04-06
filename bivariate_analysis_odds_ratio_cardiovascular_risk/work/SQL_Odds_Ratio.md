### Determining the Strength of the Association Between Categorical Variables and Heart Disease Risk

To calculate the **odds ratio (OR)** more easily for each categorical variable, we use the following definitions:

- **a** = number of subjects with the condition and with heart attack risk  
- **b** = number of subjects with the condition and without risk  
- **c** = number of subjects without the condition and with risk  
- **d** = number of subjects without the condition and without risk  

The formula is:(a × d)/(b × c)
 ```sql
SELECT 'Diabetes' as 'variabile', a, b, c, d, ROUND((a * d) / (b *
            c), 3) AS odds_ratio FROM ( SELECT SUM(CASE WHEN Diabetes = 1 AND
            Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS a, SUM(CASE WHEN
            Diabetes = 1 AND Heart_Attack_Risk = 0 THEN 1 ELSE 0 END) AS b,
            SUM(CASE WHEN Diabetes = 0 AND Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS c, SUM(CASE WHEN Diabetes = 0 AND Heart_Attack_Risk = 0 THEN
            1 ELSE 0 END) AS d FROM database_1.heart_attack_prediction_india_ag
            ) AS sub union SELECT 'Hypertension' as 'variabile', a, b, c, d,
            ROUND((a * d) / (b * c), 3) AS odds_ratio FROM ( SELECT SUM(CASE
            WHEN Hypertension = 1 AND Heart_Attack_Risk = 1 THEN 1 ELSE 0 END)
            AS a, SUM(CASE WHEN Hypertension = 1 AND Heart_Attack_Risk = 0 THEN
            1 ELSE 0 END) AS b, SUM(CASE WHEN Hypertension = 0 AND
            Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS c, SUM(CASE WHEN
            Hypertension = 0 AND Heart_Attack_Risk = 0 THEN 1 ELSE 0 END) AS d
            FROM database_1.heart_attack_prediction_india_ag ) AS sub union
            SELECT 'Physical_Activity' as 'variabile', a, b, c, d, ROUND((a * d)
            / (b * c), 3) AS odds_ratio FROM ( SELECT SUM(CASE WHEN
            Physical_Activity = 1 AND Heart_Attack_Risk = 1 THEN 1 ELSE 0 END)
            AS a, SUM(CASE WHEN Physical_Activity = 1 AND Heart_Attack_Risk = 0
            THEN 1 ELSE 0 END) AS b, SUM(CASE WHEN Physical_Activity = 0 AND
            Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS c, SUM(CASE WHEN
            Physical_Activity = 0 AND Heart_Attack_Risk = 0 THEN 1 ELSE 0 END)
            AS d FROM database_1.heart_attack_prediction_india_ag ) AS sub union
            SELECT 'Family_History' as 'variabile', a, b, c, d, ROUND((a * d) /
            (b * c), 3) AS odds_ratio FROM ( SELECT SUM(CASE WHEN Family_History
            = 1 AND Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS a, SUM(CASE WHEN
            Family_History = 1 AND Heart_Attack_Risk = 0 THEN 1 ELSE 0 END) AS
            b, SUM(CASE WHEN Family_History = 0 AND Heart_Attack_Risk = 1 THEN 1
            ELSE 0 END) AS c, SUM(CASE WHEN Family_History = 0 AND
            Heart_Attack_Risk = 0 THEN 1 ELSE 0 END) AS d FROM
            database_1.heart_attack_prediction_india_ag ) AS sub union SELECT
            'Obesity' as 'variabile', a, b, c, d, ROUND((a * d) / (b * c), 3) AS
            odds_ratio FROM ( SELECT SUM(CASE WHEN Obesity = 1 AND
            Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS a, SUM(CASE WHEN Obesity
            = 1 AND Heart_Attack_Risk = 0 THEN 1 ELSE 0 END) AS b, SUM(CASE WHEN
            Obesity = 0 AND Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS c,
            SUM(CASE WHEN Obesity = 0 AND Heart_Attack_Risk = 0 THEN 1 ELSE 0
            END) AS d FROM database_1.heart_attack_prediction_india_ag ) AS sub
            union SELECT 'Alcohol_Consumption' as 'variabile', a, b, c, d,
            ROUND((a * d) / (b * c), 3) AS odds_ratio FROM ( SELECT SUM(CASE
            WHEN Alcohol_Consumption = 1 AND Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS a, SUM(CASE WHEN Alcohol_Consumption = 1 AND
            Heart_Attack_Risk = 0 THEN 1 ELSE 0 END) AS b, SUM(CASE WHEN
            Alcohol_Consumption = 0 AND Heart_Attack_Risk = 1 THEN 1 ELSE 0 END)
            AS c, SUM(CASE WHEN Alcohol_Consumption = 0 AND Heart_Attack_Risk =
            0 THEN 1 ELSE 0 END) AS d FROM
            database_1.heart_attack_prediction_india_ag ) AS sub union SELECT
            'Air_Pollution_Exposure' as 'variabile', a, b, c, d, ROUND((a * d) /
            (b * c), 3) AS odds_ratio FROM ( SELECT SUM(CASE WHEN
            Air_Pollution_Exposure = 1 AND Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS a, SUM(CASE WHEN Air_Pollution_Exposure = 1 AND
            Heart_Attack_Risk = 0 THEN 1 ELSE 0 END) AS b, SUM(CASE WHEN
            Air_Pollution_Exposure = 0 AND Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS c, SUM(CASE WHEN Air_Pollution_Exposure = 0 AND
            Heart_Attack_Risk = 0 THEN 1 ELSE 0 END) AS d FROM
            database_1.heart_attack_prediction_india_ag ) AS sub union SELECT
            'Smoking' as 'variabile', a, b, c, d, ROUND((a * d) / (b * c), 3) AS
            odds_ratio FROM ( SELECT SUM(CASE WHEN Smoking = 1 AND
            Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS a, SUM(CASE WHEN Smoking
            = 1 AND Heart_Attack_Risk = 0 THEN 1 ELSE 0 END) AS b, SUM(CASE WHEN
            Smoking = 0 AND Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS c,
            SUM(CASE WHEN Smoking = 0 AND Heart_Attack_Risk = 0 THEN 1 ELSE 0
            END) AS d FROM database_1.heart_attack_prediction_india_ag ) AS sub
            union SELECT 'Healthcare_Access' as 'variabile', a, b, c, d,
            ROUND((a * d) / (b * c), 3) AS odds_ratio FROM ( SELECT SUM(CASE
            WHEN Healthcare_Access = 1 AND Heart_Attack_Risk = 1 THEN 1 ELSE 0
            END) AS a, SUM(CASE WHEN Healthcare_Access = 1 AND Heart_Attack_Risk
            = 0 THEN 1 ELSE 0 END) AS b, SUM(CASE WHEN Healthcare_Access = 0 AND
            Heart_Attack_Risk = 1 THEN 1 ELSE 0 END) AS c, SUM(CASE WHEN
            Healthcare_Access = 0 AND Heart_Attack_Risk = 0 THEN 1 ELSE 0 END)
            AS d FROM database_1.heart_attack_prediction_india_ag ) AS sub

```

| Variable Considered       | a    | b    | c    | d    | Odds Ratio |
|---------------------------|-----:|-----:|-----:|-----:|-----------:|
| Physical_Activity         | 1805 | 4153 | 1202 | 2840 | 1.027      |
| Family_History            | 939  | 2174 | 2068 | 4819 | 1.006      |
| Hypertension              | 740  | 1729 | 2267 | 5264 | 0.994      |
| Obesity                   | 909  | 2128 | 2098 | 4865 | 0.991      |
| Air_Pollution_Exposure    | 1199 | 2837 | 1808 | 4156 | 0.971      |
| Healthcare_Access         | 910  | 2200 | 2097 | 4793 | 0.945      |
| Smoking                   | 880  | 2134 | 2127 | 4859 | 0.942      |
| Diabetes                  | 267  | 662  | 2740 | 6331 | 0.932      |
| Alcohol_Consumption       | 1023 | 2505 | 1984 | 4488 | 0.924      |

The odds ratio is almost always close to 1.  
Therefore, no clear association emerges from this analysis.

# Final Odds Ratio chart with gradient:
<img width="1536" height="1024" alt="Image 6 apr 2026, 22_12_04" src="https://github.com/user-attachments/assets/5b65f5d5-cea3-425d-b823-4e09a368a89b" />


