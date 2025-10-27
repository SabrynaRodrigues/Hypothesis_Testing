# **Hypothesis Test for New Music Recommendation Strategy**

## **Scenario for "MusicStream Ltd." Company**

**MusicStream Ltd.**, a music streaming platform, currently uses **Flow A** for recommendations, which suggests music based solely on general popularity. The average listening time per song with this approach is historically **1.22 minutes**.

The product team has developed **Flow B**, a new strategy that recommends music based on users with similar tastes (collaborative filtering system). The hypothesis is that this personalization will increase user engagement.

## **A/B Test Data**

- **Sample size**: 1,200 users
- **Analyzed variable**: Listening time per song (in minutes)
- **Objective**: Verify if Flow B significantly increases the average listening time **to at least 2 minutes**

## **Generating the Simulated Dataset**

```python
import numpy as np
import pandas as pd

np.random.seed(42)
n_users = 1200
hypothetical_mean = 2.1
standard_deviation = 0.8

listening_time = np.random.normal(loc=hypothetical_mean, scale=standard_deviation, size=n_users)

listening_time = np.maximum(listening_time, 0.1)

df_ab_test = pd.DataFrame({
    'user_id': range(1, n_users + 1),
    'group': 'Flow B', 
    'listening_time_min': listening_time
})

df_ab_test.to_csv('music_streaming_recommendation_test_data.csv', index=False)

sample_mean = df_ab_test['listening_time_min'].mean()
sample_std = df_ab_test['listening_time_min'].std()
standard_error = sample_std / np.sqrt(n_users)

print("Key Statistics for Flow B")
print(f"Sample Mean: {sample_mean:.3f} minutes")
print(f"Sample Standard Deviation: {sample_std:.3f} minutes")
print(f"Standard Error: {standard_error:.3f} minutes")
print(f"Sample Size: {n_users} users")
```

## Data Visualization

- Histogram and boxplot generated to understand data distribution
- Identified outliers with listening times ≥ 4.5 minutes

```python
import seaborn as sns
sns.histplot(df['listening_time_min'])
```

![image.png](attachment:738133a7-335e-4939-a717-ad25b0605891:image.png)

```python
sns.boxplot(df['listening_time_min'])
```

![image.png](attachment:6fcc0ab6-7809-4647-b5d5-47e4588ffd7f:image.png)

```python
df_filtered = df.loc[df['listening_time_min']>= 4.5]
```

![image.png](attachment:32c70e86-0d29-49ce-bfcd-99230696e284:image.png)

## Confidence Interval Calculation

- **Confidence Level**: 90%
- **t-value**: 1.658 (from Student's t-distribution)
- **Margin of Error**: ±0.038 minutes
- **90% Confidence Interval**: [2.09; 2.17] minutes

![image.png](attachment:ff660c5f-e5c7-41dd-84c7-67516219f739:d585da01-3090-4e8e-ad63-0bda28bbcc81.png)

```python
from math import sqrt

std = df['listening_time_min'].std()
n = df['user_id'].count()
mean = df['listening_time_min'].mean()
margin_of_error = (1.658 * std) / sqrt(n)  # 1.658 from T-Student Table for 90%
upper_bound = mean + margin_of_error
lower_bound = mean - margin_of_error

print('Confidence Interval:')
print(f'[{lower_bound:.2f}; {upper_bound:.2f}]')
```

## Hypothesis Testing

- **Null Hypothesis (H0)**: Mean listening time = 2 minutes
- **Alternative Hypothesis (H1)**: Mean listening time ≠ 2 minutes

---

## Conclusion

The 90% confidence interval [2.09; 2.17] minutes does not contain the target value of 2 minutes. Therefore, we reject the null hypothesis with 90% confidence.

**We can confirm with 90% confidence that Flow B successfully exceeds the objective**, achieving a mean listening time significantly higher than the 2-minute target. The implementation of Flow B has statistically significantly increased user engagement compared to the previous flow (Flow A: 1.22 minutes).

**Business Interpretation:** While we statistically reject the hypothesis that the mean equals exactly 2 minutes, this is actually a positive outcome since the true mean is confidently above the target, indicating successful improvement in user engagement.
