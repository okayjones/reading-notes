# Linear Regressions

Linear regression models relationship between two variables, but fitting a linear equasion to them.

`Scikit-learn` - Python module for machine learning
`sklearn.linear_model` - Includes methods for linear regression
`sklearn` - Includes data sets to try out

## Scikit Learn

Importing linear regression module
Y - Dependent variable
X - Independent variable

### Example

```python
from skilearn.linear_model import LinearRegression
X = bos.drop('PRICE', axis = 1) ##stores everything except price

# this created a LinearRegression object
lm = LinearRegression()
lm
```

### Functions

`lm.fit()` - fits a linear model
`lm.predict()` - predict Y using the linear model w/ estimated coefficients
`lm.score()` - Returns the coefficient of determination (R^2). A measure of how well observed outcomes are replicated by the model, as the proportion of total variation of outcomes explained by the model.
`lm.coef_` - Estimated coefficients
`lm.intercept` - Estimated intercept

## Fitting a Linear Model

Example

```python
#IN
lm.fit(X, bos.PRICE)

#OUT
LinearRegression(copy_X=True, fit_intercept=True, normalize=False)

#print intercept coefficient
print 'Estimated intercept coefficient:', lm.intercept_
#OUT -> 36.49

#print number of coefficients
print 'Number of coefficients:', len(lm.coef_)
#OUT -> 13


#Build Dataframe w/ features & coefficients
pd.DataFrame(zip(X.columns, lm.coef_), columns = ['features', 'estimatedCoefficients'])
#Higher coefficients mean higher relationship
```

Variable RM had highest coeffifient. Create scatter plot to show relationship between RM and price.

```python
plt.scatter(bos.RM, bos.PRICE)
plt.xlabel("Average number of rooms per dwelling (RM)")
plt.ylabel("Housing Price")
ply.title("Relationship between RM and Price")
plt.show()
```

## Prediction

`lm.predict()` - used to predict X

Example

```python
#IN, first 5 predictions
lm.predict(X)[0:5]

#OUT
array([1, 2, 3, 4, 5])

# Built scatter plot
plt.scatter(bos.PRICE, lm.predict(X))
plt.xlabel("Prices: $Y_i$")
plt.ylabel("Predicted prices: $\hat{Y}_i$")
plt.title("Prices vs Predicted Prices: $Y_i$ vs $\hat{Y}_i$")
```

Error in prediction, calculate mean squared error:

```python
mseFull = np.mean((box.PRICE - lm.predict(X)) ** 2)
print mseFull
# 21.89

# get PTRATIO to calculate mean squared error
lm = LinearRegression()
lm.fit(X[['PTRATIO']], box.PRICE)
# OUT: Linear Regression object

msePTRATIO = np.mean((bos.PRICE - lm.predict(X[['PTRATIO']])) ** 2)
print msePTRATIO
# 62.65

# MSE increased, which means a single feature is not a good predictor
```

## Training & Validation data sets

- Train model on training data
- See how it performs on test data
- Divide dataset randomly to do this w/ `train_test_split`

Example

```python
X_train, X_test, Y_train, Y_test = skilearn.cross_validation.train_test_split(X, bos.PRICE, test_size=0.33, random_state = 5)


lm = LinearRegression()
lm.fit(X_train, Y_train)
pred_train = lm.predict(X_train)
pred_test = lm.predict(X_test)
```
