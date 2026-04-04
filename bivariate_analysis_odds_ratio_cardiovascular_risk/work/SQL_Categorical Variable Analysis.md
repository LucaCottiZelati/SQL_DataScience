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



