# Assignment 4: Interactive Statistical Analysis App

**Part 2 of 2 — Streamlit, Statistical Testing, and Dataset Extension**

---

## Overview

In this assignment, you will build the **Part 2** continuation of Assignment 3.

You will take the **Gold dataset** you created in Assignment 3, extend it with at least **one new external data source**, and build a **Streamlit app** that helps a user:

- follow a clear data story,
- explore the dataset,
- understand how the data was prepared,
- run multiple hypothesis tests,
- and interpret the results in plain language.

This assignment is not only about using statistical test functions correctly. It is also about showing that you can:

- tell a coherent analytical story with data and visuals,
- connect datasets in a meaningful way,
- justify why a new API or external source belongs in your project,
- choose appropriate tests,
- and explain what your results do and do not mean.

You should think of this assignment as a continuation of your original data story.

### Example directions

- if you used Binance and crypto sentiment in Assignment 3, you might ask whether **holidays**, **Google search interest**, **aggregated social sentiment**, **macroeconomic announcement days**, or **news sentiment** are associated with changes in returns or volume;
- if you used weather and air quality, you might ask whether **holidays**, **mobility**, **wildfire smoke data**, or **energy demand** help explain air-quality differences;
- if you used NASA events and weather, you might ask whether **seasonality**, **land use**, **air quality**, or **climate normals** add useful context to event patterns.

Your goal is to show that your original Gold dataset was a starting point, not the end of the project.

---

## Learning Goals

By the end of this assignment, you should be able to:

1. Extend an existing analysis-ready dataset with at least one new external source
2. Explain and defend a join strategy across multiple datasets
3. Build a Streamlit app for interactive data exploration
4. Select and run appropriate statistical tests based on variable types
5. Interpret test results in plain language
6. Reflect on assumptions, limitations, and possible sources of bias

---

## Core Requirement

You must continue from **your Assignment 3 project**.

Your Assignment 4 submission must:

1. Reuse your Assignment 3 Gold dataset as the foundation
2. Add at least **1 new API or external structured data source**
3. Create at least **1 new derived variable** enabled by that added source
4. Build a **Streamlit app**
5. Run and explain the required analyses

You may revise your pipeline from Assignment 3 if needed, but this should still clearly feel like the same project continued forward.

---

## New Data Source Requirement

In Assignment 4, you must add at least **one new external source** beyond the two required sources from Assignment 3.

This new source should help you ask a better question, create a stronger grouping variable, or introduce a useful categorical feature for testing.

Your new source does **not** need to be a live API if a clean public CSV or public dataset is more appropriate, but it must be:

- publicly accessible,
- clearly cited,
- relevant to your project question,
- and joined using a reasonable key such as `date`, `city`, `region`, or another documented identifier.

### What counts as a good extension?

A good extension does at least one of the following:

- adds explanatory context,
- creates a meaningful grouping variable,
- supports a categorical analysis,
- supports a new hypothesis,
- or strengthens the story behind your analysis.

### Example extensions

#### If your project is Crypto & Sentiment

- **Holiday calendar** joined by `date`
  - Example question: Do BTC returns, trading volume, or volatility differ on holidays versus non-holidays?
- **Google Trends** joined by `date`
  - Example question: Are higher search-interest days associated with larger trading volume?
- **Aggregated X or Reddit sentiment by day** joined by `date`
  - Example question: Are daily returns or trading volume associated with shifts in social sentiment around Bitcoin or Ethereum?
- **Macroeconomic calendar** joined by `date`
  - Example question: Are returns different on CPI or interest-rate announcement days?
- **News or social sentiment data** joined by `date`
  - Example question: Are returns different on high-negative-sentiment days?

#### If your project is Weather & Air Quality

- **Holiday calendar** joined by `date`
  - Example question: Is air quality different on holidays versus regular weekdays?
- **Mobility or traffic data** joined by `date` or location
  - Example question: Do higher-mobility days correspond to worse air quality?
