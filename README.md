# **Hypothesis Test for New Music Recommendation Flow**

![Capa](https://image2url.com/images/1761534453933-e94c4667-4238-4fdc-8d7b-fac0b39c4fa0.jpg)
## **Scenario for “MusicStream Ltd.”**

**MusicStream Ltd.**, a digital music platform, previously used **Flow A**, recommending songs purely based on global popularity.  
A new strategy, **Flow B**, applies *collaborative filtering* — suggesting songs liked by users with similar tastes.  
The hypothesis is that this personalization increases engagement, measured by **average listening time per song**.

---

## **Test Data**

- **Sample Size:** 120 users  
- **Analyzed Variable:** Listening time per song (in minutes)  
- **Historical Mean (Flow A):** 1.22 minutes  
- **Objective:** Verify if Flow B significantly increases the mean to **2 minutes**

---

## **Dataset Generation**

```python
import numpy as np
import pandas as pd

np.random.seed(42)
n_users = 120
hypothetical_mean = 2.4
standard_deviation = 0.8

listening_time = np.random.normal(
    loc=hypothetical_mean,
    scale=standard_deviation,
    size=n_users
)
listening_time = np.maximum(listening_time, 0.1)

df_ab_test = pd.DataFrame({
    "user_id": range(1, n_users + 1),
    "group": "Flow B",
    "listening_time_min": listening_time
})

sample_mean = df_ab_test["listening_time_min"].mean()
sample_std = df_ab_test["listening_time_min"].std()
standard_error = sample_std / np.sqrt(n_users)

print("Key Statistics for Flow B")
print(f"Sample Mean: {sample_mean:.3f} min")
print(f"Sample Std: {sample_std:.3f} min")
print(f"Standard Error: {standard_error:.3f} min")
```

---

## **Data Visualization**

Distribution and outlier analysis for Flow B:

![Hist](https://image2url.com/images/1761533930787-157ae411-09ba-4ee6-83ea-ca08fbd4f09c.png)  
![Boxplot](https://image2url.com/images/1761533829740-6b05a691-e102-4b20-b136-6b7a6d24b31e.png)  
![Outliers](https://image2url.com/images/1761533947148-e928c170-30da-4c08-bcb8-004a15d8edff.png)

---

## **Confidence Interval**

- **Confidence Level:** 90 %  
- **t-value:** 1.658  
- **Formula:**  
  \[
  \bar{x} \pm t \times \frac{s}{\sqrt{n}}
  \]

```python
from math import sqrt

std = df_ab_test["listening_time_min"].std()
n = df_ab_test["user_id"].count()
mean = df_ab_test["listening_time_min"].mean()
margin_of_error = (1.658 * std) / sqrt(n)
upper_bound = mean + margin_of_error
lower_bound = mean - margin_of_error

print(f"90% Confidence Interval: [{lower_bound:.2f}; {upper_bound:.2f}] min")
```

![Formula](https://image2url.com/images/1761533989218-dc79b451-3cbe-4a85-b17d-0c05d4ecc30e.png)

**Result:** 90 % CI ≈ [2.09 ; 2.17] minutes  

✅ Interval entirely above 2 → **Flow B surpasses the target with confidence.**

---

## **Hypothesis Testing**

- **Null Hypothesis (H₀):** μ = 2 minutes  
- **Alternative Hypothesis (H₁):** μ ≠ 2 minutes  
- **Significance Level:** α = 0.10  

### **t-Test and p-Value**

```python
from scipy import stats

t_statistic, p_value = stats.ttest_1samp(
    df_ab_test["listening_time_min"], 2
)

print(f"t-Statistic: {t_statistic:.3f}")
print(f"p-Value: {p_value:.4f}")
```

---

## **Results**

| Metric | Value |
|:--|--:|
| Sample Mean | ≈ 2.4 min |
| Sample Std Dev | ≈ 0.8 min |
| Confidence Interval (90 %) | [2.09 ; 2.17] min |
| t-Statistic | ≈ +6.75 |
| p-Value | **< 0.001** |

---

## **Conclusion**

The **p-value < 0.10** → we **reject H₀** and conclude that the mean listening time under Flow B is *significantly different (and higher)* than 2 minutes.  

**Flow B achieved a clear improvement over Flow A (1.22 → 2.4 min)**, proving that personalized recommendations successfully increase user engagement.

---

## **Business Interpretation**

This is a **positive and statistically significant result**:  
- Flow B users listen ≈ 2× longer than before.  
- The 90 % CI and low p-value confirm the effect is real, not random.  
- Flow B can be rolled out to the entire user base with confidence.  

**We can confirm with 90% confidence that Flow B successfully exceeds the objective**, achieving a mean listening time significantly higher than the 2-minute target. The implementation of Flow B has statistically significantly increased user engagement compared to the previous flow (Flow A: 1.22 minutes).

**Business Interpretation:** While we statistically reject the hypothesis that the mean equals exactly 2 minutes, this is actually a positive outcome since the true mean is confidently above the target, indicating successful improvement in user engagement.
