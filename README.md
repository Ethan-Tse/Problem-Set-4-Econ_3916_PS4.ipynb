# Problem-Set-4-Econ_3916_PS4.ipynb
# PS4: The Model That Fit Too Well

ECON 3916 Problem Set 4. Why a revenue model with an R-squared of 0.94 over-forecast the biggest stores, and what its honest accuracy is.

## Objective
I simulated a store panel where the truth is known, reproduced the inherited model's R-squared of 0.94, found what that number hides, and rebuilt the model with an error estimate I can defend.

## Method
- Simulated 400 stores with 12 predictors: four real ones, seven irrelevant ones, and one that is nearly a combination of two others. Revenue flattens for the biggest stores and its spread grows with store size.
- Fitted OLS with every predictor on all the data, then on a training split, and compared in-sample and held-out R-squared, including a demonstration that extra noise columns never lower R-squared.
- Diagnosed the residuals (non-constant variance, a missed curve, influential stores), quantified the over-forecast by store size, and reported variance inflation factors.
- Ran a complexity sweep across eight models, chose by 5-fold cross-validation, and corrected the estimate for choosing with nested cross-validation.
- Ran a learning curve and checked it against the sweep, showed how the departing analyst's validation cell leaks, and wrote the handover note.

## Key findings
- The 0.94 reproduces: R-squared is 0.9439 with every predictor on all stores. Held-out R-squared averages 0.9383 over 200 splits, so the model was not over-fitted; the number simply says nothing about where the errors are.
- Residuals fan out with store size (Breusch-Pagan p = 3.3e-05) and bend: adding floor area squared has an HC3 t of -25.0 and lifts R-squared from 0.9439 to 0.9773.
- The largest fifth of stores is over-forecast by $98 thousand a store a month, 4.4% of what they earn.
- Variance inflation is 333.1 for the near-duplicate column. It makes the standard error of floor area 13.3 times larger and does not change the fit.
- Held-out error is lowest for the 12 predictors plus floor area squared. Nested cross-validation puts its RMSE at 81.7 $ thousand a month (rough interval 76.7 to 86.5).
- The learning curve agrees with the sweep: the old model has high bias (validation RMSE 125.7, a gap of only 5.1), and better features, not more stores, are the better investment (79.3 with floor area squared). A random forest gives a different picture (validation 97.0, gap 54.8), but it is a different model, and it is still worse than the straight line with one added feature.
- The analyst's validation cell scores 0.096 on pure noise; the same steps inside a Pipeline score -0.260, a gap of 0.356.
