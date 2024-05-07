Linear Regression

Assumes a linear relationship between the dependent variable and independent variable(s)
Assumptions: Linearity, independence, homoscedasticity, normality of residuals
Equation: $y = \beta_0 + \beta_1x_1 + \beta_2x_2 + ... + \beta_nx_n + \epsilon$

```python
import statsmodels.api as sm

# Fit linear regression model
model = sm.OLS(y, sm.add_constant(X)).fit()

# Print model summary
print(model.summary())
```
