# econ3916-lab14-ols-diagnostics
# OLS Diagnostics: When the Standard Error Is Lying

## Objective
I tested how heteroskedasticity, a wrong functional form and influential observations affect an OLS regression, and when robust standard errors do and do not fix the problem.

## Methodology
- Simulated 2,000 points on a line with a true slope of 2.0, with errors whose spread grows with x, and fitted the regression with classical and with robust (HC3) standard errors.
- Drew 400 simulated samples of 400 points from the same model and counted how often each 95% interval contained the true slope, with a Monte Carlo standard error for each coverage rate.
- Built four outcomes on the same values of x (well behaved, heteroskedastic, missing curvature, and one outlier), plotted residuals against fitted values, and ran Breusch-Pagan and Jarque-Bera tests beside each plot.
- Measured leverage, standardised residuals and Cook's distance for the outlier panel, then deleted the most influential row and fitted again.
- Regressed California Housing median house value on median income, house age, rooms per household and people per household, and compared classical and HC3 standard errors.
- Wrote a function, `hc3_se()`, that returns the HC3 standard error on one term for any table and list of features, and used it with and without the 965 districts at the 5.0 ceiling.

## Key Findings
- Heteroskedasticity left the slope alone and broke the standard error. Both fits gave a slope of 2.0147. The classical standard error was 0.0264 and the robust one 0.0290.
- The classical 95% interval covered the true slope in 91.8% of samples (Monte Carlo SE 1.4%), about 2.3 Monte Carlo SEs short of 95%. The robust interval covered it in 93.5% (Monte Carlo SE 1.2%), which is consistent with 95%.
- Each test checks one thing. Breusch-Pagan flagged the heteroskedastic panel but missed the missing-curvature panel (p = 0.7918), which the residual plot showed clearly.
- One row out of 600 mattered more for the standard error than for the slope. Deleting it moved the slope 0.58 of its own standard error, but the standard error fell 33%, because a huge residual enters the error variance squared.
- In California Housing, the HC3 standard error on AveRooms was 5.02 times the classical one.
- The HC3 standard error on AveRooms was 0.0121 with all districts and 0.0082 without the 965 at the ceiling. I did not treat the smaller number as a reason to drop them: they are real districts, not errors, and their capped values can affect the coefficient itself, which robust standard errors do not fix.