- **Wildfire smoke or fire event data** joined by `date`
  - Example question: Are PM2.5 levels higher on wildfire-related days?
- **Energy demand data** joined by `date`
  - Example question: Do extreme temperature days align with higher pollution levels?

#### If your project is NASA Events & Weather

- **Holiday calendar** joined by `date`
  - Example question: Are event counts or reporting patterns different around holidays?
- **Air quality data** joined by `date`
  - Example question: Are wildfire event days associated with poorer air quality?
- **Climate normals** joined by `date` or season
  - Example question: Are event days associated with unusually high precipitation or temperature?
- **Regional population or land-use data** joined by region
  - Example question: Do some regions show different event patterns than others?

### Note on explanation

You are not required to prove causation.

However, you must explain:

- why you think the new source is relevant,
- what relationship you expect to see,
- and why that relationship may be associative rather than causal.

For example, in a crypto project, a holiday calendar does not mean the market closes. Crypto trades continuously. But holidays may still be associated with different trader behavior, volume, sentiment, or volatility.

---

## Statistical Testing Requirements

Your app and analysis must include at least the following:

1. **Two t-test-based analyses**
2. **One categorical count-based analysis from the chi-square family**
3. **One variance comparison**
4. **One correlation analysis**

For each required analysis, you must briefly justify:

- why the test is appropriate for your variables,
- whether its assumptions are reasonably satisfied,
- and what important limitations still remain.

### T-test requirement

Your two t-test analyses must include:

- at least **one one-sample t-test**
- at least **one two-sample or paired t-test**

Examples:
- Is mean daily BTC return different from `0`?
- Do PM2.5 levels differ between two cities?
- Do returns differ on holiday versus non-holiday days?
- Do temperature levels differ before and after a certain type of event?

### Categorical count-based analysis

Use a chi-square-family method when you are working with **categorical counts**.

Examples:
- Is `positive_return` independent of `holiday_flag`?
- Is `bad_air_day` independent of `season`?
- Is `event_day` independent of `rainy_day`?
- Are positive, negative, and flat-return days equally distributed?
- Are event days distributed evenly across seasons?

You may use either:

- a **chi-square test of independence** if you are comparing two categorical variables,
- or a **chi-square goodness-of-fit test** if you are comparing observed counts to an expected distribution.

If you use a goodness-of-fit test, you must clearly explain where your **expected proportions** came from.

### Variance comparison

Compare the **variability of two groups** using an F-test or another statistically justified variance-comparison method.

Examples:
- Is return variance different on holidays versus non-holidays?
- Is PM2.5 variability different between two cities?
- Is event-count variance different in rainy versus non-rainy periods?

### Correlation analysis

Examine the relationship between **two quantitative variables** using a correlation method that fits your data, such as Pearson or Spearman correlation.

Examples:
- Is Google search interest associated with BTC trading volume?
- Is daily social sentiment associated with crypto returns?
- Is temperature associated with PM2.5 concentration?
- Is precipitation associated with event counts?

### Technical note

Your dataset and app should be designed so that these analyses feel motivated by your data story, not forced onto unrelated variables.

You should be especially careful about the following:

- a paired t-test requires a meaningful natural pairing in the data,
- chi-square tests require categorical variables and reasonable expected cell counts,
- a chi-square goodness-of-fit test requires a clearly justified expected distribution,
- a variance-comparison test should match the distributional shape of your data,
- and your choice of correlation method should reflect the shape of the relationship, outliers, and whether a linear assumption is reasonable.

If one named method is not technically appropriate for your dataset, you may substitute a statistically justified alternative, but you must explain why the original method was inappropriate and why your substitute is better.

---

## Streamlit App Requirements

Build a Streamlit app that reads your final analysis-ready dataset and includes the following sections.

