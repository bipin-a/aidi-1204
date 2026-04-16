# Assignment 5: Comparative Binary Classification with XGBoost

**Theme:** production-grade model comparison for an imbalanced tabular classification problem  
**Primary model family:** XGBoost, compared against strong baselines  
**Deliverables:** Jupyter notebook, short results summary, and walkthrough video

---

## Overview

In this assignment, you will build a binary classification model that predicts whether a company is likely to go bankrupt. You will compare exactly 10 experiments, justify your metric, control overfitting, select a final model without using the test set, and explain which features matter.

The point of the assignment is not to get the highest possible score by trial and error. The point is to show that you can run a disciplined machine learning workflow:

- define the prediction task clearly;
- split data correctly;
- avoid target leakage;
- compare simple and complex models;
- choose a metric that matches the business problem;
- tune models without touching the test set;
- select a final model using validation evidence;
- explain whether the final model is overfit.

---

## Dataset

Use the workshop dataset:

- Repository: <https://github.com/bipin-a/ml-workshop/tree/main/training>
- File: `training/data.csv`
- Target column: `Bankrupt?`
- Positive class: `1`, meaning the company went bankrupt
- Approximate shape: 6,819 rows and 96 columns
- Approximate class balance: 220 bankrupt companies and 6,599 non-bankrupt companies, about 3.2% positive class

Use only `training/data.csv` for modeling. Do not use `model.json`, `inference_test.csv`, `catboost_info`, or any pretrained artifact from the workshop repository.

---

## Business Context

You are building a risk screening model for a lender or investment risk team. The model should identify companies at elevated bankruptcy risk before money is lent, invested, or extended as credit.

The classes are highly imbalanced, and the error costs are asymmetric:

- False negative: the model predicts safe, but the company goes bankrupt.
- False positive: the model predicts risky, but the company does not go bankrupt.

For this assignment, assume a false negative is about 7 times more costly than a false positive. This cost asymmetry must influence your metric choice and threshold choice.

Plain accuracy is not acceptable as the primary metric because a model can predict "not bankrupt" for almost every company and still look accurate.

---

## Learning Goals

By the end of this assignment, you should be able to:

1. Build a reproducible binary classification pipeline for tabular data.
2. Explain class imbalance and why accuracy can be misleading.
3. Compare multiple model families against a simple benchmark.
4. Use XGBoost with appropriate validation discipline.
5. Create and justify multiple feature sets.
6. Select a primary metric that matches the business risk.
7. Choose a classification threshold using validation data only.
8. Track overfitting using train, validation, test, and cross-validation results.
9. Interpret the final model using feature importance or SHAP.
10. Communicate results concisely in a decision-focused report.

---

## Required Tools

You may use:

- pandas
- numpy
- scikit-learn
- xgboost
- catboost
- matplotlib
- seaborn
- shap

You may not use neural networks or deep learning models.

---

## Required Notebook Structure

Your notebook must run top-to-bottom without errors and must use a fixed random seed.

Organize the notebook in this order.

### 1. Problem Definition

Clearly state:

- the target variable;
- what the positive class means;
- why bankruptcy prediction matters;
- whether the data is balanced or imbalanced;
- why the error costs are asymmetric;
- which metric you will use as the primary metric.

### 2. EDA

Keep EDA brief and decision-focused. Include no more than about 10 plots.

You must report:

- dataset shape;
- target distribution;
- missing values;
- duplicate rows, if any;
- numeric feature ranges or suspicious values;
- correlation or redundancy patterns;
- at least one EDA finding that changes your pipeline.

Do not include plots that do not influence a modeling decision.

### 3. Splitting Strategy

Create a three-way split:

| Split | Percent | Purpose |
|---|---:|---|
| Train | 70% | Model fitting and 5-fold CV |
| Validation | 15% | Model selection, threshold selection, early stopping |
| Test | 15% | Final one-time evaluation only |

Requirements:

- Use stratified sampling.
- Report class balance in train, validation, and test.
- Do not inspect test performance until the final model has been selected.
- Do not use the test set for feature selection, threshold selection, early stopping, or hyperparameter tuning.

### 4. Preprocessing

You must:

- identify numerical and categorical features, even if all features are numeric;
- handle missing values if present;
- scale features only for models that need scaling, such as logistic regression, KNN, or SVM;
- avoid scaling tree-based models unless you justify it;
- check for target leakage and suspicious features;
- build preprocessing in a reproducible way using pipelines where appropriate.

### 5. Feature Engineering and Feature Selection

You must create and justify at least two feature sets.

| Feature set | Requirement |
|---|---|
| Feature Set A | Full cleaned predictor set |
| Feature Set B | Reduced or selected predictor set |

Feature Set B must be selected using one or more of the following:

- correlation or redundancy analysis;
- model-based feature importance;
- permutation importance;
- mutual information;
- domain reasoning;
- removal of leakage-prone, constant, duplicate, or weak features.

You must explain:

- what you removed or transformed;
- why you removed or transformed it;
- whether performance improved or worsened;
- whether the selected feature set made the model simpler or more stable.

