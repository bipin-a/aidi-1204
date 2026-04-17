# Assignment 5: Comparative Binary Classification with XGBoost

**Theme:** learning the machine learning process, not chasing a perfect score  
**Main model:** XGBoost  
**Main workflow tool:** Codex in VS Code  
**Deliverables:** new GitHub repository link, notebook, short summary, and 10-minute walkthrough video

---

## Overview

In this assignment, you will build a binary classification model that predicts whether a company is likely to go bankrupt.

You will use Codex to help you code, debug, and organize your work. The goal is not to prove that you are a perfect machine learning engineer. The goal is to learn the process that good machine learning projects follow:

- define the prediction problem;
- understand the target variable;
- split the data correctly;
- avoid data leakage;
- compare simple models against stronger models;
- choose a metric that makes sense for the problem;
- try a smaller feature set;
- pick a final model without cheating on the test set;
- explain whether the model is overfit.

This assignment is designed to be doable in a few focused days if you build one reusable evaluation function and use Codex effectively.

---

## Dataset

Use the workshop dataset:

- Repository: <https://github.com/bipin-a/ml-workshop/tree/main/training>
- File: `training/data.csv`
- Target column: `Bankrupt?`
- Positive class: `1`, meaning the company went bankrupt
- Approximate shape: 6,819 rows and 96 columns
- Approximate class balance: 220 bankrupt companies and 6,599 non-bankrupt companies, about 3.2% positive class

Use only `training/data.csv` for modeling.

Do not use:

- `model.json`
- `inference_test.csv`
- `catboost_info`
- any pretrained model artifact from the workshop repo

---

## Business Context

You are helping a risk team identify companies that may go bankrupt.

This is an imbalanced classification problem. Most companies do not go bankrupt, so accuracy can be misleading. A model that predicts "not bankrupt" almost every time can look accurate while still being useless.

For this assignment, assume this business rule:

```text
Missing a bankrupt company is worse than incorrectly flagging a healthy company.
```

In other words:

- false negative = predicted safe, actually bankrupt;
- false positive = predicted risky, actually not bankrupt;
- false negatives matter more.

You do not need to build a perfect cost model. You do need to choose a metric that respects this imbalance and risk.

---

## Learning Goals

By the end of this assignment, you should be able to:

1. Build a basic binary classification pipeline.
2. Explain why train, validation, and test splits are separate.
3. Explain why accuracy is a weak metric for imbalanced data.
4. Compare multiple models using the same evaluation process.
5. Use XGBoost as a strong model candidate.
6. Try a reduced feature set and explain the tradeoff.
7. Track overfitting using train and validation scores.
8. Select a final model using validation results, not test results.
9. Use Codex productively without blindly trusting it.

---

## Required Tools

You may use:

- pandas
- numpy
- scikit-learn
- xgboost
- catboost, optional
- matplotlib
- seaborn
- shap, optional

You may not use neural networks or deep learning models.

---

## Codex Requirement

You are expected to use Codex in VS Code while working on this assignment.

Good uses of Codex include:

- asking it to create a reusable evaluation function;
- asking it to help fix errors;
- asking it to generate plotting code;
- asking it to explain confusing model results;
- asking it to help simplify messy notebook cells;
- asking it to check whether you accidentally used the test set too early.

Bad uses of Codex include:

- submitting code you cannot explain;
- accepting a metric choice without thinking about the class imbalance;
- letting Codex tune on the test set;
- copying a final answer without checking that the notebook runs.

Your summary must include a short **AI Usage** section with 3 to 5 bullets:

- which AI tool you used;
- what it helped with;
- one thing you had to verify or fix yourself;
- one mistake or bad suggestion you caught, if any.

---

## Required Notebook Structure

Your notebook must run from top to bottom without errors.

Use this order.

### 1. Problem Definition

State:

- target variable;
- positive class;
- why the prediction task matters;
- whether the classes are balanced or imbalanced;
- your primary metric.

### 2. Quick EDA

Keep this short. You are not being graded on beautiful plots.

You must show:

- dataset shape;
- target distribution;
- missing value count;
- duplicate row count;
- at least one simple plot of the target distribution;
- one observation that affects your modeling choices.

Example observation:

```text
Only about 3.2% of rows are bankrupt, so I will not use accuracy as my primary metric.
```

### 3. Train, Validation, Test Split

Use this split:

| Split | Percent | Purpose |
|---|---:|---|
| Train | 70% | Fit models |
| Validation | 15% | Compare models and choose threshold |
| Test | 15% | Final check only |

Requirements:

- Use stratified sampling.
- Show the class balance in each split.
- Do not use the test set until the final section of the notebook.

This rule matters more than almost anything else in the assignment.

### 4. Preprocessing

Do the minimum preprocessing needed to make the models work.

You must:

- separate `X` and `y` correctly;
- remove the target from the feature matrix;
- identify whether the data has categorical features;
- handle missing values if any exist;
- scale only the models that need scaling, such as logistic regression, KNN, or SVM;
- briefly check for obvious leakage columns.