Your Streamlit app may read the final Gold dataset from local files stored in the repository. This is the recommended approach for most projects. You do **not** need to move your data into Google Cloud Storage for this assignment. If your dataset is unusually large or you have a clear project reason to use cloud storage, you may instead read the final dataset from Google Cloud Storage or another documented cloud source. If you choose this option, explain why it was appropriate for your project. Do not rely on app-generated local files to persist after deployment.

### Technical note on local files

For this assignment, the simplest and most appropriate pattern is to keep your final Gold dataset as a small file in your repository and have Streamlit read it directly.

For example, your app might read a file such as:

- `data/gold/final_dataset.csv`
- or `data/gold/final_dataset.parquet`

using normal Python code such as `pandas.read_csv()` or `pandas.read_parquet()`.

This works well for class projects because the app, the code, and the final analysis-ready dataset all live together in one place. Keep your final dataset reasonably small, clean, and intentional. Do not treat your Streamlit app like a full production data platform.

Even if you choose cloud storage, your app should still read from a final analysis-ready Gold dataset rather than calling raw APIs directly inside the Streamlit app.

Your dashboard should feel like a guided analytical story, not just a collection of widgets. A viewer should be able to understand the question, see the relevant patterns in the data, and then understand why each hypothesis test was chosen.

### 1. Project Overview / Data Story

This section should explain:

- your original Assignment 3 dataset,
- the new external source you added,
- your join key,
- and the main question or questions your project explores.

### 2. Data Preview

Show:

- a sample of the final dataset,
- summary statistics,
- column descriptions,
- and a short explanation of any important derived variables.

### 3. Visual Storytelling

Include at least:

- `2` strong charts for continuous variables,
- `1` chart for categorical or grouped data,
- `1` chart that supports your correlation analysis,
- and optional filters such as date range, city, category, or group.

Your charts should do more than display data. They should help motivate the hypotheses you test later in the app.

Good choices may include:

- time-series plots,
- grouped boxplots or violin plots,
- bar charts of category counts,
- scatterplots with a fitted trend line,
- and clearly labeled annotations or captions that explain why the chart matters.

### Examples of strong chart choices

- a time-series line chart of BTC returns, PM2.5, or event counts to show trend, seasonality, or unusual periods worth testing,
- a grouped boxplot of a continuous variable by group such as holiday vs non-holiday or city A vs city B to motivate a t-test,
- a grouped or stacked bar chart of category counts such as positive-return days by holiday flag to motivate a chi-square analysis,
- and a scatterplot of two quantitative variables such as Google Trends vs trading volume or temperature vs PM2.5 to motivate a correlation analysis.

### 4. Hypothesis Testing Section

Your app should allow a user to:

- move from the visual evidence to the formal test,
- select an analysis,
- see the null and alternative hypotheses,
- view the test statistic and p-value,
- view the variables being tested,
- and read a short justification for why the method fits the question,
- and read a short plain-language interpretation.

You may hard-code your required analyses or make them partially interactive. Either approach is acceptable if it is clear and usable.

### 5. Reflection / Limitations

Include a section that explains:

- important assumptions behind your tests,
- limitations of your data,
- possible join issues,
- and why statistical significance does not automatically imply practical importance or causation.

---

## Suggested Workflow

### Step 1 — Revisit Assignment 3

Start by reviewing your Assignment 3 Gold dataset and asking:

- What continuous variables do I already have?
- What binary or categorical variables do I already have?
- What variables am I missing if I want t-tests, chi-square analyses, correlation, or variance comparisons to make sense?
- What new source could help me create stronger comparisons?

### Step 2 — Add one new source

Bring in one additional source and document:

1. why you chose it,
2. how you joined it,
3. what cleaning was required,
4. and what new column(s) it enabled.

### Step 3 — Update your final dataset

Your final dataset should include:

- at least one continuous variable,
- at least two categorical or binary variables,
- and at least one grouping variable that supports multiple comparisons.

### Step 4 — Plan your required analyses

