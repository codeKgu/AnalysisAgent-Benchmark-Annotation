# Common Statistical Models

## Linear Regression

Assumes a linear relationship between the dependent variable and independent variable(s)

**Assumptions**: Linearity, independence, homoscedasticity, normality of residuals

**Equation**: $y = \beta_0 + \beta_1x_1 + \beta_2x_2 + ... + \beta_nx_n + \epsilon$

**Sample Code**:
```python
import statsmodels.api as sm

# Fit linear regression model
model = sm.OLS(y, sm.add_constant(X)).fit()

# Print model summary
print(model.summary())
```

## Linear Regression

Assumes a linear relationship between the dependent variable and independent variable(s)

**Assumptions**: Linearity, independence, homoscedasticity, normality of residuals

**Equation**: $y = \beta_0 + \beta_1x_1 + \beta_2x_2 + ... + \beta_nx_n + \epsilon$

**Sample Code**:

```python
import statsmodels.api as sm

# Fit linear regression model
model = sm.OLS(y, sm.add_constant(X)).fit()

# Print model summary
print(model.summary())
```

## Logistic Regression

Used for binary classification problems

**Assumptions**: Independence, linearity between log odds and predictors, no multicollinearity

**Equation**: $\log(\frac{p}{1-p}) = \beta_0 + \beta_1x_1 + \beta_2x_2 + ... + \beta_nx_n$

**Sample Code**:

```python
from sklearn.linear_model import LogisticRegression

# Fit logistic regression model
model = LogisticRegression()
model.fit(X, y)

# Make predictions
predictions = model.predict(X_new)
```

## Poisson Regression

Models count data or rates

**Assumptions**: Independence, mean equals variance, no overdispersion

**Equation**: $\log(\lambda) = \beta_0 + \beta_1x_1 + \beta_2x_2 + ... + \beta_nx_n$

**Sample Code**:

```python
import statsmodels.api as sm

# Fit Poisson regression model
model = sm.GLM(y, sm.add_constant(X), family=sm.families.Poisson()).fit()

# Print model summary
print(model.summary())
```

## Analysis of Variance (ANOVA)

Compares means across multiple groups

- One-way ANOVA: One categorical independent variable
- Two-way ANOVA: Two categorical independent variables

**Assumptions**: Independence, normality, homogeneity of variances

**Sample Code**:

```python
from scipy import stats

# Perform one-way ANOVA
f_statistic, p_value = stats.f_oneway(group1, group2, group3)

# Print results
print(f"F-statistic: {f_statistic}, p-value: {p_value}")
```



# Print model summary
cph.print_summary()
\`\`\`
```
