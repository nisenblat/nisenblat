# Choose right criteria for hypothesis testing.
Confirming hypotheses is an essential step in statistical data analysis. It assists in drawing conclusions based on observations.

The proper selection of a criteria directly impacts the quality of the study/test. Without understanding that the criteria is suitable for the chosen hypothesis and the selected data, we cannot conduct the testing.

### Meaning of hypothesis testing.
Statistical hypothesis testing serves as a fundamental tool in drawing meaningful inferences from data. It involves formulating and assessing hypotheses to make informed conclusions about a population or dataset. The process revolves around two key hypotheses: the **null hypothesis (H0)** and the **alternative hypothesis (H1 or Ha)**. Understanding these hypotheses is crucial, as it helps us navigate through the complexities of decision-making in statistical analysis.

1. **Null Hypothesis (H0)**: The null hypothesis is a statement formulated for examination. Typically asserting the absence of differences, relationships, or effects in data or a population, H0 might posit, for instance, that the mean value in two groups is identical or that two variables are not correlated.
2. **Alternative Hypothesis (H1 or H0)**: Serving as the counterpart to the null hypothesis, the alternative hypothesis suggests the presence of differences, relationships, or effects in data or a population. The alternative hypothesis can be **one-sided** (claiming the existence of only a positive or negative effect) or **two-sided** (asserting any effect different from zero).
3. **Type I and Type II Errors**: In the process of hypothesis testing, errors can occur. A **Type I error** (α error) happens when we reject the null hypothesis when it is true. On the other hand, a **Type II error** (β error) occurs when we fail to reject the null hypothesis when it is false. The probability of a Type I error is commonly denoted as α (significance level), while the probability of a Type II error is denoted as β. The concept of test power (1-β) is often used in conjunction with β. Understanding and managing these errors are crucial for the reliability of hypothesis testing outcomes.
### Why is it importatnt to fit with the criteria for choosen hypothesis and data?

- Objectivity and reliability of results:
    - The correct choice of criterion allows us to eliminate subjective biases and distortions in the interpretation of research results.
- Statistical power:
    - The criterion affects the ability to detect real differences or effects in the data. Some criteria are more sensitive to detecting small but statistically significant differences, while others are less sensitive. The higher the power, the lower the chance of error.
- Considering data features:
    - Different criteria for different types of data. For example, for normally distributed data the Student's t test may be used, while for non-parametric data the Mann-Whitney test may be used. Choosing a criterion that matches the characteristics of the data allows you to obtain more accurate and reliable results.
- Application area:
    - Different hypothesis testing criteria are developed to address different types of questions and research purposes. For example, analysis of variance (ANOVA) compares means across three or more groups, while the chi-square test is used to test associations between categorical variables. Selecting a criterion appropriate to a specific application.


### Procedure for testing hypotheses:
- H1 or H0 hypotheses are formulated.
- Selecting a **statistical criteria** that is appropriate to the type of data and purpose of the study.
- A statistical measure is calculated (for example, t-statistic or chi-square, etc.).
- Then the obtained statistic value is compared with the critical value or the p-value is calculated.

### Procedure for choosing correctly criteria:

1. Type of metric?
    1. proportions
    2. countable
2. For

### **Metric selection scheme:**

Is the metric **quantitative** or **discrete**?

**Quantitative Metrics:**
- sums, averages, medians, and other numerical indicators.

**Proportional Metrics:**
- relative indicators such as percentages, ratios, and other measures expressed as proportions or fractions of a whole.
- categorical Variables, driver activity groups and their number (high, middle, low), age groups, groups by gender, groups based on types of cardholders, reasons for refusal to register documents, NPS

After choosing the type of metric, we need to take into account the requirements for the sample data.

### Quantitative + normal distribution:
independent:
- T-test for independent samples

dependent:
- T-test for paired samples

### Quantitative + abnormal distribution:
independent:
- Mann-Whitney

dependent:
- Wilcoxon

### Proportional
independent:
- Chi-square

### Quantitative

The criteria for quantitative metrics depend on **sample size** and **normality of distribution**.

It is necessary to check the normality of the sample distribution:

An example of confirming the normality of the distribution:

```python
import numpy as np
from scipy import stats

# Group with normal distribution
group = np.random.normal(loc=10, scale=2, size=100)

# Significance level
alpha = 0.05

# Checking the normality of the group distribution
_, p_value = stats.shapiro(group)

if p_value > alpha:
     print("The group has a normal distribution")
else:
     print("The group is not normally distributed")
```

An example of not confirming the normality of the distribution:

```python
import numpy as np
from scipy import stats

# Group
group = np.random.poisson(lam=5, size=100)

# Significance level
alpha = 0.05

# Checking the normal distribution of group 1
_, p_value = stats.shapiro(group)

if p_value > alpha:
     print("The group has a normal distribution")
else:
     print("The group is not normally distributed")
```

Next we will consider criteria that are suitable for testing hypotheses between two groups.

The sample has a **normal distribution** → we need to determine whether the metrics are dependent or not.

**Independent** metrics:

- two groups of different respondents (for example, like an AB test, two groups of respondents who do not depend on each other)

**Dependent** metrics:

- the same sample, metrics that are measured before and after the change

For independent samples with a normal distribution, we use the **Student's T-test** for independent samples.

The **T-test** will compare the mean values between groups. Let's calculate the required sample size:

```python
from statsmodels.stats.power import TTestIndPower

baseline_mean = 65 #current value average value in the sample
desired_mean = 67 #desired value
std = 51 #standard deviation, for t-test we can take this
#metric from a sample, not from the population
effect_size = (desired_mean - baseline_mean) / std
alpha = 0.05
power = 0.8

power_analysis = TTestIndPower()
sample_size = power_analysis.solve_power(
     effect_size=effect_size,
     power=power,
     alpha=alpha,
     alternative='two-sided')

print("Required sample size:", int(np.ceil(sample_size)))
```
For the **T-test** we also need to take into account the homogeneity of the sample:

- Homogeneity of variances assumes that the variances of the two groups are similar, that is, the spread of values in both groups is approximately the same.
- When this assumption is violated, when the variances differ significantly, the standard errors and therefore the t-statistic may be skewed. This may lead to an incorrect conclusion about the statistical significance of differences between group means.

Levin's test:

```python
from scipy.stats import levene

# Data from two groups
group1 = [1, 2, 3, 4, 5]
group2 = [1, 2, 3, 4, 5, 6]

# Checking homogeneity of variances
statistic, p_value = levene(group1, group2)

print("Test statistics:", statistic)
print("p-value:", p_value)
if p_value > 0.05:
     print("The sample is homogeneous")
else:
     print("The sample is not homogeneous")
```
We check the sample, taking into account the homogeneity value:

If the sample is **homogeneous**, we use the Student's t-test):

```python
from statsmodels.stats.weightstats import ttest_ind

group1 = [5, 7, 9, 8, 6] # first sample data
group2 = [3, 4, 6, 5, 7]

t_statistic, p_value, df = ttest_ind(group1, group2, alternative='two-sided')

print("t-statistic:", t_statistic)
print("p-value:", p_value)
```

If the sample is **not homogeneous**, use a test (Welch's t-test, add usevar='unequal'):

```python
from statsmodels.stats.weightstats import ttest_ind

group1 = [5, 7, 9, 8, 6] # first sample data
group2 = [3, 4, 6, 5, 7] # second sample data

t_statistic, p_value, df = ttest_ind(group1, group2, alternative='two-sided', usevar='unequal')

print("t-statistic:", t_statistic)
print("p-value:", p_value)
```

**For dependent variables:**
```python
from statsmodels.stats.power import TTestPower

baseline_mean = 65 #current value average value in the sample
desired_mean = 67 #desired value
std = 51 #standard deviation, for t-test we can take this
#metric from a sample, not from the population
effect_size = (desired_mean - baseline_mean) / std
alpha = 0.05
power = 0.8

power_analysis = TTestPower()
sample_size = power_analysis.solve_power(
     effect_size=effect_size,
     power=power,
     alpha=alpha,
     alternative='two-sided'
)
print("Required sample size:", int(np.ceil(sample_size)))
```

The paired t-test does not require a test for homogeneity since we are comparing the same samples.

```python
from scipy.stats import ttest_rel

group1 = [5, 7, 9, 8, 6] # first sample data
group2 = [3, 4, 6, 5, 7] # second sample data

t_statistic, p_value = ttest_rel(group1, group2)

print("t-statistic:", t_statistic)
print("p-value:", p_value)
```

### Additional nuances of the t-test:

- Sample size must be greater than 30
- With a large sample, the test is less sensitive to the normal distribution.
- The data should be homogeneous, if not, then Welch’s t-test is performed

### Non-parametric** metrics (not suitable for normal distribution):

For independents:

**Mann-Whitney test**

For addicts:

**Wilcoxon test**

```python
import numpy as np
from scipy.stats import mannwhitneyu, wilcoxon

# Set the data of two samples
import numpy as np

mean1 = 100
mean2 = 99
sample_size = 100

# Generate samples using Poisson distribution
group1 = np.random.poisson(lam=mean1, size=sample_size)
group2 = np.random.poisson(lam=mean2, size=sample_size)

# Add 1 to values to ensure they are positive
group1 += 1
group2 += 1

# Perform the Mann-Whitney test
statistic, p_value = mannwhitneyu(group1, group2, alternative='two-sided')

# Output Mann-Whitney results
print("Mann-Whitney U-statistic:", statistic)
print("p-value:", round(p_value, 5))

# Perform the Wilcoxon test
statistic, p_value = wilcoxon(group1, group2, alternative='two-sided')

# Output Wilcoxon results
print("Wilcoxon statistics:", statistic)
print("p-value:", round(p_value, 5))
```

### Discrete metrics.

Discrete independent values.

Chi-square.

```python
from statsmodels.stats.gof import chisquare_effectsize

probs0 = [30, 20, 10] # Probabilities for the null hypothesis
probs1 = [15, 25, 20] # Probabilities for the alternative hypothesis
combined_probs = [probs0, probs1]

effect_size = chisquare_effectsize(probs0, probs1)

print("Effect size:", effect_size)
from statsmodels.stats.power import GofChisquarePower

# Significance level
alpha = 0.05

# Desired power
power = 0.8

# Calculate required sample size
power_analysis = GofChisquarePower()
sample_size = power_analysis.solve_power(effect_size=effect_size, nobs=None, alpha=alpha, power=power)

print("Required sample size:", int(sample_size))
```
<aside>
💡 Calculation of criterion

</aside>

```python
import numpy as np
import scipy.stats as stats

# Set the observed frequencies
observed = np.array([[800, 750],
                      [200, 250]])

# Perform chi-square analysis
stat, p_value, dof, expected = stats.chi2_contingency(observed)

# Output results
print("Chi-square statistic:", stat)
print("p-value:", p_value)
```

harki bera test

if p_value > alpha:
     print("The group has a normal distribution")
else:
     print("The group is not normally distributed")
```
