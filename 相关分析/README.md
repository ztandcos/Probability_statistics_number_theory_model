# Restaurant Data Analysis Project

This repository showcases a comprehensive analysis of customer behavior and tipping patterns in a restaurant, utilizing a mathematical modeling approach to solve real-world problems. It includes source code, visualizations, and an accompanying research paper documenting the methodology and findings.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Data Analysis Objectives](#data-analysis-objectives)
- [Datasets](#datasets)
- [Research Paper](#research-paper)
- [Data Processing](#data-processing)
- [Methodology](#methodology)
- [Key Findings](#key-findings)
- [Technologies Used](#technologies-used)
- [Future Improvements](#future-improvements)
- [Acknowledgements](#acknowledgements)

---

## Project Overview

This project applies mathematical modeling and data analysis techniques to understand customer behavior and optimize restaurant operations. Using data collected over 244 days, the analysis focuses on uncovering patterns and relationships that can guide strategic decision-making.

### Highlights
- Includes a detailed research paper explaining the mathematical models used.
- Combines statistical analysis, hypothesis testing, and visualization.
- Provides actionable insights to improve customer satisfaction and restaurant performance.

---

## Data Analysis Objectives

1. **Analyze the Relationship Between Total Bill and Tip Amount**
   - Investigate correlation using Spearman's rank correlation.
   - Test for normality with the Jarque-Bera (JB) test.

2. **Evaluate Gender's Impact on Tipping Generosity**
   - Perform independent t-tests.
   - Visualize gender-based differences using box plots.

3. **Examine Daily Spending Patterns**
   - Analyze tipping trends across different days of the week.
   - Conduct statistical tests to identify significant variations.

4. **Assess Combined Effects of Smoking Habits and Gender**
   - Explore the interaction between smoking and gender on tipping behavior.
   - Identify trends and outliers with grouped box plots.

5. **Compare Tipping Behavior Between Meal Times**
   - Differentiate tipping patterns during Lunch and Dinner.
   - Highlight statistically significant differences.

---

## Datasets

This repository includes the following datasets for analysis:

1. **`tips.csv`**: The initial dataset with column names in English.
2. **`tipsz.csv`**: A modified version of the dataset with column names translated into Chinese for ease of use.
3. **`tipszz.csv`**: A processed dataset combining existing columns and adding a new column representing "Gender + Smoking Combination".

Each dataset is used in different stages of the analysis to perform hypothesis testing, visualization, and model building.

---

## Research Paper

The repository includes a well-documented research paper that delves into:
- The mathematical and statistical methods employed.
- Hypothesis formulation and testing.
- Comprehensive visualization and interpretation of results.
- Practical recommendations based on findings.

[Download the Research Paper](sandbox:/mnt/data/基于假设检验的餐馆数据相关分析.docx)

---

## Data Processing

1. **Initial Inspection**
   - Conducted exploratory data analysis to understand key attributes and their distributions.
   - Identified and handled missing data and outliers.

2. **Column Renaming**
   - Translated column names from English to Chinese for better interpretability.

3. **Data Transformation**
   - Created new variables, such as "Gender + Smoking Combination," for multi-factor analysis.
   - Normalized and scaled data where necessary for statistical testing.

---

## Methodology

- **Spearman's Rank Correlation**
  - Used to measure the strength and direction of the monotonic relationship between two variables.
  - Unlike Pearson's correlation, Spearman's correlation does not assume normality, making it ideal for non-parametric datasets.
  - Computed using:
    \[
    r_s = 1 - \frac{6 \sum d_i^2}{n(n^2 - 1)}
    \]
    where \( d_i \) is the difference between ranks of corresponding values in two datasets, and \( n \) is the sample size.

- **Statistical Tests**
  - Jarque-Bera test for normality verification.
  - Independent t-tests for mean comparison.

- **Visualization Techniques**
  - Scatter plots, box plots, and heatmaps for intuitive understanding.
  - Annotated figures highlighting significant findings.

- **Key Assumptions**
  - Customers act independently of each other.
  - Tipping reflects generosity and satisfaction.
  - Smoking habits and gender are constant during the study period.

---

## Key Findings

1. **Strong Positive Correlation Between Total Bill and Tip Amount**
   - Spearman's coefficient reveals a significant positive relationship.

2. **No Significant Gender-Based Differences**
   - While males occasionally tip higher in extreme cases, overall behavior shows no significant differences.

3. **Thursdays See the Highest Spending**
   - Significant differences identified in total spending compared to other days.

4. **Smoking Customers Tend to Tip More**
   - Both male and female smokers show higher generosity in extreme cases.

5. **Dinner Tips Exceed Lunch Tips**
   - Dinner tipping amounts are higher with greater variability and statistical significance.

---

## Technologies Used

- **Programming Language:** Python
- **Key Libraries:**
  - `Pandas` and `Numpy` for data manipulation
  - `Matplotlib` and `Seaborn` for advanced visualizations
  - `SciPy` for robust statistical testing

---

## Future Improvements

- Enhance the analysis by incorporating predictive models.
- Explore cross-location datasets for broader applicability.
- Leverage Pearson's correlation for datasets meeting normality assumptions.
- Apply advanced machine learning techniques for deeper insights.

---

## Acknowledgements

This repository combines mathematical modeling expertise and data analysis to address practical challenges in the restaurant industry. Special thanks to resources on statistical testing, visualization, and modeling methodologies.

---

Feel free to explore the repository, read the research paper, and contribute to further improvements!