Before coding the app, write down:

- your null hypothesis,
- your alternative hypothesis,
- the variables used,
- the visualization that motivates the analysis,
- and why the selected method is appropriate.

### Step 5 — Build the Streamlit app

Build a clear, usable interface that:

- presents the data,
- runs the required analyses,
- and explains the outputs.

### Step 6 — Interpret carefully

Do not stop at reporting p-values.

For each analysis, explain:

- what the result suggests,
- whether the test assumptions are reasonable,
- and at least one caution or limitation.

---

## Deliverables

Your repository should include:

```text
your-repo/
├── README.md
├── requirements.txt
├── .env.example
├── data/
│   ├── bronze/
│   ├── silver/
│   └── gold/
├── ingest/
├── transform/
├── notebooks/
├── app/
│   └── streamlit_app.py
├── assignment4_analysis_plan.md
└── assignment4_reflection.md
```

### `assignment4_analysis_plan.md`

This file should describe:

1. the new source you added,
2. the join key,
3. the new variables you created,
4. the story or question your dashboard will tell,
5. the required analyses you plan to run,
6. the chart or visualization that supports each analysis,
7. and a short justification for each method.

### `assignment4_reflection.md`

This file should describe:

1. what worked well,
2. what was difficult,
3. what assumptions were hardest to defend,
4. and what you would improve if this project became a larger analytics product.

---

## Submission Format

Submit:

1. **A link to your GitHub repository**
2. **A link to one recorded demo video**

Your video must:

- be **15 minutes or less**
- include **facecam on**
- include **screen recording**
- include **audio narration**
- demonstrate the Streamlit app
- explain the new data source you added
- show at least one example of how the additional source changed or improved your analysis
- walk through the required analyses
- and explain at least one limitation or design tradeoff

---

## Demo Expectations

In your demo video, you should show and explain:

- the original Assignment 3 project and its Gold dataset
- the new source you added and why it matters
- how the new source was cleaned and joined
- at least one new derived variable created from that source
- how your Streamlit app is organized
- one example chart or filter
- how the dashboard tells a coherent story from visuals to analysis
- each required analysis at a high level
- one result interpreted in plain language
- one weakness, limitation, or assumption issue in your analysis

---

## Grading Emphasis

Your work should be evaluated primarily on:

1. Whether the project clearly continues and improves Assignment 3
2. Whether the new source is meaningfully connected to the analysis
3. Whether the join strategy and feature engineering are reasonable
4. Whether the statistical tests are appropriate and correctly interpreted
5. Whether the Streamlit app is clear, functional, and understandable
6. Whether the student demonstrates judgment rather than only producing output

---

## Tips for Success

- Choose a new source that creates a useful comparison, not just more columns.
- Use visualizations to make an argument, not just to fill space.
- Keep your join logic simple and explainable.
- Create categorical variables intentionally.
- Be careful not to confuse association with causation.
- Do not force a method onto variables that do not fit it.
- If a test requires assumptions, say so clearly.
- A smaller, well-justified app is better than a large, confusing one.

---

## Looking Back and Forward

Assignment 3 asked you to build a pipeline that could support future analysis.

Assignment 4 asks you to prove it.

By the end of this assignment, your project should show that you can move from:

- raw public data,
- to cleaned and joined datasets,
- to feature engineering,
- to statistical reasoning,
- to an interactive analytical product.

---

## Helpful Resources

If you are unsure how to structure your Streamlit app or how to read data into it, these official references are a good place to start:

- [Streamlit Community Cloud file organization](https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/file-organization)
- [Streamlit connecting to data](https://docs.streamlit.io/develop/concepts/connections/connecting-to-data)
- [Streamlit Google Cloud Storage tutorial](https://docs.streamlit.io/develop/tutorials/databases/gcs)
- [Streamlit static file serving and persistence notes](https://docs.streamlit.io/develop/concepts/configuration/static-file-serving)
