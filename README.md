# Data Mining: Prediction & Classification (R)

Two supervised learning tasks in R for 161.324 Data Mining at Massey University (Assignment 2, 2026). Graded 34 / 50.

- [`analysis.Rmd`](analysis.Rmd) is the R Markdown source, with all code and notes.
- [`report.html`](report.html) is the knitted report, with output and plots. Download it to view it, since GitHub shows HTML as source.

To re-run it, open `analysis.Rmd` in RStudio and click Knit. The data is read straight from the course URLs, and the predictions are written to `predictions/`.

## Exercise 1: Property price per unit (regression)

Predict `price_per_unit` from sale time, property age, distance to transit, nearby stores and location.

| Model | Training RMSE |
| --- | --- |
| Linear regression (all predictors) | 9.25 |
| Stepwise linear regression (AIC, drops longitude) | 9.26 |
| Regression tree (rpart, cp = 0.01) | 7.18 |
| **Random forest** (mtry = 2) | **3.99** |

Random forest was used for the test predictions. On the held-out test set, as marked, it scored **RMSE 5.77**.

## Exercise 2: Wheat variety (classification)

Classify kernels as Canadian, Kama or Rosa from seven geometric measurements.

- One outlier found with `ggpairs` and jitter plots, then removed
- LDA assumptions checked. The size features (area, perimeter, length, width) are strongly collinear and the class covariances differ, so LDA, multinomial logistic and Naive Bayes were ruled out
- Compared LDA, two decision trees and a random forest using accuracy and ROC curves
- **Random forest** selected. Held-out test accuracy as marked: **0.963**

Size is the main split: Rosa kernels are the largest, Canadian the smallest and Kama in between. Canadian kernels have the highest asymmetry, which helps separate them from Kama. Rosa has the longest groove.

## Predictions

[`predictions/`](predictions) has the submitted test-set predictions: `.pred` for property, and `.pred_class` plus class probabilities for wheat.

## Marker feedback, and what I'd change

Most of the marks lost came from validation:

- **No train/validation split.** Every model was scored on the data it was fitted to, so the RMSE and accuracy figures above are training fit, not real performance. The random forest's 3.99 training RMSE against 5.77 on test shows the gap. Next time: `initial_split()` or cross-validation with `vfold_cv()`, and tune on the validation folds.
- **Thin EDA.** Check missing values, outliers and outcome-predictor relationships before modelling, not after.
- **Sanity-check final predictions** against the training distribution.
- **Outlier handling.** Consider correcting or downweighting it instead of dropping it.
- **Explaining the varieties.** Use grouped boxplots and summary statistics per variety, and explain the model actually used (random forest variable importance) rather than a decision tree.

## Data

The datasets are course material and are read from the course URLs in the report. They are not included here.

## Stack

R, tidyverse, tidymodels, yardstick, rpart, rpart.plot, randomForest, discrim, GGally, skimr