Most features in this dataset are already numeric, so do not overcomplicate this section.

### 5. Feature Sets

Create two feature sets.

| Feature set | Meaning |
|---|---|
| Feature Set A | All usable features after basic cleaning |
| Feature Set B | Smaller selected feature set |

Feature Set B can be simple. Choose one approach:

- keep the top 20 to 30 features from an XGBoost feature importance model;
- remove highly correlated duplicate-like features;
- remove constant or almost-constant features;
- use another reasonable feature selection method.

Explain in 3 to 5 bullets:

- how you selected Feature Set B;
- how many features were kept;
- whether performance got better, worse, or stayed similar;
- whether the smaller feature set is easier to explain.

Do not use the test set for feature selection.

---

## The 5 Required Experiments

You must run exactly 5 experiments.

The easiest way to finish this assignment is to write one function that trains a model, evaluates it, and adds one row to a results table.

Use the same train, validation, and test split for every experiment. The four XGBoost experiments should use the same cleaned training data, but vary the modeling choices.

Use these 5 experiments:

| # | Model | Feature set | Purpose |
|---:|---|---|---|
| 1 | Simple baseline | A | Logistic Regression or Dummy Classifier |
| 2 | XGBoost baseline | A | First serious model using all usable features |
| 3 | XGBoost with imbalance handling | A | Try `scale_pos_weight`, sample weights, or threshold tuning |
| 4 | XGBoost tuned lightly | A | Change 3 to 5 important hyperparameters |
| 5 | XGBoost selected features | B | Test whether fewer features help |

The four XGBoost experiments must vary meaningfully. Do not submit four runs that only change `n_estimators` by a small amount.

Across the XGBoost experiments, you must change at least two of these:

- number of selected features;
- class imbalance handling;
- probability threshold;
- `max_depth`;
- `learning_rate`;
- `n_estimators`;
- `subsample` or `colsample_bytree`;
- regularization such as `reg_alpha`, `reg_lambda`, or `gamma`.

### What Counts as Tuning?

Keep tuning small and realistic. You do not need a massive grid search.

For experiment 4, try one of these:

- a small `RandomizedSearchCV` with 5 to 10 candidates;
- a small manual search over 3 to 5 XGBoost configurations;
- a Codex-assisted search plan that you document clearly.

Do not tune on the test set.

---

## Required Metrics

You must choose one primary metric.

Because this dataset is imbalanced, **accuracy cannot be your primary metric**.

Recommended primary metrics:

- F2-score;
- recall, if you explain that false negatives are the bigger risk;
- PR-AUC;
- balanced accuracy;
- expected cost, if you want a challenge.

F2-score is the recommended default for this assignment because it values recall more than precision.

You must also report these secondary metrics for every model:

- precision;
- recall;
- F1-score;
- Brier score, if the model produces probabilities.

You may report accuracy as extra context, but it cannot drive model selection.

Brier score measures how good the predicted probabilities are, not just whether the final class label is right. Lower Brier score is better. It is useful here because a risk model should produce probabilities that are believable enough to support decisions.

### Metric Explanation

In your summary, explain your metric choice in 4 to 6 sentences.

Your explanation must mention:

- the positive class is rare;
- accuracy is misleading here;
- false negatives are more serious than false positives;
- why your metric fits that situation;
- what Brier score tells you that precision, recall, and F-score do not.

---

## Threshold Selection

For models that produce probabilities, you may adjust the classification threshold.

To keep this assignment manageable, choose one threshold strategy:

- use the default threshold of 0.5 and explain why it is a simple baseline;
- choose the threshold that gives the best F2-score on the validation set;
- choose a threshold that improves recall while keeping precision reasonable.

If you change the threshold, you must choose it using validation data only.

Use the same chosen threshold when evaluating the final model on the test set.

---

## Results Table

Your notebook and summary must include one table with exactly 5 rows.

Required columns:

| Column | Meaning |
|---|---|
| `exp_id` | Experiment number |
| `model` | Model name |
| `feature_set` | A or B |
| `main_settings` | Short model settings |
| `train_score` | Training primary metric |
| `val_score` | Validation primary metric |
| `overfit_gap` | `train_score - val_score` |
| `val_precision` | Validation precision |
| `val_recall` | Validation recall |
| `val_f1` | Validation F1-score |
| `val_brier` | Validation Brier score, if available |
| `selected_finalist` | Yes or no |
| `notes` | One short comment |

Optional but encouraged:

- `cv_score_mean`
- `cv_score_std`

Cross-validation is encouraged but not required. If you use 5-fold stratified CV, include the CV columns and mention it in your summary.

---

## Overfitting Check

For every experiment, calculate:

```text
overfit_gap = train_score - val_score
```

A large gap means the model may be memorizing the training data.

You must discuss overfitting for your final model.

Use this rough guide:

| Gap | Interpretation |
|---:|---|
| 0.00 to 0.05 | Usually fine |
| 0.05 to 0.10 | Watch carefully |
| Above 0.10 | Needs explanation or fixing |

