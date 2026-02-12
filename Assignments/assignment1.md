# Assignment 1: Distributions Are Harder Than They Look

*Real data doesn't follow textbook rules. This assignment will show you why.*

---

## Overview

You will work with the Titanic dataset—a real, messy dataset with missing values, mixed data types, and distributions that will challenge your assumptions. By the end, you'll understand why statistics requires interpretation, not just calculation.

**Time estimate:** 4–5 hours


---

## Learning Objectives

After completing this assignment, you will understand:

- How missing data affects your analysis
- When summary statistics are trustworthy—and when they lie
- How to choose appropriate visualizations for different data types
- Multiple ways to detect unusual values—and why they disagree
- When and why to transform variables
- The difference between correlation and covariance

---

## Dataset

Use the Titanic dataset, available directly through seaborn:

```python
import seaborn as sns
df = sns.load_dataset('titanic')
```

This dataset contains passenger information from the Titanic, including whether they survived, their age, fare paid, and more. Crucially, it has real missing values—not artificially clean data.

---

# Part 0 — Setup (5 points)

Create a new Jupyter notebook. In your first cell, import the libraries you'll need. We've given you the starting point:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Load the data
df = sns.load_dataset('titanic')
```

**Your task:** Add any additional imports you discover you need as you work through the assignment.

---

# Part 1 — First Contact With Messy Data (15 points)

Before computing any statistics, you need to understand what you're working with.

### Tasks

1. Display the shape of the dataset, the first 10 rows, and the column data types
2. For each column, count how many values are missing. Display this as a table or series showing only columns with missing data.
3. Identify which columns are categorical, which are numeric, and which are "numeric but not really continuous" (think about what the numbers actually represent)


### Questions (answer in markdown cells)

**Q1.1:** The `age` column has missing values. If you simply deleted all rows with missing ages, what percentage of your data would you lose? Is that acceptable?

**Q1.2:** Look at the `pclass` column. It contains the values 1, 2, and 3. Is this truly a numeric variable, or is it something else? Explain your reasoning.

**Q1.3:** The `embarked` column has only 2 missing values out of 891. The `deck` column has far more. Which missing data problem is worse, and why?

> *Hint: Use `.info()`, `.isnull().sum()`, and `.nunique()` to explore.*

---

# Part 2 — Describing a Typical Passenger (15 points)

**Your goal is to describe what a "typical" Titanic passenger looked like in terms of age, fare paid, and family size.**

### Tasks

1. For `age`, `fare`, and `sibsp` (number of siblings/spouses aboard), calculate summary statistics that help describe the typical value and the spread.
2. For `fare`, try describing the "typical" fare in multiple ways. Compare the results.

### Questions

**Q2.1:** You calculated multiple summary statistics. For each of the three variables, which single number would you report as "typical"? Justify each choice—they may require different approaches.

**Q2.2:** In plain English, what does the standard deviation of `age` actually tell you about the passengers? Don't give the formula, explain what it means.

**Q2.3:** Variance is measured in "squared units." If age is in years, what unit is the variance in? Why is this awkward for interpretation?

**Q2.4:** If you calculated the mean fare with and without the most expensive tickets, did the "typical passenger fare" actually change, or just your statistic? What's the difference?

Note: This requires removing outliers. Hint use the `quantile()` in pandas.
Examples of summary statistics include: **Mean, Median, Mode, Std Dev, Variance**

---

# Part 3 — Visualizing the Passengers (20 points)

Numbers alone hide crucial information. Now you'll visualize what's actually happening.

### Tasks

Create visualizations use a simple `fig, axes = plt.subplots(3,1)` to show all 3 plots,Histograms and Box plot and KDE plot, for each of the questions. to understand the distributions of `age`, `fare`, `sibsp`, and how `age` varies across passenger classes. You'll need to figure out the appropriate seaborn functions and parameters yourself for each plot.

For each visualization, briefly note what you observe.

### Questions

**Q3.1:** Look at your `fare` visualization. Describe its shape. What summary statistic would you report to someone asking "how much did passengers typically pay?" Defend your choice using the plot.

**Q3.2:** You visualized `sibsp`. Did any visualization method produce strange or misleading results? If so, why did that happen, and what does it tell you about the variable?

**Q3.3:** If someone asked "what's the typical age on the Titanic?", would you give them a number or show them a plot? Defend your choice.

> *Note: seaborn documentation is your friend. Search for "seaborn histplot" and "seaborn boxplot".*

---

# Part 4 — Finding Unusual Values (15 points)

Some passengers look very different from the rest. But "unusual" depends on how you define it.

### Tasks

1. For the `fare` column, identify unusual values using the IQR method (values below Q1 - 1.5×IQR or above Q3 + 1.5×IQR). Count how many there are.
2. Now identify unusual values using z-scores (|z| > 3). Count how many.
3. Create a visualization that shows both the distribution and marks where these two methods disagree.
4. Calculate what percentage of your data each method flags.

### Questions

**Q4.1:** The two methods (IQR vs z-score) identified different numbers of unusual values. Which found more? Why do they disagree? (Think about what each method assumes about the data.)

**Q4.2:** What does a z-score of 4 actually mean in plain language?

---

# Part 5 — Fixing a Broken Distribution (10 points)

By now, you've seen that `fare` has a distribution that makes the mean misleading. Transformations can sometimes fix this—but not always.

### Tasks

1. Apply a **log transformation** to `fare`. Use `np.log1p()` which handles zero values safely.
2. Create side-by-side histograms: original fare vs. log-transformed fare.
3. Calculate the skewness of both versions (research how to do this in pandas or scipy).
4. Now apply the same log transformation to `age`. Create the same before/after comparison.

### Questions

**Q5.1:** What happened to the shape of `fare` after the log transformation? Why does this make the distribution more useful for analysis?

**Q5.2:** The log transformation helped `fare` but not `age`. Look at both original distributions—what's different about them that explains why the transformation only works for one?

**Q5.3:** After transforming `fare`, identify the outliers using the IQR method. Are they the same passengers as before the transformation? What does this tell you about how we define "unusual"?

---

# Part 6 — Relationships Between Variables (15 points)

Now you'll explore how variables relate to each other—and check your prediction from Part 1.

### Tasks

1. Compute the correlation matrix (use the `df.corr()` function ) for all numeric columns. Display as a heatmap.
2. Compute the covariance matrix for the same columns (use the `df.cov()` function ) . Display as a heatmap.
3. Create a scatter plot of `age` vs. `fare`, use the raw values. (**NOTE:** This scatterplot is what your `.corr()` function is trying to communicate)


### Questions


**Q6.1:** The covariance between `age` and `fare` is some number. The covariance between `survived` and `fare` is a different number. Can you compare these directly to determine which relationship is stronger? Explain.

**Q6.2:** Why is correlation easier to interpret than covariance?

**Q6.3:** Which numeric variable is most strongly correlated with survival? Does this make intuitive sense given what you know about the Titanic?


