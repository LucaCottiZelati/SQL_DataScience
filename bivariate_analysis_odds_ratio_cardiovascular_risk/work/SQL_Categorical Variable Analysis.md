# Categorical Variable Analysis

## Population Study

The population analyzed in the `heart_attack_prediction_india` dataset is composed mainly of male subjects rather than female subjects.

### Distribution by Gender

To examine the gender composition of the population, the following query was used:

```sql
SELECT COUNT(*) AS conteggio, gender
FROM database_1.heart_attack_prediction_india
GROUP BY gender
ORDER BY conteggio DESC;

| Count | Gender |
| ----: | ------ |
|  5516 | Male   |
|  4484 | Female |

This result shows a slight predominance of male subjects in the study population.

---
### Age Distribution

The population is also distributed across a wide range of ages. To identify the most frequent ages in the dataset, the following query was executed:

```sql
SELECT COUNT(*) AS conteggio, age
FROM database_1.heart_attack_prediction_india
GROUP BY age
ORDER BY conteggio DESC
LIMIT 3;

| Count | Age |
| ----: | --: |
|   202 |  32 |
|   194 |  35 |
|   190 |  67 |
The most represented ages in the sample are therefore 32, 35, and 67 years.
---
### Mean and Median Age

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

| Median |
| -----: |
|   49.0 |

```sql
SELECT AVG(age) AS mean
FROM database_1.heart_attack_prediction_india;

| Mean |
| ---: |
| 49.4 |

The comparison between the mean (49.4) and the median (49.0) suggests that the age distribution is fairly balanced, with no strong asymmetry.

---
Population Split by Age Group

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
