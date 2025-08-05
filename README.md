# A/B Test: Price Framing Effect on Subscription Conversion
Evaluate the impact of price framing on the subscription conversion rate for whitelist customers in an eCommerce platform. 
<br>⭐ **Link Google Collab:** [link](https://colab.research.google.com/drive/1V-Y_WHeZX7iO5JBziu-nHGcLxgPZDtyB?usp=sharing)

## :one: Project Overview:
**:round_pushpin: The main purpose of this project is:** to determine which promotional message—discounted price (99K) or discount amount (save 100K)—is more effective in increasing the subscription conversion rate among whitelist customers through an A/B testing experiment.

## :two: Dataset Description:
- **Dataset:**
    - customer_id: unique id of each users
    - group: group1: show screenA | group2: show screenB
    - is_buy: whether that user buy the subscription or not (0:no, 1:yes)
⭐ **Link Dataset:** [link](https://drive.google.com/file/d/1ogNLPiZzqM0Iqw6whE22gu3psJOT_iyf/view?usp=drive_link)
  
## :three: Tools used:
- Package used:
    - **pandas (processing data):** groupby(), agg(), tolist()
    - **statsmodel (statistics):** proportions_ztest()
## :four: Work flow:
1. Define Hypothesis Statement
2. Define Measurable Metrics
3. Define Audience
4. Conduct Experiment
5. Collect Result & Analyse

## :five: Python Code:
### 1. Define Hypothesis Statement
🌟 **Study Case:** An eCommerce business is offering a special promotion to a group of whitelisted customers for its membership subscription. Currently, these customers see Screen A, which displays the discounted price of the subscription (99K). However, the business has observed that the conversion rate for purchasing the subscription is lower than expected.

To improve performance, they propose testing an alternative banner design:

- Screen A (current version): Shows the discounted price of the subscription as 99K

- Screen B (proposed version): Highlights the discount amount, stating a 100K discount off the original price (199K)

📌 **Hypothesis statement:** "If we change the subscription banner from showing the discounted price (99K) to showing the discount amount (100K off), then conversion rate will increase"

### 2. Define Measurable Metrics:
- **Primary Metric:** Conversion Rate
- **Secondary Metric (optional):** Click-through rate (CTR), add-to-cart rate
- **Counter Metric (optional):** Refund or Cancellation Rate
<br>➡️ Due to limited data availability for secondary and counter metrics, the assessment will rely exclusively on the conversion rate.

### 3. Define Audience
- **Choose sample size:**
    - **Baseline Conversion Rate (group 1):** 34.6%
    - **Expected lift:** 5%
    - **Significance level:** α = 0.05 (CI=95%)
    - **Power: 90%**
    <br>➡️ **Minimum sample size per group is** 13.096 (calculated by ABTestGuide)
    <br>✅ **Current sample size per group is** 15.024 (Group A and Group B have the same number of participants)  

🖊️**Check the number of participants of each group:**
```
# Number of customers in group 1 and group 2
data['group'].value_counts()
```
➡️ **Output**
```

count
group	
1	15025
2	15024

dtype: int64

```

### 4. Conduct Experiment
- Show Screen A to Group 1, Screen B to Group 2
- Keep all other conditions constant (timing, devices, platforms, etc.)
- Run the test long enough to gather statistically significant results (2-3 weeks)

📌 **Assumption:** this A/B testing experiment was conducted in the perfect circumstances

### 5. Collect Result & Analyse
- **Calculate the Conversion Rate for each group**
    - Group 1: 34.6%
    - Group 2: 29%
- **Run statiscal test: two-proportion z-test (one-sided)**
    - H0: Conversion Rate of group 1 ≥ Conversion Rate of group 2
    - H1: Conversion Rate of group 1 < Conversion Rate of group 2
- **Interpret the results**
    - If p < 0.05 → Statistically significant improvement
    - If p ≥ 0.05 → no strong evidence of improvement
- **Make a decision**
    - If the treatment outperforms the control, implement the change
    - Otherwise, keep the original or iterate further

🖊️**Calculate the current conversion rate of group 1 and group 2**
```
purchases = data[(data['group'] == 1) & (data['is_buy'] == 1)].shape[0]
totalcustomers_group1 = data[data['group'] == 1].shape[0]
conversion_rate_group1 = purchases / totalcustomers_group1 * 100

purchases = data[(data['group'] == 2) & (data['is_buy'] == 1)].shape[0]
totalcustomers_group2 = data[data['group'] == 2].shape[0]
conversion_rate_group2 = purchases / totalcustomers_group2 * 100



print("Conversion Rate_Group 1:", conversion_rate_group1)
print("Conversion Rate_Group 2:", conversion_rate_group2)
```
➡️ **Output**
```
Conversion Rate_Group 1: 34.60232945091514
Conversion Rate_Group 2: 29.000266240681576
```

🖊️**Run a statistical test:**
- **Statistical test method:** two-proportion Z-test (one-sided)
```
import pandas as pd
from statsmodels.stats.proportion import proportions_ztest

# 1. Calculate the total participants who purchased ('sum') and total participants ('count')
group_counts = data.groupby('group')['is_buy'].agg(['sum', 'count'])

# 2. Assign variables
successes = group_counts['sum'].tolist()  # [total participants who purchased of group 1, total participants who purchased of group 2]
n_obs = group_counts['count'].tolist()    # [total participants of group 1, total participants of group 2]

# 3. Z-test: one-sided testing (whether group 1 < group 2)
stat, pval = proportions_ztest(successes, n_obs, alternative='smaller')

# 4. Print Z-statistic and P-value
print(f"Z-statistic: {stat:.4f}")
print(f"P-value (one-sided): {pval:.4f}")

# 5. Inclusion
alpha = 0.05
if pval < alpha:
    print("There is statistical evidence that Group 2 has a higher conversion rate than Group 1.")
else:
    print("There is not enough statistical evidence to conclude that Group 2 has a higher conversion rate than Group 1.")

```
➡️ **Output**
```
Z-statistic: 10.4261
P-value (one-sided): 1.0000
There is not enough statistical evidence to conclude that Group 2 has a higher conversion rate than Group 1.
```















