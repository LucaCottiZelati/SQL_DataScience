# Categorical Variable Analysis

## Population Study

The population analyzed in the `heart_attack_prediction_india` dataset is composed mainly of male subjects rather than female subjects.

### 1. Distribution by Gender

To examine the gender composition of the population, the following query was used:

```sql
SELECT COUNT(*) AS conteggio, gender
FROM database_1.heart_attack_prediction_india
GROUP BY gender
ORDER BY conteggio DESC;
```


| Count | Gender |
| ----: | ------ |
|  5516 | Male   |
|  4484 | Female |

This result shows a slight predominance of male subjects in the study population.

---
### 2. Age Distribution

The population is also distributed across a wide range of ages. To identify the most frequent ages in the dataset, the following query was executed:

```sql
SELECT COUNT(*) AS conteggio, age
FROM database_1.heart_attack_prediction_india
GROUP BY age
ORDER BY conteggio DESC
LIMIT 3;
```

| Count | Age |
| ----: | --: |
|   202 |  32 |
|   194 |  35 |
|   190 |  67 |

The most represented ages in the sample are therefore 32, 35, and 67 years.
---
### 3. Mean and Median Age

To better describe the age distribution of the patients, it is useful to compare the mean and the median, since together they provide a clearer view of the central tendency of the distribution.

Median Calculation

```sql
WITH counts AS (
    SELECT COUNT(*) AS total
    FROM database_1.heart_attack_prediction_india
),
median_rows AS (
    SELECT age, ROW_NUMBER() OVER (ORDER BY age) AS row_num
    FROM database_1.heart_attack_prediction_india
),
limits AS (
    SELECT 
        FLOOR((total + 1) / 2) AS start_row,
        FLOOR((total + 2) / 2) AS end_row
    FROM counts
)
SELECT ROUND(AVG(age), 1) AS median
FROM median_rows, limits
WHERE row_num BETWEEN start_row AND end_row;
```

| Median |
| -----: |
|   49.0 |

```sql
SELECT AVG(age) AS mean
FROM database_1.heart_attack_prediction_india;
```

| Mean |
| ---: |
| 49.4 |

The comparison between the mean (49.4) and the median (49.0) suggests that the age distribution is fairly balanced, with no strong asymmetry.

---
### 4. Population Split by Age Group

To simplify the analysis, the population was divided into two age groups:

Under_50
Over_50

The following query was used:
```sql
WITH tabella_age_group AS (
    SELECT *,
           CASE
               WHEN age >= 50 THEN 'Over_50'
               ELSE 'Under_50'
           END AS age_group
    FROM database_1.heart_attack_prediction_india
)
SELECT COUNT(*) AS conteggio, age_group
FROM tabella_age_group
GROUP BY age_group
ORDER BY conteggio DESC;
```

| Age_group | Count |
| --------- | ----: |
| Under_50  |  5073 |
| Over_50   |  4927 |

The population is therefore almost evenly distributed between subjects younger than 50 and subjects aged 50 or older.

For convenience, this new variable was stored in a copy table called heart_attack_prediction_india_ag.
```sql
CREATE TABLE database_1.heart_attack_prediction_india_ag AS
SELECT *,
       CASE
           WHEN age >= 50 THEN 'Over_50'
           ELSE 'Under_50'
       END AS age_group
FROM database_1.heart_attack_prediction_india;
```
---
### Case Study: Cardiac Risk in Diabetic Patients Over 50
Given the structure of the dataset, it is possible to further investigate the number of male and female subjects who present:

the presence or absence of a disease (diabetes, obesity, and so on);
the average level of variables such as triglycerides, stress level, and similar indicators.

One example of analysis is to study how many diabetic patients over 50 show a risk of heart attack.

### Diabetes Distribution in the Over_50 Population

To determine how many men and women over 50 have or do not have diabetes, the following query was used:
```sql
SELECT 
    main.gender,
    main.Diabetes,
    COUNT(*) AS conteggio,
    ROUND(COUNT(*) * 100.0 / gender_total.total, 2) AS percentuale
FROM database_1.heart_attack_prediction_india_ag AS main
INNER JOIN (
    SELECT gender, COUNT(*) AS total
    FROM database_1.heart_attack_prediction_india_ag
    WHERE age_group = 'Over_50'
    GROUP BY gender
) AS gender_total
    ON main.gender = gender_total.gender
WHERE main.age_group = 'Over_50'
GROUP BY main.gender, main.Diabetes, gender_total.total
ORDER BY main.gender, main.Diabetes;
```

| Gender | Diabetes | Count | Percentage |
| ------ | -------: | ----: | ---------: |
| Male   |        0 |  2440 |      90.30 |
| Male   |        1 |   262 |       9.70 |
| Female |        0 |  2013 |      90.47 |
| Female |        1 |   212 |       9.53 |

The percentages are very similar for both men and women over 50. In both groups, about 90% of the population does not have diabetes, while about 10% is diabetic.

### Heart Attack Risk in Diabetic Patients Over 50

A possible extension of the analysis is to compare diabetes status with the Heart_Attack_Risk variable, in order to observe the proportion of diabetic subjects who present a cardiac risk.

The following query makes it possible to calculate, for each gender, how many diabetic patients over 50 present heart attack risk:
```sql
SELECT 
    main.gender,
    main.Heart_Attack_Risk,
    COUNT(*) AS conteggio,
    ROUND(COUNT(*) * 100.0 / diabetici_per_gender.total_diabetici, 2) AS percentuale
FROM database_1.heart_attack_prediction_india_ag AS main
INNER JOIN (
    SELECT gender, COUNT(*) AS total_diabetici
    FROM database_1.heart_attack_prediction_india_ag
    WHERE age_group = 'Over_50'
      AND Diabetes = 1
    GROUP BY gender
) AS diabetici_per_gender
    ON main.gender = diabetici_per_gender.gender
WHERE main.age_group = 'Over_50'
  AND main.Diabetes = 1
  AND main.Heart_Attack_Risk = 1
GROUP BY main.gender, main.Heart_Attack_Risk, diabetici_per_gender.total_diabetici
ORDER BY main.gender;
```
| Gender | Heart_Attack_Risk | Count | Percentage |
| ------ | ----------------: | ----: | ---------: |
| Male   |                 1 |    73 |      27.86 |
| Female |                 1 |    56 |      26.42 |

Once again, the percentages are quite similar between the two genders. In particular:

27.86% of diabetic men over 50 present heart attack risk;
26.42% of diabetic women over 50 present heart attack risk.

---

### Final Considerations

This first analysis shows that:

- the dataset population is slightly skewed toward males;
- mean and median age are very close, suggesting a fairly regular age distribution;
- the population can be divided almost evenly into Under_50 and Over_50 groups;
- among Over_50 subjects, diabetes prevalence is very similar for men and women;
- the proportion of diabetic subjects with heart attack risk is also comparable across genders