You do not need to make the model perfect. You do need to notice when it is overfit.

---

## Final Model Selection Rule

Choose the final model using this order:

1. Best validation score on your primary metric.
2. Reasonable overfit gap.
3. Simpler model if two models perform similarly.
4. Test score only after the final model is chosen.

Do not pick the winner based on the test set.

In your summary, write 3 to 5 bullets explaining why your winner was selected.

---

## Final Test Evaluation

Only after selecting the final model, evaluate it on the test set.

Report:

- test score for your primary metric;
- test precision;
- test recall;
- test F1-score;
- test Brier score, if available;
- confusion matrix;
- final threshold, if you changed it;
- final model settings.

Include one final plot:

- precision-recall curve, preferred;
- or ROC curve, acceptable if you explain that PR curves are usually more useful for imbalanced data.

---

## Interpretability

Include one simple interpretation of the final model.

Choose one:

- XGBoost feature importance plot;
- permutation importance plot;
- SHAP summary plot, optional challenge.

Then write 3 bullets:

- top feature;
- whether it makes sense;
- one limitation of the interpretation.

SHAP is useful, but it is not required. Do not lose a day fighting SHAP installation issues.

---

## Deliverables

### 1. New GitHub Repository

Create a new GitHub repository for this assignment and submit the repository link.

The repository must include:

- your notebook;
- your results summary;
- a `README.md` with setup/run instructions;
- any helper scripts you created;
- a `.gitignore` that avoids committing large temporary files, virtual environments, caches, or secrets.

Your repo should be organized enough that another person can understand what to run.

### 2. Notebook

Submit a notebook named:

```text
lastname_firstname_assignment5.ipynb
```

The notebook must:

- run top-to-bottom without errors;
- use a fixed random seed;
- include the required sections;
- include the 5-row experiment table;
- keep the test set untouched until the final evaluation.

### 3. Results Summary

Submit a Markdown or PDF summary, maximum 3 pages.

Use jot notes. Do not write long paragraphs.

The summary must include:

- problem statement;
- dataset summary;
- class imbalance statement;
- metric choice explanation;
- feature selection explanation;
- 5-row experiment table;
- winning model and why it won;
- final test results;
- confusion matrix;
- feature importance result;
- overfitting discussion;
- AI Usage section.

### 4. Walkthrough Video

Submit a 10-minute maximum walkthrough video.

Show:

- your GitHub repository structure;
- your experiment table;
- your metric choice;
- your final model choice;
- your confusion matrix;
- your feature importance plot;
- one example of how Codex helped you.

Do not read your report word for word.

---

## Grading Rubric

| Area | Points | Full-credit evidence |
|---|---:|---|
| Problem definition | 7 | Target, positive class, imbalance, and business risk are clear |
| EDA | 7 | Short EDA with at least one modeling decision |
| Split and leakage control | 10 | Stratified 70/15/15 split and test set held until the end |
| Preprocessing | 7 | Sensible cleaning, scaling only when needed, target removed from features |
| Feature sets | 10 | Full feature set and selected feature set with explanation |
| 5 experiments | 15 | Exactly 5 experiments in one clear table |
| Metric choice | 14 | Primary metric fits imbalance; accuracy not used as main metric |
| Overfitting check | 10 | Train score, validation score, and overfit gap reported |
| Final test evaluation | 8 | Test metrics, confusion matrix, final settings |
| Interpretability | 5 | Feature importance or similar explanation |
| Codex usage | 4 | AI Usage section explains how Codex helped and what was verified |
| Repository, summary, and video | 3 | GitHub link, concise report, and clear walkthrough |
| Total | 100 |  |

---

## Recommended Workflow

1. Ask Codex to help create a clean notebook outline.
2. Load the data and inspect the target distribution.
3. Create the train, validation, and test split.
4. Write one reusable evaluation function.
5. Run the simple baseline first.
6. Add the XGBoost baseline.
7. Add imbalance handling and light tuning.
8. Create the selected feature set.
9. Fill the 5-row results table.
10. Pick the final model from validation results.
11. Evaluate once on the test set.
12. Write the summary in jot notes.

---

## Submission Checklist

Before submitting, verify:

- [ ] I created a new GitHub repository for this assignment.
- [ ] I included the GitHub repository link in my submission.
- [ ] I used only `training/data.csv` for modeling.
- [ ] I reported class balance in train, validation, and test.
- [ ] I used a stratified 70/15/15 split.
- [ ] I ran exactly 5 experiments.
- [ ] I included one 5-row comparison table.
- [ ] I chose a primary metric that is not accuracy.
- [ ] I explained why the metric fits the problem.
- [ ] I considered Brier score for probability quality.
- [ ] I reported train score, validation score, and overfit gap.
- [ ] I selected the final model before using the test set.
- [ ] I reported final test metrics.
- [ ] I included a confusion matrix.
- [ ] I included feature importance or similar interpretation.
- [ ] I included an AI Usage section.
- [ ] I recorded a walkthrough video of 10 minutes or less.
- [ ] My notebook runs top-to-bottom.
