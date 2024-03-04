# Choose right criteria for hypothesis testing.
Confirming hypotheses is an essential step in statistical data analysis. It assists in drawing conclusions based on observations.

The proper selection of a criteria directly impacts the quality of the study/test. Without understanding that the criteria is suitable for the chosen hypothesis and the selected data, we cannot conduct the testing.
### Hypothesis
Statistical hypothesis testing serves as a fundamental tool in drawing meaningful inferences from data. It involves formulating and assessing hypotheses to make informed conclusions about a population or dataset. The process revolves around two key hypotheses: the null hypothesis (H0) and the alternative hypothesis (H1 or Ha). Understanding these hypotheses is crucial, as it helps us navigate through the complexities of decision-making in statistical analysis.

1. **Null Hypothesis (H0)**: The null hypothesis is a statement formulated for examination. Typically asserting the absence of differences, relationships, or effects in data or a population, H0 might posit, for instance, that the mean value in two groups is identical or that two variables are not correlated.
2. **Alternative Hypothesis (H1 or H0)**: Serving as the counterpart to the null hypothesis, the alternative hypothesis suggests the presence of differences, relationships, or effects in data or a population. The alternative hypothesis can be **one-sided** (claiming the existence of only a positive or negative effect) or **two-sided** (asserting any effect different from zero).
3. **Type I and Type II Errors**: In the process of hypothesis testing, errors can occur. A **Type I error** (α error) happens when we reject the null hypothesis when it is true. On the other hand, a **Type II error** (β error) occurs when we fail to reject the null hypothesis when it is false. The probability of a Type I error is commonly denoted as α (significance level), while the probability of a Type II error is denoted as β. The concept of test power (1-β) is often used in conjunction with β. Understanding and managing these errors are crucial for the reliability of hypothesis testing outcomes.
4. Процедура проверки гипотез: 
    - Формулируется нулевая и альтернативная гипотезы.
    - Выбор статистического критерия, который соответствует типу данных и цели исследования.
    - Вычисляется статистическая мера (например, t-статистика или хи-квадрат и тд).
    - Затем сравнивается полученное значение статистики с критическим значением или вычисляется p-значение.

Steps to choose correctly criteria:

1. Type of metric?
    1. discrete
    2. countable
2. 

**p-значение**: p-значение -  вероятность получить результат, такой же или еще более экстремальный, чем наблюдаемый, при условии, что нулевая гипотеза верна. Если p < (α), то мы отвергаем **нулевую** гипотезу. Если p-значение больше уровня значимости, то нулевая гипотеза **не отвергается.**
   
```python
import numpy as np
from scipy import stats

# Группа с нормальным распределением
group = np.random.normal(loc=10, scale=2, size=100)

# Уровень значимости
alpha = 0.05

# Проверка нормальности распределения группы 
_, p_value = stats.shapiro(group)


if p_value > alpha:
    print("Группа имеет нормальное распределение")
else:
    print("Группа не имеет нормальное распределение")
```