Feature selection must be done without using the test set.

---

## The 10 Required Experiments

You must run exactly 10 experiments.

| # | Model | Required role |
|---:|---|---|
| 1 | Dummy Classifier | Naive baseline, such as majority class or stratified random |
| 2 | Logistic Regression | Simple benchmark that every serious model should beat |
| 3 | Regularized Logistic Regression | Tune regularization and class weighting if appropriate |
| 4 | Decision Tree | Interpretable non-linear baseline |
| 5 | Random Forest | Bagged tree ensemble baseline |
| 6 | KNN or SVM | Distance or margin-based comparison model |
| 7 | CatBoost baseline | Boosting baseline with minimal tuning |
| 8 | XGBoost baseline | Default or lightly configured XGBoost |
| 9 | XGBoost tuned | Tuned hyperparameters with validation discipline |
| 10 | Selected-feature XGBoost | Tuned XGBoost using Feature Set B |

Rules:

- Experiments must be meaningfully different.
- At least one XGBoost experiment must use early stopping.
- CatBoost must use early stopping or a clear validation-based tuning process.
- At least one experiment must use Feature Set B.
- At least one experiment must address class imbalance using class weights, `scale_pos_weight`, thresholding, or another justified method.
- Hyperparameter tuning must be done using the training set and validation set only, never the test set.

---

## Cross-Validation Requirement

Use 5-fold stratified cross-validation on the training split for every experiment.

For each experiment, report:

- CV mean score;
- CV standard deviation;
- training score after fitting the experiment pipeline;
- validation score;
- overfit gap.

Use the same CV setup across experiments so the results are comparable.

Suggested definition:

```text
overfit_gap = train_primary_metric - validation_primary_metric
```

If your primary metric is a cost where lower is better, define and explain the gap carefully so the direction is clear.

---

## Metric Requirement

You must choose one primary metric and justify it.

Because the positive class is rare and false negatives are much more costly than false positives, plain accuracy is not acceptable as the primary metric.

Strong primary metric choices include:

- expected cost at a validation-selected threshold;
- F2-score or another F-beta score with beta greater than 1;
- recall at a fixed precision target;
- PR-AUC, if paired with a justified threshold rule for final classification.

Secondary metrics must include at least two of:

- precision;
- recall;
- F1-score;
- ROC-AUC;
- PR-AUC;
- balanced accuracy;
- expected cost.

You must defend your metric in about five concise sentences. A good defense references:

- the 3.2% positive class rate;
- the 7x false-negative cost assumption;
- why accuracy is misleading;
- whether your metric rewards recall, precision, ranking quality, or cost reduction;
- how your chosen threshold connects to the metric.

Hard rule: if plain accuracy is used as the primary metric, marks will be deducted.

---

## Threshold Selection

Do not blindly use a probability threshold of 0.5.

You must:

- use `predict_proba` or model scores where available;
- choose a threshold using the validation set only;
- explain the threshold rule;
- report the chosen threshold;
- use that threshold once on the final test set.

Acceptable threshold strategies include:

- threshold that minimizes expected cost on validation data;
- threshold that maximizes F2 on validation data;
- threshold that achieves a minimum precision while maximizing recall;
- threshold chosen from a precision-recall curve with a business justification.

---

## Required Results Table

Your notebook and summary must include one 10-row comparison table.

The table must include these columns:

| Column | Meaning |
|---|---|
| `exp_id` | Experiment number, 1 to 10 |
| `model` | Model name |
| `feature_set` | Feature Set A or Feature Set B |
| `key_preprocessing` | Scaling, imputation, encoding, imbalance handling |
| `key_hyperparameters` | Important settings only |
| `cv_score_mean` | 5-fold CV mean for primary metric |
| `cv_score_std` | 5-fold CV standard deviation |
| `train_score` | Training primary metric |
| `val_score` | Validation primary metric |
| `overfit_gap` | Train minus validation, or explained equivalent |
| `val_precision` | Validation precision at chosen threshold |
| `val_recall` | Validation recall at chosen threshold |
| `val_f1` | Validation F1 at chosen threshold |
| `selected_finalist` | Yes or no |
| `overfitting_notes` | Short comment |

This table is the center of the assignment. If the table is missing or incomplete, the submission will lose significant marks.

---

## Final Model Selection Rule

Choose the final model using this order:

1. Primary metric on validation data.
2. Overfit gap.
3. CV stability across folds.
4. Model simplicity and interpretability.
5. Test performance only after the final model has already been selected.

You may not pick the final model because it has the best test score.

A model should be treated as suspicious if:

- training performance is much higher than validation performance;
- validation performance is unstable across CV folds;
- validation performance drops sharply on the test set;
- feature selection was done using test data;
- hyperparameters were changed after viewing test results.

If the winning model has an overfit gap greater than 0.10 on the primary metric, you must either fix it or explicitly justify why it is acceptable.

---

## Final Test Evaluation

After selecting the final model, evaluate it once on the untouched test set.

Report:

- final test primary metric;
- test precision;
- test recall;
- test F1-score;
- test PR-AUC;
- test ROC-AUC;
- confusion matrix at the chosen threshold;
- classification report;
- final threshold;
- final hyperparameters;
- train, validation, and test primary metric side by side.

Include a precision-recall curve for the final model. A ROC curve is optional, but it cannot replace the precision-recall curve for this imbalanced problem.

---

## Interpretability Requirement

For the final model, include one of:

- SHAP summary plot for the top 10 features;
- permutation importance plot;
- XGBoost feature importance plot, if SHAP is not available.

Then explain in 2 to 4 concise jot notes:

- the top features;
- whether the important features make business sense;
- whether any important feature looks suspicious or leakage-prone;
- one limitation of the interpretation.

---

## Deliverables

### 1. Notebook

Submit a Jupyter notebook named:

```text
lastname_firstname_assignment5.ipynb
```

The notebook must:

- run top-to-bottom without errors;
- use a fixed random seed;
- contain the required sections in order;
- include the 10-row experiment table;
- keep test-set evaluation until the end.

### 2. Results Summary

Submit a Markdown or PDF summary, maximum 3 pages.

Use jot notes, not long paragraphs.

The summary must include:

- problem statement;
- dataset summary;
- class imbalance statement;
- metric choice and five-sentence defense;
- feature selection strategy;
- 10-row experiment table;
- winning model and why it won;
- final test results;
- confusion matrix;
- top 10 feature importance or SHAP result;
- overfitting discussion;
- top 3 things you would do next with another week.

### 3. Walkthrough Video

Submit an 8-minute maximum walkthrough video.

You must walk through:

- your metric choice;
- your experiment table;
- your final model selection logic;
- your threshold choice;
- your feature importance or SHAP plot.

Do not read the report word for word.

---

## Grading Rubric

| Area | Points | Full-credit evidence |
|---|---:|---|
| Problem definition and business framing | 8 | Target, positive class, imbalance, and cost asymmetry are clearly explained |
| EDA linked to decisions | 8 | EDA findings directly influence preprocessing, feature selection, or metric decisions |
| Split strategy and leakage control | 10 | Stratified 70/15/15 split, untouched test set, leakage checks documented |
| Preprocessing pipeline | 8 | Appropriate imputation, scaling, and model-specific preprocessing choices |
| Feature engineering and feature selection | 10 | Two feature sets, justified removals/transforms, no test-set leakage |
| 10 experiments | 14 | Exactly 10 meaningful experiments with complete comparison table |
| Cross-validation and tuning discipline | 10 | 5-fold stratified CV, consistent setup, validation-based tuning only |
| Metric and threshold choice | 15 | Primary metric fits imbalance and 7x false-negative cost; threshold justified |
| Overfitting discipline | 10 | Train/val/test scores, overfit gap, early stopping for boosting, gap discussed |
| Final evaluation and interpretability | 10 | Test metrics, PR curve, confusion matrix, SHAP or importance explanation |
| Results summary and video | 7 | Concise summary and clear video focused on decisions, not definitions |
| Total | 100 |  |

---

## Automatic Deductions

| Issue | Deduction |
|---|---:|
| No benchmark model | -10 |
| Fewer or more than 10 experiments | -10 |
| Trivial experiment variations | -10 |
| Plain accuracy used as the primary metric | -10 |
| No `overfit_gap` column | -5 |
| Test set used for tuning, feature selection, or threshold selection | -20 |
| No stratified split | -8 |
| No final confusion matrix | -5 |
| No PR curve for final model | -5 |
| No feature importance, permutation importance, or SHAP | -5 |
| Notebook does not run top-to-bottom | -15 |
| Video longer than 10 minutes or read word-for-word | -3 |

---

## Recommended Workflow

1. Load `training/data.csv` and confirm the target distribution.
2. Build the split function first and freeze the test set.
3. Build the metric and threshold-selection functions before modeling.
4. Run the dummy and logistic regression baselines.
5. Add tree-based models and boosting models.
6. Add cross-validation logging.
7. Create Feature Set B using training and validation evidence only.
8. Tune XGBoost with early stopping.
9. Select the final model using validation score, overfit gap, CV stability, and simplicity.
10. Evaluate the selected model once on the test set.

---

## Submission Checklist

Before submitting, verify:

- [ ] I used only `training/data.csv` for modeling.
- [ ] I reported the class balance in every split.
- [ ] I used stratified 70/15/15 splitting.
- [ ] I ran exactly 10 experiments.
- [ ] I included the required 10-row comparison table.
- [ ] I chose a primary metric that is not accuracy.
- [ ] I justified the metric using imbalance and false-negative cost.
- [ ] I selected a probability threshold using validation data only.
- [ ] I reported `overfit_gap` for every experiment.
- [ ] I used early stopping for at least one boosting model.
- [ ] I did not tune after seeing test performance.
- [ ] I included final test metrics only at the end.
- [ ] I included a final confusion matrix.
- [ ] I included a precision-recall curve.
- [ ] I included SHAP or another feature importance method.
- [ ] My notebook runs top-to-bottom without errors.
